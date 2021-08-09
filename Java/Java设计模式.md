# 一、单例

## 1. 饿汉式（单例）

```java
public class Mine01 {
    public static final Mine01 INSTANCE = new Mine01();
    private Mine01(){
        System.out.println("Create the Instance!");
    }
    public static Mine01 getInstance(){
        return INSTANCE;
    }
    public static void main(String[] args) {
        Mine01 mine01 = Mine01.getInstance();
    }
}
```

缺点是类一加载对象就初始化了，不想用这个对象这就浪费了空间。

## 2. 懒汉式

### 不加锁（非单例）

```java
public class Mine02_lazy_loading {
    public static Mine02_lazy_loading INSTANCE;
    private Mine02_lazy_loading(){
    };
    public static Mine02_lazy_loading getInstance(){
        if (INSTANCE == null){
            try {
                Thread.sleep(1) ;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            INSTANCE = new Mine02_lazy_loading();
        }
        return INSTANCE;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Mine02_lazy_loading mine02_lazy_loading = Mine02_lazy_loading.getInstance();
                    System.out.println("Create the Instance!" + mine02_lazy_loading.hashCode());
                }
            }).start();
        }
    }
}
```

多个线程同时访问进入会出问题

### 方法加锁（单例）

```java
public class Mine02_lazy_loading {
    public static Mine02_lazy_loading INSTANCE;
    private Mine02_lazy_loading(){
    };
    public static synchronized Mine02_lazy_loading getInstance(){
        if (INSTANCE == null){
            try {
                Thread.sleep(1) ;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            INSTANCE = new Mine02_lazy_loading();
        }
        return INSTANCE;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Mine02_lazy_loading mine02_lazy_loading = Mine02_lazy_loading.getInstance();
                    System.out.println("Create the Instance!" + mine02_lazy_loading.hashCode());
                }
            }).start();
        }
    }
}
```

给static方法加锁，就是把Mine02_lazy_loading.class锁住，这种在方法上加锁的方式会使效率下降。

### 给需要的东西加锁（非单例）

```java
public class Mine02_lazy_loading {
    public static Mine02_lazy_loading INSTANCE;
    private Mine02_lazy_loading(){
    };
    public static Mine02_lazy_loading getInstance(){
        if (INSTANCE == null) {
            /*有可能多个线程进入这里*/
            /*一个线程用完就放锁释放了，下一个就进去了*/
            synchronized (Mine02_lazy_loading.class) {
                try {
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                INSTANCE = new Mine02_lazy_loading();
            }
        }
        return INSTANCE;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Mine02_lazy_loading mine02_lazy_loading = Mine02_lazy_loading.getInstance();
                    System.out.println("Create the Instance!" + mine02_lazy_loading.hashCode());
                }
            }).start();
        }
    }
}
```

### 双重检查（单例）

```java
package singleton;

public class Mine02_lazy_loading {
    public static Mine02_lazy_loading INSTANCE;

    private Mine02_lazy_loading() {
    }

    ;

    public static Mine02_lazy_loading getInstance() {
        /*双重检查*/
        if (INSTANCE == null) {
            synchronized (Mine02_lazy_loading.class) {
                if (INSTANCE == null) {
                    try {
                        Thread.sleep(1);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    INSTANCE = new Mine02_lazy_loading();
                }
            }
        }
        return INSTANCE;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Mine02_lazy_loading mine02_lazy_loading = Mine02_lazy_loading.getInstance();
                    System.out.println("Create the Instance!" + mine02_lazy_loading.hashCode());
                }
            }).start();
        }
    }
}
```

## 3. 静态内部类（单例）

```java
public class Mine03 {
    private Mine03(){ }
    private static class Mine03Holder{
        public static final Mine03 INSTANCE = new Mine03();
    }
    public static Mine03 getInstance(){
        return Mine03Holder.INSTANCE;
    }
    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            new Thread(()->{
                System.out.println(Mine03.getInstance().hashCode());
            }).start();
        }
    }
}
```

## 4. 枚举（单例）

```java
public enum Mine04 {
    INSTANCE;
    public static void main(String args[]){
        for (int i = 0; i < 100; i++) {
            new Thread(()->{
                System.out.println(Mine04.INSTANCE.hashCode());
            }).start();
        }
    }
}
```