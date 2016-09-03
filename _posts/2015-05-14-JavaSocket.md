---
layout: post
title: Socket和ServerSocket
categories: Java语言

---

>网络编程中常见的方式是利用Socket实现客户服务器模式。这种模式下，客户端需要主动创建用于服务器连接的Socket套接字，服务器端收到客户端的连接请求，也会创建用户客户端连接的Socket。Socket看作是通信连接两端的收发器，服务端和客户端都通过Socket来收发数据

<br/>

#### Socket的构造

Socket构造器有五种重载形式。具体代码如下：

```
Socket socket = new Socket();  
Socket socket = new Socket(InetAddress address, int port);  
Socket socket = new Socket(InetAddress address, int port, InetAddress localAddress, int localPort);  
Socket socket = new Socket(String host, int port);  
Socket socket = new Socket(String host, int port, InetAddress localAddress, int localPort);
```

在构造方法中可以设定服务器的IP地址，客户端的IP地址，以及服务器的监听端口、客户端的监听端口等参数。

可以注意到，构造方法中有一个无参的默认构造方法，它的用途在于连接特定的服务器之前，先设置Socket的一些选项，因为一旦与特定的IP绑定之后，这些Socket的选项参数就不可以改变了。具体的Socket选项参数在后面介绍。

<br/>

#### Socket连接时可能抛出的异常

客户端调用Socket构造方法请求连接服务器时，可能抛出的异常有几种：

* A、UnknownHostException：无法识别主机的名字和IO地址时抛出的异常
* B、ConnectException：没有服务器进程监听指定的端口或者是服务器进程拒绝连接时抛出的异常
* C、SocketTimeoutException：等待连接超时时抛出的异常
* D、BindException：无法把Socket对象与指定的本地IP地址或端口绑定时抛出的异常

<br/>

#### 获取Socket的信息的相关方法

```
socket.getInetAddress();//获得远程服务器的IP地址  
socket.getPort();//获取远程服务器的端口  
socket.getLocalAddress();//获取客户本地的IP地址  
socket.getLocalPort();//获取客户本地的端口号  
socket.getInputStream();//获得输入流  
socket.getOutputStream();//获得输出流  
```

根据这些常用的方法，Socket建立连接之后，最常用的配置是家里输出流的重定向和输入流的获取与缓存。

当需要向Socket对象中写入数据时，首先要定义一个输出流对象，让它指向Socket的输出流，然后调用输出流对象println方法或者是write方法来写入数据。

```
PrintWriter pw = new PrintWriter(socket.getOutputStream());//输出流对象设定为Socket的输出流  
//pw.write();  
//pw.print();  
pw.println();  
```

当需要从socket 对象中读取数据时，为了提高效率，一般需要定义一个缓冲区，将读取的数据先放到缓冲区，然后再从缓冲区中提取。具体的方法如下：首先定义一个输入流的读取对象，从Socket中导出输出对象，定义一个缓存，将输入流放到缓冲中。

缓存区的应用减少了实际的物理读写次数，缓存区的内存一直被重用，减少了动态分配和回收内存区的次数。通过这两个方面来提高了IO操作的效率。

```
InputStreamReader reader = new InputStreamReader(socket.getInputStream());  
BufferedReader br = new BufferedReader(reader);  
```

当然，这两行代码可以简化一下：

```
BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));  

```

<br/>

#### Socket的关闭和半关闭

当客户端和服务器的通信结束时，要及时的关闭Socket，以释放Socket占用的包括端口在内的各种资源，同时保证接收方能够顺利的读到输入流的末尾，Socket的关闭是通过调用Socket对象的close()方法来实现的。为了确保关闭操作总被执行，一般是把这个操作放到finally代码块中。

但是调用close()方法之后，输入流和输出流都会被关闭，Socket连接被断开。有时候，我们只希望关闭输入流或者是输出流之一，而不希望连接断开，这时候需要使用Socket类提供的半关闭方法：shutdownInput()、shutdownOutput()来实现。

<br/>

#### Socket的选项

Socket选项有很多种，都是可以设置的，用来定义Socket的一些动作。常见的选项有以下几种，每种选项都有相应的方法来实现各种动作的设定。

* A、TCP_NODELAY ：表示立即发送数据，不使用缓存区。（默认为false，表示使用Negale算法，使用缓存区）
* B、SO_RESUSEADDR：表示允许重用Socket所绑定的本地地址
* C、SO_TIMEOUT：表示接收数据时的等待超时时间
* D、SO_LINGER ：表示执行close方法时，是否立即关闭底层的Socket
* E、SO_SNFBUF ：表示发送数据的缓存区的大小
* F、SO_REVBUF：表示接收数据的缓存区的大小
* G、SO_KEEPALIVE：表示对于长时间处于空闲状态的Socket，是否自动把它关闭
* H、OOBINLINE：表示是否支持发送一个字节的TCP紧急数据


<br/>

#### 服务类型的设定

在Internet上传输数据时，具有不同的服务类型，用户可以根据自己的需求，选择合适的服务类型。IP 规定了四种服务类型，用来定性的描述服务的质量。

* 低成本：发送成本低
* 高可靠行：保证把数据可靠的送达目的地
* 最高吞吐量：一次可以接收或者发送大批量的数据
* 最小延迟：传输数据的速度快

<br/>

而Socket提供了设置服务类型的方法：setTrafficClass(int trafficClass)，其中的服务类型整型如下：

* 0x02: 低成本
* 0x04: 高可靠性
* 0x08: 高吞吐量
* 0x10: 最小延迟

```
Socket socketd = new Socket(www.javathinker.org, 80);   
socketd.setTrafficClass(0x08); 
```


<br/>

#### ServerSocket的构造方法

```
ServerSocket serverSocket = new ServerSocket();  
ServerSocket serverSocket = new ServerSocket(int port);  
ServerSocket serverSocket = new ServerSocket(int port, int backlog);  
ServerSocket serverSocket = new ServerSocket(int port, int backlog, InetAddress bindAddr);  
```

参数Port指定服务器要绑定（监听）的端口，参数backlog指定客户连接请求队列的长度，参数bindAddr指定服务器要绑定的IP地址。同样，这里也有默认无参的构造器，作用也是在绑定端口之前设定ServerSocket选项参数。

ServerSocket绑定端口之后，开始监听端口的连接请求，调用ServerSocket的accept()方法从连接请求队列中取出一个客户的连接请求，然后创建该服务器与客户端的Socket对象，并将这个对象返回。如果队列中没有连接请求，则accept方法就会阻塞，一直等待下去，直到接收到连接请求才返回。

同样的，在数据传输结束之后，要将ServerSocket的连接关闭，断开与客户的连接。

<br/>

#### ServerSocket信息获取以及选项参数

可以通过getInetAddress()方法来获取该服务器绑定的IP地址， 通过getLocalPort()来获取绑定的端口号。
        
ServerSocket的选项主要有下面几个：

* A、SO_TIMEOUT ：表示等待客户连接的超时时间
* B、SO_REUSEADDR：表示是否允许重用服务器所绑定的地址
* C、SO_RCVBUF：表示接收数据的缓冲区的大小




























 