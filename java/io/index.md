**ChenPengNotes**

# 1. java File，FileFilter

## 1. File类;
> 文件和目录路径名的抽象表示

### 1.1 构造
1. File(String filepath);    
直接写路径

2. File(String parent, String path);    
parent 父路径  
path 子路径

3. File(File parent,String path)    
parent 先构造一个File
path  这个File下面的路径

### 1.2 获取方法
1. String getAbsoluteFile();    
    返回绝对路径
2. String getName();    
    返回文件或目录名
3. String getPath();    
    返回路径
4. Long length();   
    返回文件长度  
    只能获取文件的，不能获取文件夹的

demo:
```
 public static void readPath(){
        File file = new File("fileTest.txt");
        File file2 = new File("C:\\Users\\81022\\Desktop","无标题.png");

        String fileAbsPath = file.getAbsolutePath();
        String fileName = file.getName();
        String filePath = file.getPath();
        Long fileLenght = file.length();

        System.out.println(fileAbsPath);
        System.out.println(fileName);
        System.out.println(filePath);
        System.out.println(fileLenght);
    }
```

### 1.3 File对象的删除和创建方法

#### 1.3.1 创建方法
1. 创建文件     
boolean createNewFile()   
//创建一个文件（只能是文件，不能是文件夹）  
//如果存在则不会创建

2. 创建文件夹    
boolean mkdir();    
//只能创建文件夹
//如果存在则不会创建

demo:
```
    public void newFileDemo() throws IOException {
        File file = new File("C:\\Users\\81022\\Desktop\\111.txt");
        System.out.println(file);
        boolean b1 = file.createNewFile();
        System.out.println(b1);
    }

    public void  mkdirDemo(){
        File file = new File("C:\\Users\\81022\\Desktop\\111");
        boolean b1 = file.mkdirs();
        System.out.println(b1);
    }
```

#### 1.3.2 判断方法
1. 判断是否是文件      
boolean isFile();
2. 判断是否是文件夹     
boolean isDirectory();
3. 判断文件或者文件夹是否存在
boolean exist();

#### 1.3.3 删除方法
boolean delete();          
只能删除当文件，或空文件夹

### 1.4 其他方法
1. String [] list();    
仅返回文件的文件名数组
2. File[] listFiles();
返回文件数组
demo:
```
    public void listDemo(){
        File file = new File("C:\\Users\\81022\\Downloads");
        //list 方法
        String[] fileList = file.list();
        for (String stirng : fileList) {
            System.out.println(stirng);
        }
        //listFiles
        File[] filesList = file.listFiles();
        for (File file1 : filesList) {
             System.out.println(file1.getAbsolutePath());
        }
    }
```

删除某个目录下面全部文件demo:
```java
public class DeleteDemo {
    public static void main(String[] args){
        File file = new File("C:\\Users\\81022\\Downloads\\deleteTest");
        //先判断路径是否存在
        if (!isPath(file)){
            System.out.println("路径不存在！");
        }else {
            boolean result = deleteTest(file);
            if (result){
                System.out.println("删除成功！");
            }else {
                System.out.println("删除失败！");
            }
        }
    }
    public static boolean isPath(File file){
        return file.exists();
    }
    public static boolean deleteTest(File file){
        //如果是文件直接删除
        if (file.isFile()){
            return file.delete();
        }
        //如果不是文件
        //先删除 从而判断是否为空文件夹
        if (file.delete()){
            return true;
        }
        //删除不成功那么说明为非空文件夹
        File[] fileList = file.listFiles();
        for (File file1 : fileList) {
            deleteTest(file1);
            file1.delete();
        }
        return true;
    }
}
```

## 2 文件过滤器
```java
public class FileFilterDemo {
    public static void main(String[] args){
        String endString = ".txt";
        File file = new File("C:\\Users\\81022\\Downloads\\deleteTest");

        FileFilter fileFilter = new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                if (pathname.getName().endsWith(endString)){
                    return true;
                }
                return false;
            }
        };

        File[] fileArray = file.listFiles(fileFilter);
        for (File file1 : fileArray) {
            System.out.println(file1.getName());
        }

    }
}
```

# 2. java IO流，文件
## 1. IO 流
### 1.1 根据流的流向分类
1. Input 输入流
2. Output 输出流

### 1.2 根据流操作的数据来分类
1. 字符流： 操作字符        
只能操作普通文本文件
2. 字节流：操作字节     
能操作一切文件

### 1.3 java中的四大流
1. 字符输入流    
    1. 父类：Reader    
        1. FileReader   
        2. BufferedReader       
    2. 功能： 读取一个字符，读取一个字符数组
    
2. 字符输出流    
    1. 父类：Write    
        1. FileWrite    
        2. BufferedWrite    
    2. 功能：写一个字符，写一个字符数组，字符串
    
3. 字节输入流
    1. 父类：InputStream   
        1. FileInputStream
        2. BufferedInputStream
    2. 功能：读取一个字节，读取一个字节数组
    
4. 字节输出流
    1. 父类：OutputStream  
        1. FileOutputStream
        2. BufferedOutputStream
    2. 功能：写一个字节，写一个字节数组
    
5. 规律   
只要是输入流 此流方法名一定叫read     
只要是输出流 此流方法名一定叫write

java中的流命名规范;
功能+父类的名字

## 2 OutputStream 
>  字节输出流的根类，是一个抽象类

1. void close();    
关闭流
2. void flush();    
刷新流

3. void write(int b);   
写一个字节
4. void write(byte[] bs);   
写一个字节数组
5. void write(byte[] bs, int startIndex, int lenght);   
写一个字节数组的一部分

### 2.1 构造
1. FileOutPutStream(String filename)    
不可以续写
2. FileOutPutStream(String filename,boolean flag)   
可以续写
3. FileOutPutStream(File file,boolean flag) 

### 2.2 使用
demo:
```
public void outputStreamdemo() throws IOException {
    //创建数据流
    FileOutputStream fileOutputStream = new FileOutputStream("2.txt",true);
    //写数据

    fileOutputStream.write("hello".getBytes());

    //关闭数据流
    fileOutputStream.close();
}
```


## 3 InputStream
>  字节输入流的根类，是一个抽象类


1. void close();    
关闭流
2. int read();  
读取一个字节，返回的是ASSIC码
3. int read(byte[] bs);     
读取一个字节数组，读出的数据是传入的，返回实际读出的数量

### 3.1 构造
1. FileInputStream  
### 3.2 使用
demo:
```
    public void inputStreamDemo() throws IOException{
        FileInputStream fileInputStream = new FileInputStream("2.txt");

        //一个个读
        int b = 0;
        while ((b = fileInputStream.read()) != -1){
            System.out.print((char)b);
        }

        fileInputStream.close();
    }

```

```
    public void inputStreamDemo2() throws IOException{
        FileInputStream fileInputStream = new FileInputStream("2.txt");

        //一次读一个数组
        int tempLen = 0;
        byte[] bs = new byte[4];
        while ((tempLen=fileInputStream.read(bs))!=-1){
            String s = new String(bs,0,tempLen);
            System.out.print(s);
        }

        fileInputStream.close();
    }
```

#### 复制文件
```java
public class CopyFileDemo {
    public static void main(String[] args) throws IOException {
        String inputSrc = "C:\\Users\\81022\\Downloads\\copyFile\\原始文件\\无标题.png";
        String outputSrc = "C:\\Users\\81022\\Downloads\\copyFile\\目标文件夹\\test.png";
        //创建两个流
        FileInputStream fileInputStream = new FileInputStream(inputSrc);
        FileOutputStream fileOutputStream = new FileOutputStream(outputSrc,true);

        //复制
        //一次读一个字节  太慢
        /*int b = 0;
        while ((b=fileInputStream.read())!=-1){
            fileOutputStream.write(b);
        }*/

        byte[] bs = new byte[1024];
        int len = 0;
        while ((len=fileInputStream.read(bs))!=-1){
            fileOutputStream.write(bs,0,len);
        }


        //关流
        fileInputStream.close();
        fileOutputStream.close();

    }
}

```

## 4 缓冲流
> 相比没有换成功流，效率更高

### 4.2 BufferedOutputStream 缓冲输出流
1. 创建
    ```
     BufferedOutputStream bufOutputStr = new BufferedOutputStream(new FileOutputStream(outSrc));
    ```
2. 方法
    ```
        //写一个字节
        bufOutputStr.write(11);
        //写一个字节数组
        bufOutputStr.write("test".getBytes());
        //写一个字符串
        byte[] bs= new byte[10];
        bs = "你好！！！".getBytes();
        bufOutputStr.write(bs);
        bufOutputStr.write(bs,0,2);
    ```
    
### 4.1 BufferedInputStream 缓冲输入流
1. 创建
    ```
     BufferedInputStream bufdInputStr = new BufferedInputStream(new FileInputStream(inSrc));
    ```
2. 方法
    ```
        int n = 0;
        while ((n = bufdInputStr.read())!=-1){
            System.out.print((char)n);
        }
    ```    
    ```

        //读一个字符串
        byte[] bs = new byte[10];
        int len = 0;
        String s;
        while ((len = bufdInputStr2.read(bs))!=-1){
            s= new String(bs,0,len);
            System.out.print(s);
        }
    ```

# 5 字符流
> 使用字节流读中文会出现乱码

### 5.1 FileReader
和字节流差不多

### 5.2 BufferedReader
和字节流差不多     
多了一个 readLine();

### 5.3 FileWrite
和字节流差不多     
多了写字符串的功能

### 5.4  BufferWrite
和字节流差不多     
多一个newLine()


demo:
```
    @Test
    public void ReaderDemo1() throws IOException {
        FileReader fileReader = new FileReader(new File("fileTest.txt"));
        int n = 0;
        while ((n=fileReader.read())!=-1){
            System.out.print((char)n);
        }

        fileReader.close();
    }

    @Test
    public void ReaderDemo2() throws IOException {
        FileReader fileReader = new FileReader("fileTest.txt");

        int len = 0;
        char[] cr = new char[10];
        while ((len = fileReader.read(cr))!=-1){
            System.out.print(cr);
        }

        fileReader.close();
    }

    @Test
    public void RederDemo3() throws IOException{
        BufferedReader bfReader = new BufferedReader(new FileReader("fileTest.txt"));

        String s;
        while ((s = bfReader.readLine())!=null){
            System.out.println(s);
        }

        bfReader.close();
    }



    @Test
    public void RederDemo4() throws IOException{
        FileWriter fr = new FileWriter("fileTest.txt");
        fr.write("这里是测试写入");
        fr.close();
    }


    @Test
    public void RederDemo5() throws IOException{
        BufferedWriter br = new BufferedWriter(new FileWriter("fileTest.txt",true));

        br.newLine();
        br.write("sdfsfd");
        br.close();
    }
```

##### 文件归类demo：
```java
    import net.sf.json.JSONObject;
    
    import java.io.*;
    import java.util.Map;
    import java.util.Scanner;
    import java.util.Set;
    
    /**
     * @author chenPeng
     * @version 1.0.0
     * @ClassName MoveFile.java
     * @Description TODO
     * @createTime 2019年01月26日 15:41:00
     */
    public class MoveFile {
        static String src;
        static String toSrc;
        static Set<Map.Entry<String,String>> fenLei;
        static JSONObject json;
    
    
        public static void init() throws IOException{
            File directory = new File("");
            BufferedReader fileReader = new BufferedReader(new FileReader(directory.getAbsoluteFile()+"/config.json"));
            StringBuffer strBuf = new StringBuffer();
            String temp;
            while ((temp = fileReader.readLine())!=null){
                System.out.println(temp);
                strBuf.append(temp);
            }
    
            fileReader.close();
    
            json = JSONObject.fromObject(strBuf.toString());
            //获取连接
            JSONObject srcJson = json.getJSONObject("路径");
            src = (String) srcJson.get("原始路径");
            toSrc = (String) srcJson.get("目标路径");
    
            fenLei = json.getJSONObject("分类").entrySet();
        }
    
        /**
         * 入口
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:24
         * @Param [args]
         * @return void
         **/
        public static void main(String[] args) throws IOException {
            init();
            File files = new File(src);
            getPath(files.listFiles());
    
            System.out.println("是否删除原文件！！！");
            System.out.println("1 为删除 否则按其他");
            Scanner scanner = new Scanner(System.in);
            String str = scanner.next();
            if ("1".equals(str)){
                System.out.println("正在删除...");
                removeFile(files.listFiles());
            }
    
        }
    
        /**
         * 删除文件
         * @Author chenpeng
         * @Description //TODO
         * @Date 22:20
         * @Param [files]
         * @return void
         **/
        public static void removeFile(File[] files) throws IOException{
            for (File file : files) {
                if (file.isFile()){
                    file.delete();
                }else{
                    if (!file.delete()){
                        removeFile(file.listFiles());
                        file.delete();
                    }
                }
            }
        }
    
        /**
         * 得到路径 分析是文件还是文件夹
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:24
         * @Param [files]
         * @return void
         **/
        public static void getPath(File[] files) throws IOException {
            for (File file : files) {
                if (file.isFile()){
                    classification(file);
                }else{
                    copyDir(file.listFiles(),file.getName());
                }
            }
        }
    
    
        /**
         * 递归复制文件
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:24
         * @Param [files]
         * @return void
         **/
        public static void copyDir(File[] files, String name) throws IOException {
            //如果不存在就先建立文件夹
            String gotoSrc = toSrc+"/文件夹/"+name;
            isHave(gotoSrc);
    
            //复制文件
            for (File file : files) {
                if (file.isFile()){
                    copy(file,gotoSrc);
                }else{
                    copyDir(file.listFiles(), name+"/"+file.getName());
                }
            }
        }
    
        /**
         * 复制文件
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:25
         * @Param [file]
         * @return void
         **/
        public static void copy(File file,String src) throws IOException {
    
            System.out.println(file.getAbsolutePath());
            System.out.println("复制到");
            System.out.println(src);
            System.out.println("==============开始执行==============");
    
            BufferedInputStream fis = new BufferedInputStream(new FileInputStream(file));
            BufferedOutputStream fos = new BufferedOutputStream(new FileOutputStream(src+file.getName()));
    
            byte[] bs = new byte[1024];
            int len = 0;
            while ((len = fis.read(bs))!=-1){
                fos.write(bs,0,len);
            }
    
            fos.close();
            fis.close();
    
            System.out.println("==============执行结束==============");
            System.out.println();
        }
    
        /**
         * 分发文件
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:25
         * @Param [file]
         * @return void
         **/
        public static void classification(File file) throws IOException {
            String fileName = file.getName();
            String toUrl="";
    
            String[] fName = fileName.split("\\.");
            String endName = "."+fName[(fName.length-1)];
            System.out.println("=========================================================="+endName);
    
    
            for (Map.Entry<String, String> stringEntry : fenLei) {
                String strKye = stringEntry.getKey();
    
                JSONObject leiBie = json.getJSONObject("分类").getJSONObject(strKye);
                if (leiBie.containsValue(endName)){
                    toUrl = toSrc+"/"+strKye+"/";
                }
            }
    
    
            if ("".equals(toUrl)){
                toUrl = toSrc+"/其他文件/";
            }
            isHave(toUrl);
            copy(file,toUrl);
    
        }
    
        /**
         * 是否存在，不存在建立文件夹
         * @Author chenpeng
         * @Description //TODO
         * @Date 16:59
         * @Param [srcs]
         * @return void
         **/
        public static void isHave(String srcs){
            File gotoFile = new File(srcs);
            if (!gotoFile.exists()){
                gotoFile.mkdirs();
            }
        }
    
    
    }

```

配置文件
```json
{
  "路径":{
    "原始路径":"C:\\Users\\81022\\Downloads",
    "目标路径":"F:\\普通文档\\归档"
  },
  "分类":{
    "文本": {
      "txt": ".txt",
      "docx": ".docx",
      "doc": ".doc",
      "xls": ".xls",
      "xlsx": ".xlsx",
      "ppt": ".ppt",
      "pptx": ".pptx"
    },
    "视频": {
      "mp4": ".mp4",
      "mov": ".mov"
    },
    "代码文件": {
      "sql": ".sql",
      "war": ".war",
      "class": ".class",
      "jar": ".jar"
    },
    "图片": {
      "psd": ".psd",
      "jpg": ".jpg",
      "png": ".png"
    },
    "压缩包": {
      "zip": ".zip",
      "rar": ".rar",
      "7z": ".7z",
      "tar":".tar",
      "gz": ".gz"
    },
    "可执行文件": {
      "exe": ".exe",
      "msi": ".msi"
    }
  }
}

```

# 3. java 编码，序列化流与反序列化流
## 1 编码
### 1.1 字符编码表(字符集)
 1. ASCII 码表：    
保存了数字，字母等   
A - 65，a - 97，0 - 48
 2. GB2312 码表：
    保存了常用的汉字（6-7千个），一个中文占两个字节，且都为负数
    
 3. GBK 码表：
    保存了基本所有的汉字（20000多个），不管中文还是英文都为2个字节，这两个字节可为正负
 
 4. Unicode 码表：  
 统一码标(万国码标)     
 不管是中文还是英文都是两个字节
 
 5. UTF-8 码表：   
 一个字节就可以存储的数据不用两个字节存储
 这个码表更加标准化，在每一个字节头加入了编码信息   
 
 6. ISO-8859-1 码表：  
 拉丁码表   
 Tomcat默认编码     
 都是负数
 
 
 在GBK中一个中文两个字节  
 在UTF-8中一个中文三个字节

## 2 编码转换流
## 2.1 OutputStreamWrite
> 可以写一个字符串，字符数组     
> 是字符流向字节的桥梁


### 2.2 OutputStreamWrite
> 可以接收一个字符串，字符数组     
> 是字节向字符流的桥梁

```java
public class streamDemo {
    @Test
    public void outputStreamDemo() throws IOException {
        File file = new File("");
        String src = file.getAbsolutePath()+"/src/com/looc/demo10/1.txt";
        OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream(src),"UTF-8");

        writer.write("啦啦啦啦");

        writer.close();
    }

    @Test
    public void inputStreamReader() throws IOException{
        File file = new File("");
        String src = file.getAbsolutePath()+"/src/com/looc/demo10/1.txt";

        InputStreamReader reader = new InputStreamReader(new FileInputStream(src),"utf-8");
        char[] chars = new char[4];
        int len = 0;
        String str;
        while ((len = reader.read(chars))!=-1){
            str = new String(chars,0,len);
            System.out.print(str);
        }

        reader.close();
    }
}

```

> InputStreamReader 是 fileReader 的父类   
> fileReader 封装的时候就设置默认编码为GBK了

> OutputStreamWrite 是 fileWrite 的父类     
> fileWrite 封装的时候就设置了默认编码为UTF-8

## 3. 序列化流与反序列化流
### 3.1 序列化流 
写对象到文件  
ObjectOutputStream

1. 构造：
    ObjectOutPutStream(FileOutputStream out);
2. 方法：
    writeObject(Object obj);//obj这个对象需要实现Serializable接口
    > Serializable 启标记作用
### 3.2 反序列化流
从文件中读取对象    
ObjectInputStream

1. 构造：
    ObjectInPutStream(FileInputStream out);
2. 方法：
    Object readeObject();
 
 //之前写完对象之后，如果修改所属对象，原来的类即失效，这个时候就会出现无效类异常      
 
> JVM判断一个类是否有效，通过一个类的版本号判断  
> 当写完一个类的时候，内部有一个版本号，当你修改这个类的时候，版本号就会随之发生变化     
> private static final long serialVersionUID

#### transient 关键字
> 用来修饰成员变量，使不使用transient修饰对成员变量完全没有影响   
> 但是序列化的时候有用，如果一个成员被transient修饰，那么序列化的时候就会忽略改成员变量


## 4 打印流
> 打印字符流 打印字符流   
> 两个打印东西方法一样    
> 使用几乎一样，底层不同

### 4.1 PrintWriter 打印字符流
可以打印到：
1. 字符串的文件名
2. FIle对象
3. 其他的OutputStream流
4. 其他的Writer流

### 4.2 PrintStream 打印字符流
可以打印到
1. 字符串的文件名
2. FIle对象
3. 其他的OutputStream流


# 4. java commonsIO
## 1 commons-IO

### 1.1 FileUtils
1. readFileToString(File file); 读取文件内容，返回一个String


2. writeStringToFile(File file, String content); 将字符串写入file中


3. copyFile(File srcFile, FIle destFile); 文件复制


4. copyDirectoryToDirectory(File srcDir,File destDir); 文件夹复制    


demo:
```java
public class CommonsDemo {
    String path = "";
    @Before
    public void getPath(){
        File file = new File("");
        path = file.getAbsolutePath()+"/src/com/looc/demo10/commonsIODemo";
    }


    @Test
    public void commWrit() throws IOException {
        FileUtils.writeStringToFile(new File(path+"/测试.txt"),"这里是测试第一行","utf-8",true);
    }


    @Test
    public void commRead() throws IOException{
        String readStr = FileUtils.readFileToString(new File(path+"/测试.txt"),"utf-8");
        System.out.println(readStr);
    }

    @Test
    public void commCopyFile() throws IOException{
        FileUtils.copyFile(new File("C:\\Users\\81022\\Downloads\\commons-io-2.6.jar"),new File(path+"/test.jar"));
    }

    @Test
    public void commCopyDirectory() throws IOException{
        FileUtils.copyDirectory(new File(""),new File(""));
    }

    @Test
    public void commDemo() throws IOException{
        FileUtils.forceMkdir(new File("C:\\Users\\81022\\Downloads\\aaa\\xxx"));
        FileUtils.moveFileToDirectory(new File(""),new File(""),true);
        FileUtils.deleteDirectory(new File(""));
    }
}

```

