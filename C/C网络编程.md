# 判断大小端

低地址存低位——小端

低地址存高位——大端

```c
#include <stdio.h>

int main(int argc, char const *argv[])
{
    int a = 0x12345678;
    printf("%#x\n", a);
    printf("%#x\n", (char)a);

    return 0;
}
0x12345678
0x78
```

很显然是小端存储。

网络协议指定了通讯字节序——大端，也就是说小端存储的0x12345678发过去会变成0x78563412。有些函数可以进行大小端的转换操作。

```c
host --> network
/*
将32位主机字节序数据转换成网络字节序数据
*/
uint32_t htonl(uint32_t hostint32);
uint16_t htons(uint16_t hostint16);
network --> host
uint32_t ntohl(uint32_t hostint32);
uint16_t ntohs(uint16_t hostint16);
```

# 地址转换函数

> 人类识别并输入给计算机的是字符串，而网络中用的是uint值，这个uint值在存储时还是按照字节存储。IPV4地址共32位，分为4字节，所以计算机中也是以4字节存储。

`inet_pton`person-->network

```c
/**
*头文件：#include <arpa/inet.h>
*功能：将点分十进制字符串转换成32位无符号整数
*参数：family 协议族 
			AF_INET IPV4网络协议
			AF_INET6 IPV6网络协议
	  strptr 点分十进制数串
	  addptr 32位无符号整数的地址
*返回值：成功返回1
*		失败返回其它
*/
int inet_pton(int family, const char *strptr, void *addrptr);
unsigned int inet_addr(const char* address);
```

```c
#include <stdio.h>
#include <arpa/inet.h>

int main(void)
{
    char ip_str[] = "123.123.123.123";
    unsigned int ip_int = 0;
    unsigned char *ip_p = NULL;

    inet_pton(AF_INET, ip_str, &ip_int);

    ip_p = (char *)&ip_int;
    printf("in_uint = %d.%d.%d.%d\n", *ip_p, *(ip_p + 1), *(ip_p + 2), *(ip_p + 3));

    return 0;
}
in_uint = 123.123.123.123
```

`inet_ntop`network-->person

```c
/**
*头文件：#include <arpa/inet.h>
*功能：将32位无符号整数转换成点分十进制数串
*参数：family 协议族
	  strptr 点分十进制数串
	  addptr 32位无符号整数
	  len strptr缓冲区长度
	  #define INET_ADDRSTRLEN 16 //for ipv4
	  #define INET6_ADDRSTRLEN 46 //for ipv6
*返回值：成功返回字符串首地址
*		失败返回
*/
const char *inet_ntop(int family, const void *addrptr, char *strptr, size_t len);
```

```c
#include <stdio.h>
#include <arpa/inet.h>

int main(void)
{
    unsigned char inet_int[] = { 
        123, 123, 123, 123 
    };  
    char inet_ptr[16] = ""; 

    inet_ntop(AF_INET, inet_int, inet_ptr, 16);
    puts(inet_ptr);
    return 0;
}
123.123.123.123
```

# 网络编程接口socket

> 提供不同主机上的过时程之间的通信，就是得到一种文件描述符。与IO流一样，对其进行读写。

socket分类：

1. SOCK_STREAM，流式套接字，用于TCP
2. SOCK_DGRAM，数据报套接字，用于UDP
3. SOCK_RAW，原始套接字，对于其他层次的协议操作时需要使用这个类型

# UDPC/S架构

![img](https://i.loli.net/2021/06/03/Q3UeYmMGCztHTxI.png)

流程：

服务器：

​	创建套接字socket()

​	将服务器的ip地址、端口号与套接字进行绑定bind()

​	接收数据recvfrom()

​	发送数据send()

客户端：

​	创建套接字socket()

​	发送数据sendto()

​	接收数据recvfrom()

​	关闭套接字close()

# socket(创建套接字)

```
man -f socket
man 7 socket
```

```c
/**
*头文件：#include <sys/socket.h>
*功能：创建一个用于网络通信的socket套接字(描述符)
*参数：family：通信域、协议族(AF_UNIX本地通信、AF_INET、AF_INET6、PF_PACKET底层接口等)
      type：套接字类(SOCK_STREAM、SOCK_DGRAM、SOCK_RAW等)
      protocol：协议类别(0、IPPROTO_TCP、IPPROTO_UDP等)
*返回值：套接字(文件描述符)
*/
int socket(int family, int type, int protocol);

SOCKET(7)                             Linux Programmer's Manual                             SOCKET(7)

NAME
       socket - Linux socket interface

SYNOPSIS
       #include <sys/socket.h>

       sockfd = socket(int socket_family, int socket_type, int protocol);

DESCRIPTION
       This manual(使用手册) page describes the Linux networking socket layer(层) user interface.  The BSD 		   compatible(兼容的) sockets are the uniform(一致) interface between the user process(处理) and the 	          network protocol stacks in  the  kernel(中心).   The  protocol  modules(单元)  are grouped into 	            protocol families such as AF_INET, AF_IPX, and AF_PACKET, and socket types such as SOCK_STREAM or            SOCK_DGRAM.  See socket(2)  for more information on families and types.
   Socket-layer functions
       These functions are used by the user process to send or receive packets and to do other socket
       operations.  For more information see their respective(各自的) manual pages.
```

**创建套接字时，系统不会分配端口**

```c
#include <stdio.h> 
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h> //socket

int main(void)
{
    //创建套接字
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1) 
    {   
        perror("fail to socket\n");
        exit(EXIT_FAILURE);
    }   
    printf("sockfd = %d\n", sockfd);
	return 0;
}
```

```c
#include <stdio.h> 
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h> //socket
#include <netinet/in.h> //sockaddr_in
#include <arpa/inet.h> //htons inet_addr
#include <unistd.h> //close
#include <string.h>

int main(void)
{
    //创建套接字
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket\n");
        exit(EXIT_FAILURE);
    }
    //打印文件描述符
    printf("sockfd = %d\n", sockfd);

    //填充服务器网络信息结构体 sockaddr_in
    struct sockaddr_in serveraddr;
    socklen_t addrlen = sizeof(struct sockaddr_in);

    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("192.168.40.1");
    serveraddr.sin_port = htons(8080);

    //发送数据
    char buffer[N] = "";
    while(1)
    {
        fgets(buffer, N - 1, stdin);
        buffer[strlen(buffer)] = '\0';
        //这里需要转换，sendto里用的是sockaddr通用套接字
        if (-1 == sendto(sockfd, buffer, N, 0, (struct sockaddr *)&serveraddr, addrlen))
        {
            perror("fail to sendto\n");
            exit(EXIT_FAILURE);
        }
    }
    close(sockfd);
    
    return 0;
}

```



# UDP编程-发送、绑定、接收数据

## IPv4套接字地址结构

头文件：#include <netinet/in.h>

```c
struct in_addr{
    in_addr_t s_addr; //4字节
};
//网络中常用的结构体
struct sockaddr_in
{
    sa_family sin_family; //2字节
    in_port_t sin_port; //2字节
    struct in_addr sin_addr; //4字节
    char sin_zero[8]; //8字节，填充
};
//通用套接字地址结构
struct sockaddr
{
    sa_family_t sa_family; //2字节
    char sa_data[4]; //14字节
};
```

## 两种不同结构地址的使用场合

1. 在定义源地址和目的地址结构的时候，选用struct sockaddr_in;
2. 当调用编程接口函数，且该函数需要传入地址结构时，需要用struct sockaddr进行强制类型转换。

sockaddr是先出现的，发现不够用，大宽泛了，所以就加了一个sockaddr_in。

```c
bind(sockfd, (struct sockaddr*)&my_addr, sizeof(my_addr));
```

## 发送数据——sendto函数

```c
/**
*头文件：#include <sys/socket.h> 
*功能：向to结构体指针中指定的ip，发送UDP数据
*参数：sockfd 套接字
	  buf 发送数据缓冲区
	  nbytes 发送数据缓冲区大小
	  flags 
	  	0 阻塞
	  	MSG_DONTWAIT 非阻塞
	  to 指向目的主机地址结构体的指针
	  addrlen to所指向内容的长度
*返回值：
*	  成功：发送数据的字符数
*	  失败：-1
*/
ssize_t sendto(int sockfd, const void *buf, size_t nbytes, int flags, const struct sockaddr *to, socklen_t addrlen);

```

## 向“网络调试助手”发送消息

## 绑定 bind函数

UDP网络程序想要收取数据需要什么条件？

​		确定的ip地址

​		确定的port

接收端：使用bind函数，来完成地址结构与socket套接字的绑定，这样ip、port就固定了

发送端：在sendto函数中指定接收端的ip、port，就可以发送数据了

bind绑定一般用于绑定服务器端。

```
/**
*头文件：#include <sys/socket.h> 
*功能：向to结构体指针中指定的ip，发送UDP数据
*参数：sockfd 套接字
	  buf 发送数据缓冲区
	  nbytes 发送数据缓冲区大小
	  flags 
	  	0 阻塞
	  	MSG_DONTWAIT 非阻塞
	  to 指向目的主机地址结构体的指针
	  addrlen to所指向内容的长度
*返回值：
*	  成功：发送数据的字符数
*	  失败：-1
*/
```



