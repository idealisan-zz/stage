## 在Linux中使用C语言系统套接字调用编写回声网络服务器

一直以来，我都只使用java编写网络程序，也用python写过爬虫，他们都可以非常方便的创建网络应用程序，但是它们都封装了系统的Socket套接字接口，无法学习系统的网络接口。

此处使用Linux的系统调用，在使用Linux系统调用后学习Windows的系统调用也会更简单。

一般来讲，网络服务器测试采用回声服务器可以快速测试，所以这里采用回声服务器。客户端采用简单的消息发送流程。

注意，C语言不是面向对象的编程语言，使用函数，过程更加繁琐。源代码采用了网络上一些代码，经过改写。

```
//服务器端
#include <unistd.h> 
#include <stdio.h> 
#include <sys/socket.h> 
#include <stdlib.h> 
#include <netinet/in.h> 
#include <string.h> 
//端口号
#define PORT 8080 

//启动服务器
int server(int argc, char const *argv[]) 
{ 
	int server_fd, new_socket, valread; 
	struct sockaddr_in address; 
	int opt = 1; 
	int addrlen = sizeof(address); 
	char buffer[1024] = {0}; 
	char *hello = "Hello from server"; 
	
	// 获得监听描述符，AF_INET表示IPV4，SOCK_STREAM表示TCP流，0我暂时不知道
	if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) 
	{ 
		perror("socket failed"); 
		exit(EXIT_FAILURE); 
	} 
	
	// 设置套接字选项
	if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
												&opt, sizeof(opt))) 
	{ 
		perror("setsockopt"); 
		exit(EXIT_FAILURE); 
	} 
	
	//把选项写入结构体
	address.sin_family = AF_INET; 
	address.sin_addr.s_addr = INADDR_ANY; 
	address.sin_port = htons( PORT ); 
	
	// 把描述符和地址绑定
	if (bind(server_fd, (struct sockaddr *)&address, 
								sizeof(address))<0) 
	{ 
		perror("bind failed"); 
		exit(EXIT_FAILURE); 
	} 
	//开始监听
	if (listen(server_fd, 3) < 0) 
	{ 
		perror("listen"); 
		exit(EXIT_FAILURE); 
	} 
	
	
	//循环监听，开启响应套接字
	while(1){
        if ((new_socket = accept(server_fd, (struct sockaddr *)&address, 
					(socklen_t*)&addrlen))<0) 
	{ 
		perror("accept"); 
		exit(EXIT_FAILURE); 
	} 
        //读取流内容
	valread = read( new_socket , buffer, 1024); 
    printf("%s\n",buffer ); 
        //发送流内容
	send(new_socket , hello , strlen(hello) , 0 ); 
	printf("Hello message sent\n"); 
    }
	return 0; 
} 

int main(int argc,char const * args[]){
    printf("Started.\n");
    int pid=fork();
    
    if(pid==0){
        //在子进程中监听
        server(0,NULL);
    }else{
        //主进程结束
        printf("Main process is ending.");
    }
    
    return 0;
}
// 客户端
#include <stdio.h> 
#include <sys/socket.h> 
#include <arpa/inet.h> 
#include <unistd.h> 
#include <string.h> 
#define PORT 8080 

int main(int argc, char const *argv[]) 
{ 
	int sock = 0, valread; 
	struct sockaddr_in serv_addr; 
	char *hello = "Hello from client"; 
	char buffer[1024] = {0}; 
        //创建IPV4套接字流
	if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) 
	{ 
		printf("\n Socket creation error \n"); 
		return -1; 
	} 

	serv_addr.sin_family = AF_INET; 
	serv_addr.sin_port = htons(PORT); 
	
	// 从字符串本地地址创建IPV4地址 
	if(inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr)<=0) 
	{ 
		printf("\nInvalid address/ Address not supported \n"); 
		return -1; 
	} 

        //连接套接字和地址
	if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) 
	{ 
		printf("\nConnection Failed \n"); 
		return -1; 
	} 
        //发送
	send(sock , hello , strlen(hello) , 0 ); 
	printf("Hello message sent\n");
        //读取 
	valread = read( sock , buffer, 1024); 
	printf("%s\n",buffer ); 
	return 0; 
} 
```

使用Fork可以创建后台进程，即便关闭控制台也可以继续运行。