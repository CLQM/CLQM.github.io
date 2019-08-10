---
layout: post
title: "理解socket"
subtitle: 'what is socket'
author: "cslqm"
header-style: text
tags:
  - Linux
---


1.socket与进程的关系
1).socket与进程间的关系:socket   用来让一个进程和其他的进程互通信息(IPC)，而Socket接口是TCP/IP网络的API接口函数。
2).进程间通信（本机内）

进程间通信（不同计算机，要联网）


2、socket与文件的关系——如何理解socket是种特殊的I/O?
1）Socket最先应用于Unix操作系统,如果了解Unix系统的I/O的话，就很容易了解Socket了，因为Socket数据传输其实就是一种特殊的I/O。 
2）可对其进行文件操作
3）有文件描述符。而文件描述符的本质是一个非负整数。只是用于区分。类似的还有进程ID。

3.服务器端口与连接个数的关系
1）服务端在8088上监听，然后生成一个新的socket与client通讯。(注意：服务器端监听端口是
不变的，但socket连接可以一直生成，一个线程对应一个socket.)
同一时刻，一个端口只能建立一个连接。 
在一个端口监听，但是在监听端口的同时，生成一个等待队列，每一个来自客户端的连接都会送入等待队列中，服务器利用一定的算法进行选择相应的连接请求处理，所以在一个端口可以监听多各请求嘛。如果同时的连接过多，服务器相应每个连接的时间就会相应的变长。就会变慢。
2）QQ的实现方法就是在登陆的时候告诉服务器你已经登陆，发送消息的时候，首先你会发送一个包给服务器，服务器查看你需要发送的对方是否在线，如果在线就返回包告诉你对方在线，然后你就会和对方建立一个连接，然后直接发消息给对方，如果对方不在线，那么就会通过服务器转发你这次的消息
3）网络IO数与你的CPU数目一致将是比较好的选择（考虑到多线程多进程可以提高效率）。 
没有必要每个客户分配一个端口。绝对是一种无谓的浪费。 


4.有人知道socket监听的一个端口.最多可以有多少个客户连接? 
1）listen()中有个参数，应该是确定并行连接客户数？！
2）The   maximum   length   of   the   queue   of   pending   connections.   If   this   value   is   SOMAXCONN,   then   the   underlying   service   provider   responsible   for   socket   s   will   set   the   backlog   to   a   maximum   "reasonable "   value.   There   is   no   standard   provision   to   find   out   the   actual   backlog   value.   
3）linux2.4下，最多可以有1024个socket连接
4）同时连接请求好像是5个（是连接请求，不是连接），可保持的链接理论上是65535（2字节的SOCKET端口号），

3.Socket是网络上运行的两个程序间双向通讯的一端，它既可以接受请求，也可以发送请求，利用它可以较为方便的编写网络上数据的传递。

5.问：现在server与client想建立socket连接，server仅知道client的IP，端口号不知道，能建立连接吗?怎么建立呢？有没有代码看看？
答：C和S是相对而言的，发起连接的一方就是C，而监听端口接受连接的一方就是S，C如果不知道S监听的端口，怎么发起连接呢，
另外，对于S而言，端口是S上各个服务的区分标志，如果用错误的端口号去连接，是不能获得正确的服务的。
client的端口是不需要指定的，Server绑定端口，然后监听，client使用server的IP和端口建立socket连接 

6.精彩问答

问：看到的文章上说“每个网络通信循环地进出主计算机的TCP   应用层。它被两个所连接的号码唯一地识别。这两个号码合起来叫做套接字.组成套接字的这两个号码就是机器的IP   地址和TCP   软件所使用的端口号。” 
又说“通过socket（）函数可以创建一个套接字，然后再把它绑定到端口号...” 
那么套接字socket的概念究竟到哪里为止呢？是仅限于socket()返回的文件描述符？还是是IP和端口号的组合？如果是，那么socket()调用之后产生的套接字描述符的作用是什么呢？   套接字描述符，IP地址，端口号三者间的关系是怎样的？ 
谢谢各位前辈解答。
答：一个socket句柄代表两个地址对   “本地ip:port”--“远程ip:port”
问：那么socket的概念到底到那里为止呢？比如，利用socket()可以产生一个套接字句柄，可是在bind()   或者   connect   ()   之前它只是一个文件描述符，和linux中其他的文件描述符一样。 
如果说socket代表一个两个地址对，那么句柄的作用是不是仅仅是在bind()   或者   connect   ()   之后的用于区分和标记这样的地址对呢？因为这样他才能和网络的概念联系起来。这样的话，socket的意义应该是说用文件描述符描述的通信双方的IP地址和端口号地址对？（而文件描述符是区分这些地址对的标记？)
答：socket为内核对象，由操作系统内核来维护其缓冲区，引用计数，并且可以在多个进程中使用。 
至于称它为“句柄”“文件描述符”都是一样的，它只不过是内核开放给用户进程使用的整数而已
问：谢谢楼上，是我没描述清楚。对于“句柄”和“文件描述符”我没有异议。 
我想我的问题是在于句柄和ip、port的关系，不知道我这样说对否： 
1.   每一个socket   本质上都是指一个ip地址和端口对 
2.   为了操作这样的地址对，使用了文件描述符 
3.   socket（）函数只创建了一个普通的文件描述符，在进行bind（）或者connect()之前并不能说创建了用于网络通讯的套接字 
4.   只有在进行了bind（）或者connect()之后socket才被创立起来
答：socket（）创建了一个socket内核对象。 
accept或者connect后，才可以对socket句柄读写。因为只有在   connect或者bind,listen,accept后才会设置好socket内核对象里边的ip和端口 

二、socket和端口理解

一个socket句柄代表两个地址对 “本地ip:port”--“远程ip:port” 
在windows下叫句柄，在linux下叫文件描述符 
socket为内核对象，由操作系统内核来维护其缓冲区，引用计数，并且可以在多个进程中使用。 至于称它为“句柄”“文件描述符”都是一样的
我假定读者已经对于socket连接的建立过程和各种状态转换比较熟悉了，因为这篇文档的目的是澄清概念，而不是介绍概念。 
在使用socket编程时，我们都知道在网络通信以前首先要建立连接，而连接的建立是通过对socket的一些操作来完成的。那么，建立连接的过程大致可以分为以下几步： 
1） 建立socket套接字。 
2） 给套接字赋予地址，这个地址不是通常的网络地址的概念。 
3） 建立socket连接。 

1． 建立socket套接字。 
使用socket建立套接字的时候，我们实际上是建立了一个数据结构。这个数据结构最主要的信息是指定了连接的种类和使用的协议，此外还有一些关于连接队列操作的结构字段（这里就先不涉及他们了）。 
当我们使用socket函数以后，如果成功的话会返回一个int型的描述符，它指向前面那个被维护在内核里的socket数据结构。我们的任何操作都是通过这个描述符而作用到那个数据结构上的。这就像是我们在建立一个文件后得到一个文件描述符一样，对文件的操作都是通过文件描述符来进行的，而不是直接作用到inode数据结构上。我之所以用文件描述符举例，是因为socket数据结构也是和inode数据结构密切相关，它不是独立存在于内核中的，而是位于一个VFS inode结构中。所以，有一些比较抽象的特性，我们可以用文件操作来不恰当的进行类比以加深理解。 
如前所述，当建立了这个套接字以后，我们可以获得一个象文件描述符那样的套接字描述符。就象我们对文件进行操作那样，我们可以通过向套接字里面写数据将数据传送到我们指定的地方，这个地方可以是远端的主机，也可以是本地的主机。如果你有兴趣的话，还可以用socket机制来实现IPC，不过效率比较低，试试也就行了（我没有试过）。 

2． 给套接字赋予地址。 
依照建立套接字的目的不同，赋予套接字地址的方式有两种：服务器端使用bind，客户端
使用connetc。 
Bind: 
我们都知道，只要使用IP, prot就可以区分一个tcp/ip连接（当然这个连接指的是一个连接通道，如果要区分特定的主机间的连接，还需要第三个属性 hostname）。 我们可以使用bind函数来为一个使用在服务器端例程中的套接字赋予通信的地址和端口。
在这里我们称通信的IP地址和端口合起来构成了一个socket地址，而指定一个socket使用特定的IP和port组合来进行通行的过程就是赋予这个socket一个地址。 要赋予socket地址，就得使用一个数据结构来指明特定的socket地址，这个数据结构就是struct sockaddr。对它的使用我就不说了，因为这篇文档的目的是澄清概念而不是说明使用方法。Bind函数的作用就是将这个特定的标注有socket地址信息的数据结构和socket套接字联系起来，即赋予这个套接字一个地址。但是在具体实现上，他们两个是怎么联系在一起的，我还不知道。 
一个特定的socket的地址的生命期是bind成功以后到连接断开前。你可以建立一个socket数据结构和socket地址的数据结构，但是在没有bind以前他们两个是没有关系的，在bind以后他们两个才有了关系。这种关系一直维持到连接的结束，当一个连接结束时，socket数据结构和socket地址的数据结构还都存在，但是他们两个已经没有关系了。如果你要是用这个套接字在socket地址上重新进行连接时，需重新bind他们两个。再注明一次，我说的这个连接是一个连接通道，而不是特定的主机之间的连接。 
Bind指定的IP通常是本地IP（一般不特别指定，而使用INADDR_ANY来声明），而最主要的作用是指定端口。在服务器端的socket进行了bind以后就是用listen来在这个socket地址上准备进行连接。 
connect: 
对于客户端来说，是不会使用bind的（并不是不能用，但没什么意义），他们会通过connet函数来建立socket和socket地址之间的关系。其中的socket地址是它想要连接的服务器端的socket地址。在connect建立socket和socket地址两者关系的同时，它也在尝试着建立远端的连接。 
3． 建立socket连接。 
对于准备建立一个连接，服务器端要两个步骤：bind, listen；客户端一个步骤：connct。如果服务器端accept一个connect，而客户端得到了这个accept的确认，那么一个连接就建立了。
 

三、客户/服务器模式模式的理解

客户/服务器模式采取的是主动请求方式：
首先服务器方要先启动，并根据请求提供相应服务：
1. 打开一通信通道并告知本地主机，它愿意在某一公认地址上（周知口，如FTP为21）接收客户请求；
2. 等待客户请求到达该端口；
3. 接收到重复服务请求，处理该请求并发送应答信号。接收到并发服务请求，要激活一新进程来处理这个客户请求（如UNIX系统中用fork、exec）。新进程处理此客户请求，并不需要对其它请求作出应答。服务完成后，关闭此新进程与客户的通信链路，并终止。
4. 返回第二步，等待另一客户请求。
5. 关闭服务器

客户方：
1. 打开一通信通道，并连接到服务器所在主机的特定端口；
2. 向服务器发服务请求报文，等待并接收应答；继续提出请求......
3. 请求结束后关闭通信通道并终止。

从上面所描述过程可知：
1. 客户与服务器进程的作用是非对称的，因此编码不同。
2. 服务进程一般是先涌纪纪户请求而启动的。只要系统运行，该服务进程一直存在，直到正常或强迫终止。


作者：yeyuangen 
来源：CSDN 
原文：https://blog.csdn.net/yeyuangen/article/details/6799575 
版权声明：本文为博主原创文章，转载请附上博文链接！