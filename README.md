# SM2-2p

# 项目介绍

使用socket编程，实现网络通信上的SM2两方解密，并且实现两方共同签名

# 运行指导

首先找到我们代码的地址，进行复制C:\Users\legion\Desktop\....，然后打开命令行窗口输入cd进入文件夹


![image](https://user-images.githubusercontent.com/75195549/181517389-c15756b2-7932-4fa4-8d3b-6944c6014700.png)



随后输入py xxx.py运行程序：

![image](https://user-images.githubusercontent.com/75195549/181517516-71a86a7e-e426-41b0-930d-ca9f367dd206.png)


记住：先运行服务器端再运行客户端



# socket编程介绍


计算机网络通信中各种协议相当复杂，例如三次握手，累计确定，分组缓存，但是这些应该属于操作系统内核的部分，但是对于应用程序来讲，操作系统需要抽象出一个概念，让上层应用去编程，这个概念就是socket。

socket就像插座一样，客户端的插头插进服务器插座，就建立了连接

这个socket可以理解成（客户端ip，客户端port，服务器ip，服务器端port），ip用来区分不同主机，port端口用来区分主机中不同的进程。
socket是“open—write/read—close”模式的一种实现，那么socket就提供了这些操作对应的函数接口。



## 服务器伪代码
```
listenfd=socket（.....）；
bind（listenfd，本机的ip和知名端口，....）;
listen（listenfd，....）；
while（true）
{
    connfd=accept（listenfd，....）；
    receive（connfd，....）；//这里读取客户端send的数据
    send（connfd，....）；
}
```


## 客户端伪代码

```
clienfd=socekt（.....）;
connect（clienfd，服务器的ip和port，.....）；
send（cliend，数据）； //这里的发送就相当于上面的read（）
receive（clienfd，....）；
close（clienfd）
```

# 2p_dec实现

## 实验原理

第一步：生成A、B的私钥

![image](https://user-images.githubusercontent.com/75195549/181730652-17cba3b7-fc29-4e54-b7e5-d837e83da941.png)


第二步：A生成密文，传给B


![image](https://user-images.githubusercontent.com/75195549/181730732-a2ac7a74-54ab-4813-a7b8-4361269d2621.png)




第三步：B计算，并发给A


![image](https://user-images.githubusercontent.com/75195549/181730796-9b12b36f-5994-4c1b-9b8a-29046d97c3ca.png)




第四步：按照解密方式计算得到解密的消息



![image](https://user-images.githubusercontent.com/75195549/181730857-6b627157-af18-4edf-aaed-72daf0d0ff79.png)




## 代码分析

## SM21_2p_dec.py
#### Data receive

![image](https://user-images.githubusercontent.com/75195549/181522008-6cd28c11-1e89-477a-8573-ecdd2c772e92.png)


#### create k-value table


![image](https://user-images.githubusercontent.com/75195549/181522138-2296e947-4a33-49e5-a9d2-0a5248b8abeb.png)



#### compute h^ab

![image](https://user-images.githubusercontent.com/75195549/181522268-c36fc2a0-43cf-41b1-97a2-64c86ab60cea.png)


#### send H_ab and data set

![image](https://user-images.githubusercontent.com/75195549/181522379-16da104e-331d-47a9-a752-97961c3f777b.png)

## SM21_2p_dec.py
#### compute key-value

![image](https://user-images.githubusercontent.com/75195549/181522567-a254ded1-cc62-49a2-bf00-984fef899c7b.png)



#### send k and v



![image](https://user-images.githubusercontent.com/75195549/181522688-e0e1b70f-5cd3-48a3-acfa-0e5c6f6c38fc.png)


#### receive H_ab and data set S


![image](https://user-images.githubusercontent.com/75195549/181522786-2b385fa8-2980-4177-9bb5-f241a9b525a0.png)



#### detection


![image](https://user-images.githubusercontent.com/75195549/181522846-aebd8474-3d62-42a8-b14c-33e87207d9c4.png)






## 结果展示


![image](https://user-images.githubusercontent.com/75195549/181516557-bc8f58c1-98c0-4333-91d1-36e423374108.png)


