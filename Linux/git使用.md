# 三大内容

> commit 提交
>
> repository 仓库
>
> branch 分支

# 如何下载？

git clone 地址

# 如何搜索？

> awesom xxx百科全书
>
> xx samle 例子
>
> xxx star/boilerplate 空项目/架子
>
> xxx twtorial 找教程

![img](https://i.loli.net/2021/08/08/RmGNUxkife9Bpb3.jpg)

- workspace：工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库

## 初始化仓库：

```shell
git init
```

![image-20210808213538388](https://i.loli.net/2021/08/08/YdFAVNthRGgyQk6.png)

![image-20210808213824712](https://i.loli.net/2021/08/08/oZJT64yvC81fqiU.png)

## 拷贝一份远程仓库：

```shell
git clone https://github.com.cnpmjs.org/tianqixin/runoob-git-test
```

## 添加文件到暂存区：

```shell
git add [file1] [file2] ...
```

## 添加暂存区内容到本地仓库：

```shell
git commit -m "message"
```

## 反应状态：

```shell
git status
```

如果是在工作区修改，也就是还没有add。

那git status会用红色标记你作出的修改。

![image-20210808101246760](https://i.loli.net/2021/08/08/I67Chib5F8BsUaM.png)

如果已经add，也就是修改已经提交到暂存区。

git status就会用绿色给你标记。

![image-20210808101313136](https://i.loli.net/2021/08/08/hdQAVbOxHfY7o84.png)

如果已经commit，也就是已经从暂存区添加到仓库中。

git status 就没有了。

![image-20210808101338406](https://i.loli.net/2021/08/08/ugHlxeRwUGjpKtQ.png)

比较暂存区与和工作区的不同：

```shell
git diff
```

如果是在工作区修改，也就是还没有add。

那git diff会用红色标记出你原来的样子，绿色标记出你作出的修改。

![image-20210808101657536](https://i.loli.net/2021/08/08/mXu5ZJpyU1AtMCS.png)

如果已经add，也就是修改已经提交到暂存区。

git diff就什么也没有了。

## 回退版本：

查看提交记录：

```shell
git log
git log --pretty=oneline
git log --oneline 不能显示未来
```

![image-20210808103217396](https://i.loli.net/2021/08/08/earLDwcjyHEXRod.png)

回退到上一个版本：

```shell
git reset HEAD^
```

回退到指定版本：

```shell
git reset 052e
```

回退到上上上个版本：

```shell
git reset --soft HEAD~3
```

回退时撤销工作区中所有未提交的修改内容，工作区与暂存区都回到上一个版本：

```shell
git reset --hard HEAD^
```

![image-20210808222635357](https://i.loli.net/2021/08/08/AzkDS4jmHV7oWbR.png)



回退版本后再回去：

1. 查看记录

```shell
git reflog
```

![image-20210808104450562](https://i.loli.net/2021/08/08/xfwK1tbiaXgQekE.png)

2. 找到add p这条记录的版本号

```shell
git reset --hard be87990
```

![image-20210808104729427](https://i.loli.net/2021/08/08/BpGIiEL3QWgjMOZ.png)

## 比较：

```shell
git diff 工作区与暂存区比较
git diff HEAD(^) 暂存区与本地库比较
```

## 撤消：

如果你在工作区写错了东西还没有add，想撤回就用

```shell
git checkout -- [file]
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

如果已经add了，先撤消add，再用checkout：

```shell
git reset HEAD [file]
git checkout -- [file]
```

![image-20210808103924517](https://i.loli.net/2021/08/08/mIlz59iBouwPkAq.png)

取消了add还可以commit，相当于add和commit一起执行了：

```shell
git commit -am "message"
```

![image-20210808104241585](https://i.loli.net/2021/08/08/ItybJ5PvcH1Qerm.png)

## 分支：

主分支master，HEAD指向master：

### 创建分支：

```shell
git branch dev
```

### 切换分支：

```shell
git checkout dev
git switch dev
```

上面两个可以合在一起：

```shell
git checkout -b dev 
git switch -c dev
```

### 查看分支：

```shell
git branck -v
```

### 合并分支：

git只能合并别的分支到当前分支

```shell
git merge dev 
```

### 删除分支：

```shell
git branch -d dev
```

### 分支冲突：

当两个分支都对一个文件的同一行作了修改并且commit，就有冲突。人为的去解决。再去：

```shell
git add 
git commit -m "message" (不能带文件名)
```

### 分支合并图：

```shell
git log --graph
git log --graph --pretty=oneline --abbrev-commit
```



## GitHub远程仓库

新建别名：把远程地址记录到本地在origin中

```shell
 git remote add origin https://github.com/little-zhengjj/learngit.git
```

![image-20210808225050445](https://i.loli.net/2021/08/08/3cMhrw1CjeVHWEF.png)

配个密钥：

```shell
 ssh-keygen -t rsa -C "zhengjj"
```

密钥位置：

```shell
cat ~/.ssh/id_rsa.pub
```

复制：

```shell
clip < ~/.ssh/id_rsa.pub
```

放到github里：

![image-20210809090827317](https://i.loli.net/2021/08/09/lwpM7JIiG1HjVqb.png)

上传：

```shell
git push origin master
```

