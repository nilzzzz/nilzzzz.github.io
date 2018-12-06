---
layout: post
title: python爬虫基础之socket套接字
tag: python
---

### socket套接字
> TCP/IP协议中的TCP和UDP协议都通过套接字来实现网络功能。套接字是一种类文件对象，它使程序能接受客户端的连接或建立对客户端的连接，用以发送和接收数据。不论是客户端程序还是服务器端程序，为了进行网络通信，都要创建套接字对象。

 python提供了两个基本的 socket 模块。
 - 第一个是 Socket，它提供了标准的 BSD Sockets API。    
 - 第二个是 SocketServer，它提供了服务器中心类，可以简化网络服务器的开发

### Socket流程
![此处输入图片的描述][1]
![此处输入图片的描述][2]

### Socket 函数

 - 1）TCP发送数据时，已建立好TCP连接，所以不需要指定地址。UDP是面向无连接的，每次发送要指定是发给谁。
 - 2）服务端与客户端不能直接发送列表，元组，字典。需要字符串化repr(data)。
 

### socket编程思路
![此处输入图片的描述][3]
 
### 用socket建立基于TCP协议的服务器与客户端程序 
#### TCP服务端：

 - 1 创建套接字，绑定套接字到本地IP与端口   #socket.socket(socket.AF_INET,socket.SOCK_STREAM) , s.bind()
 - 2 开始监听连接   #s.listen() 
 - 3 进入循环，不断接受客户端的连接请求   #s.accept() 
 - 4 然后接收传来的数据，并发送给对方数据 #s.recv() , s.sendall() 
 - 5 传输完毕后，关闭套接字     #s.close()

#### TCP客户端:

 - 1 创建套接字，连接远端地址#socket.socket(socket.AF_INET,socket.SOCK_STREAM) , s.connect()
 - 2 连接后发送数据和接收数据          # s.sendall(), s.recv() 
 - 3 传输完毕后，关闭套接字          #s.close()

  [1]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/socket.png
  [2]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/socket1.png
  [3]: https://blog-1258233124.cos.ap-beijing.myqcloud.com/socket2.png
  
#### 用socket建立服务器端程序
  

> 在python标准库中，使用socket模块中提供的socket对象，就可以在计算机网络中建立服务器和客户端，并能够进行通信。服务器端需要建立一个socket对象，并等待客户端的连接。一旦连接成功，就可以进行通信了。


socket_server.py
```
import socket
HOST=''
PORT=10888
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.bind((HOST,PORT))
s.listen(1)
conn,addr=s.accept()
print('address:',addr)
while True:
    data=conn.recv(1024)
    if not data:
        break
    print('receive data:',data.decode('utf-8'))
    conn.send(data)
conn.close()
```

socket_client.py
```
import socket
HOST='localhost'
PORT=10888
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect((HOST,PORT))
data='hello'
while data:
    s.sendall(data.encode('utf-8'))
    data=s.recv(512)
    print('receive from server',data.decode('utf-8'))
    data=input('输入消息')
s.close()
```
### 用socket建立基于UDP协议的服务器与客户端程序

> 基于UDP协议的服务器与客户端在进行数据传送时，不是先建立连接，而是直接进行数据传送。

socket_udp_server.py
```
import socket
tmp=('127.0.0.1',3000)
s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
s.bind(tmp)
data=True
while data:
    data,address=s.recvfrom(1024)
    if data=='bye':
        break
    print('receive string:',data.decode('utf-8'))
    s.sendto(data,address)
s.close()
```


socket_udp_client.py
```
import socket
HOST='localhost'
PORT=3000
s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
data='hello'
while data:
    s.sendto(data.encode('utf-8'),(HOST,PORT))
    if data=='bye':
        break
    data,addr=s.recvfrom(512)
    print('receive from server:',data.decode('utf-8'))
    data=input('输入消息')
s.close()
```

### 用socketserver模块建立服务器

> socket模块可以创建服务器，但程序员要对包括网络连接等进行管理和编程。为了更加方便的创建网络服务器，python标准库中提供了一个创建网络服务器的框架--- socketserver。

> socketserver将处理请求划分为两个部分，为应用服务器类和请求处理类。服务器类处理通信问题，请求处理类处理数据的交换或传递。
> 
>  Socketserver模块可以简化网络服务器的编写，它包含了四种服务器类，TCPServer使用TCP协议，UDPServer使用UDP协议，还有两个不常使用的，即UnixStreamServer和UnixDatagramServer,这两个类仅仅在unix环境下有用
> 
>  使用服务器编程，需要进行以下几个步骤，首先建立一个请求句柄类，这个类继承自BaseRequestHandler类，建立这个类后重写它的handle方法，然后实例化服务器类，把主机名，端口号和句柄类传给它，然后调用server_forever()方法来处理请求。


socketserver.py
```
import socketserver
from socketserver import TCPServer,StreamRequestHandler
HOST=''
PORT=10888
class MyTcpHandler(socketserver.StreamRequestHandler):
    def handle(self):
        while True:
            data=self.request.recv(1024)
            if not data:
                server.shutdown()
                break
            print('receive data:',data.decode('utf-8'))
            self.request.send(data)
        return
server=TCPServer((HOST,PORT),MyTcpHandler)
server.serve_forever()
```
