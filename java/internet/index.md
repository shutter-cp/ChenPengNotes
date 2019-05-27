**ChenPengNotes**	

# 网络编程
-------------

# 1. 网络通信概述
可以参考[link](https://www.jianshu.com/p/ae2f9e5f4356)中的第二点[网络基础](https://www.jianshu.com/p/ae2f9e5f4356)
## 1.1 软件的结构
1. C/S 结构：客户端、服务器

2. B/S 结构： 浏览器、服务器
## 1.2 网络通信协议
1. 网络通信协议   
一个网络中的计算机需要通信，他们需要遵循一些规则    
在计算机中这些**链接**和**通信规则**就称之为**网络通信协议**
他们对数据的传输格式，传输速率，传输步骤等统一规定，通信双方必须同时遵循这些规则才能通信

2. TCP/IP协议

    > 又称为 传输控制协议，因特网互联协议
    
    定义了计算机如何介入因特网，数据在节点之间传输的标准      
    内部包含一系列用于处理数据通信的协议，采用了4层的分层模型，每层都呼叫自己的下一层为自己提供协议完成自己的需求

链路层:链路层是用于定义物理传输通道,通常是对某些网络连接设备的驱动协议,例如针对光纤、网线提供的驱动

网络层:网络层是整个TCP/IP协议的核心,它主要用于将传输的数据进行分组,将分组数据发送到目标计算机或者网络。

运输层:主要使网络程序进行通信,在进行网络通信时,可以采用TCP协议,也可以采用UDP协议。

应用层:主要负责应用程序的协议，例如HTTP协议、FTP协议等。

## 1.3 协议分类
1. UDP——无连接通信协议     
    1. 发送端给接收段发送数据不需要接收端同意，也不会判断是否存在
    2. 耗资少，通信效率高 通常用于音视频和普通数据的传输 视频会议、视频聊天一般都采用的UDP协议
     偶尔会丢一两个数据包，但对结果不会与太大影响。
    3. 因为UDP的无连接性，所有我们传输重要数据时最好不要用UDP协议       


2. TCP——面向连接通信协议    
    在传输数据之前会先建立连接，然后才会开始传输数据，可以提供两个节点之间无差错的数据传输。    
    三次握手：
    1. 客户端向服务端发送连接请求，等待服务器确认
    2. 服务端向客户端发送一个响应，回应收到了请求
    3. 客户端再次向服务端发送已经收到确认信息的响应，确认建立连接
    通过三次握手建立连接后，客户端与服务端就可以进行数据传输了。因为此特性，TCP协议可以保证传输数据的安全

## 1.4网络编程的3要素
1. 协议   
计算机网络通信必须遵循的规则，详情见上述
2. IP地址     
互联网协议地址，设备的唯一编号 
ipV4:32位    
ipV6:128位

3. 端口号  
- 指定了ip只能准确的找到计算机，但是计算机上有很多的软件。   
- 如何准确的与计算机上的软件进行通信呢？ 
- ip:端口  就可以准确的找到软件   
- 1024之前的端口基本上被已知软件占用         
- 端口不能多个软件共同使用        
- 端口的范围为 0——65535

# 2. TCP协议
> TCP通信能实现两台计算机之间进行数据交换，通信的两端，严格的区分客户端和服务端

TCP通信： 面向连接的通信，客户端和服务端必须进行三次握手才能建立逻辑连接，才能安全通信

## 2.1 通信的步骤：
1. 服务器先启动，服务器不会主动发起对客户段的连接。
2. 客户端发起请求，客户端与服务端就会建立一个逻辑连接        
    连接中包括一个对象——IO对象
3. 客户端与服务端就可以使用
`传输的数据不限于字符，所以IO对象是字节流对象`

## 2.2 服务端必须明确的事情
1. 多个客户端与服务端进行交互时，服务端必须明确是哪个客户端与其交互 
    在服务端有一个方法叫，accept可以获取到请求客户端对象
2. 多个客户端与服务器进行交互时，就需要使用多个IO流对象      
    服务端是没有IO流的，服务端可以获取到发起请求的客户端的Socket对象，从而使用每个客户端中的Socket中提供的IO流与客户端进行交互   
    - 服务器使用客户端的字`节输入流`读取客户端发送的数据
    - 服务器使用客户端的字`节输出流`给客户端会写数据
   简单说就是服务器使用客户端的流进行交互
   
 ![image.png](https://upload-images.jianshu.io/upload_images/15992215-b090c2c824b8adf2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
code:

1. TcpClient服务类
```java
public class TcpClient {
    public static void main(String[] args) throws IOException {
        //1. 创建一个客户端对象Socket，构造方法写入IP地址和端口号
        Socket socket = new Socket("127.0.0.1",8888);

        //2. 使用Socket对象中的 方法getOutputStream();获取到网络字节输出流
        OutputStream outputStream = socket.getOutputStream();

        //3. 使用网络字节输出流中的write方法来给服务器发送数据
        outputStream.write("你好服务器！！".getBytes());

        //拿到回复
        InputStream inputStream = socket.getInputStream();

        //读取类容
        ReaderMsg.readers(inputStream);

        // 释放资源
        socket.close();
    }
}
```


2. TcpServer 服务类
```java
public class TcpServer {
    public static void main(String[] args) throws IOException {
        //创建服务器对象ServerSocket,并通过构造方法指定端口号
        ServerSocket serverSocket = new ServerSocket(8888);

        //使用ServerSocket中的accpet方法来得到客户端的Socket对象
        Socket socket = serverSocket.accept();

        //使用Socket中的getInputStream();方法来获取到网络输入流
        InputStream inputStream = socket.getInputStream();

        //读取类容
        ReaderMsg.readers(inputStream);

        //使用Socket中的getOutputStream();方法来获取到网络输出流
        OutputStream outputStream = socket.getOutputStream();

        outputStream.write("我收到你的问候！".getBytes());

        //关闭流
        socket.close();
        serverSocket.close();

    }
}
```

3. 字符输出类
```java
public class ReaderMsg {
    public static void readers(InputStream inputStream) throws IOException {
        byte[] bytes = new byte[1024];
        int len = inputStream.read(bytes);
        System.out.println(new String(bytes,0,len));
    }
}
```

# 3. 综合案例_文件上传

客户端：
```java
public class TcpFileClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1",8888);
        OutputStream outputStream = socket.getOutputStream();

        BufferedInputStream biStream = new BufferedInputStream(
                new FileInputStream("I:\\download\\myeclise-2018.8.0破解文件-小笨\\readme.txt"));

        System.out.println("客户端：我开始向服务器上传数据");

        byte[] bytes = new byte[512];
        int lenth = 0;
        while ((lenth = biStream.read(bytes))!=-1){
            outputStream.write(bytes,0,lenth);
        }
        //不写的话服务器将不知道什么时候才将文件上传结束
        socket.shutdownOutput();

        InputStream inputStream = socket.getInputStream();

        lenth = 0;
        while ((lenth = inputStream.read(bytes))!=-1){
            System.out.println(new String(bytes,0,lenth));
        }

        System.out.println("客户端：数据上传完毕");

        biStream.close();
        socket.close();
    }
}
```

服务端：
```java
public class TcpFileServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8888);

        Socket socket = serverSocket.accept();
        InputStream inputStream = socket.getInputStream();

        File file = new File("I:\\chenpengFiles\\test");
        if (!file.exists()){
            file.mkdirs();
        }

        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(
                new FileOutputStream(file+"//test.txt"));
        System.out.println("服务器：开始上传文件");
        byte[] bytes = new byte[512];
        int lenth = 0;
        while ((lenth = inputStream.read(bytes))!=-1){
            bufferedOutputStream.write(bytes,0,lenth);
        }

        socket.getOutputStream().write("文件上传成功！".getBytes());

        System.out.println("服务器：文件上传成功");
        bufferedOutputStream.close();
        socket.close();
        serverSocket.close();
    }
}
```
优化版：
```java
public class TcpFileServerThread {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8888);

        while (true){
            Socket socket = serverSocket.accept();

            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        InputStream inputStream = socket.getInputStream();

                        File file = new File("I:\\chenpengFiles\\test");
                        if (!file.exists()){
                            file.mkdirs();
                        }
                        String fileName = "\\"+new Random().nextInt(99999)+".txt";
                        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(
                                new FileOutputStream(file+fileName));
                        System.out.println("服务器：开始上传文件");
                        byte[] bytes = new byte[512];
                        int lenth = 0;
                        while ((lenth = inputStream.read(bytes))!=-1){
                            bufferedOutputStream.write(bytes,0,lenth);
                        }

                        socket.getOutputStream().write("文件上传成功！".getBytes());

                        System.out.println("服务器：文件上传成功");
                        bufferedOutputStream.close();

                    }catch (IOException e){
                        System.out.println(e.getMessage());
                        throw new RuntimeException();
                    }finally {
                        try {
                            socket.close();
                        }catch (Exception e){
                            throw new RuntimeException();
                        }
                    }
                }
            }).start();
        }
    }
}
```

# 4. 模拟BS服务器案例
code:       
```java
public class WebServer {
    private String paths = "H:\\javaCode\\ideaCode\\java2019\\demo\\src\\com\\looc\\demo12网络编程\\demo2\\";

    public void webServer() throws IOException {
        ServerSocket serverSocket = new ServerSocket(80);

        //监听事件
        while (true){
            //拿到socket对象
            Socket socket = serverSocket.accept();

            //开启多线程提高效率
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        InputStream inputStream = socket.getInputStream();

                        //读响应体
                        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream,"US-ASCII"));

                        //读取到文件
                        String str = br.readLine().split(" ")[1].substring(1);
                        BufferedInputStream bfs = new BufferedInputStream(
                                new FileInputStream(paths+str));

                        //回显
                        OutputStream oStream = socket.getOutputStream();
                        oStream.write("HTTP/1.1 200 OK\r\n".getBytes());
                        oStream.write("Content-Type:text/html\r\n".getBytes());
                        oStream.write("\r\n".getBytes());

                        byte[] bytes = new byte[1024];
                        int len = 0;
                        while ((len = bfs.read(bytes))!=-1){
                            oStream.write(bytes,0,len);
                        }
                        //关闭流
                        bfs.close();
                        br.close();
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }finally {
                        try {
                            socket.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }).start();

        }
    }


    public static void main(String[] args) throws IOException {
        WebServer webServers = new WebServer();
        webServers.webServer();
    }
}
```