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
#### 私钥设置

![image](https://user-images.githubusercontent.com/75195549/181732206-cf967423-2e65-4278-b465-aa25aa800860.png)


#### 椭圆曲线基本参数设置


![image](https://user-images.githubusercontent.com/75195549/181732281-8d76085a-d943-42c9-a58c-fc9033b66503.png)



#### 函数uint_to_str

int类型转str类型


#### 基础左移右移以及运算操作

![image](https://user-images.githubusercontent.com/75195549/181732542-584d04b1-25e3-46b3-b35b-de7e028fccdd.png)

#### 函数add(x1,y1,x2,y2)
椭圆曲线加
#### 函数mul_add(x,y,k):
椭圆曲线乘

#### 基础socket编程


![image](https://user-images.githubusercontent.com/75195549/181732848-80a9c70e-7520-4b41-a21c-e4bd65702d5c.png)


## SM21_2p_dec.py
#### 接受到消息后进行运算


![image](https://user-images.githubusercontent.com/75195549/181733023-8b8116a7-6311-4202-81c5-3a2745c68902.png)



## 结果展示


![image](https://user-images.githubusercontent.com/75195549/181735600-f8ddae51-1aee-4a3c-83c5-891cd764ec85.png)






# 2p_sign实现

## 实验原理


![image](https://user-images.githubusercontent.com/75195549/181738035-ace63b96-4bc4-4c87-84ce-4c02e1eb8048.png)


首先A生成私钥的一个d1，计算P1，发送给B，B生成私钥的另一个d2,计算公钥P

A计算一个Z并计算e，和Q1发送给B，B收到后计算r，s2，s3，A即可利用r,s2,s3来输出签名

在该项目中，一方先准备建立连接，另一方接入，然后开始两方签名计算


## 代码分析
#### 子私钥 d1

![image](https://user-images.githubusercontent.com/75195549/181738367-df8ead0e-a9cf-4e5c-8e8b-f15f735363e9.png)

#### 计算P1
![image](https://user-images.githubusercontent.com/75195549/181738451-eedf4866-5ce1-418e-bfae-eadc1db123fa.png)


#### 计算ZA
![image](https://user-images.githubusercontent.com/75195549/181738554-19911558-217b-49c5-ac9e-3e1fc5ec534d.png)


#### 发送Q1,e
![image](https://user-images.githubusercontent.com/75195549/181738637-7f650fab-9cc2-474b-a3a0-be08237af8c1.png)

#### 接收r,s2,s3
![image](https://user-images.githubusercontent.com/75195549/181738690-d9eda4ae-bfab-4849-abc2-882ef86cfd7e.png)



#### 计算s

![image](https://user-images.githubusercontent.com/75195549/181738726-b3c89b05-c668-4beb-849c-293117554ee1.png)




#### 计算共享公钥P


![image](https://user-images.githubusercontent.com/75195549/181738845-94f782b2-6da3-404b-87c7-4944e995f43f.png)



## 结果展示

