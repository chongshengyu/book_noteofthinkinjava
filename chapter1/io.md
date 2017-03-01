# IO流

IO流用来处理设备之间的数据传输。Java对数据的操作是通过`流` 的方式，用于操作流的类都在IO包中。

**IO流分类**

* 按流向分为输入流、输出流。硬盘读到程序是输入流，程序输出到硬盘是输出流。
* 按数据类型分为字节流和字符流。

# 常用基类

**字节流\(java.io.OutputStream,java.io.InputStream\)**

* `public abstract class OutputStream extends Object implements Closeable,Flushable`** **
* `public abstract class InputStream extends Object implements Closeable`** **

**字符流\(java.io.Writer,java.io.reader\)**

* `public abstract class Writerextends Objectimplements Appendable, Closeable, Flushable`

* `public abstract class Readerextends Objectimplements Readable, Closeable`

# 常用类

## FileOutputStream

`public class FileOutputStream extends OutputStream`

FileOutputStream用于写入诸如图像之类的原始的字节流。

### 构造方法

```java
FileOutputStream(File file, boolean append) 
FileOutputStream(String name, boolean append)
```

在执行完`FileOutputStream fos = new FileOutputStream("a.txt");` 后，就会创建出`a.txt` 文件，期间发生了这些事情：

* 调用操作系统功能创建文件
* 创建FileOutputStream对象
* 把FileOutputStream对象指向刚创建的文件

### 文件写入

```java
FileOutputStream fos = new FileOutputStream("e:a.txt");
fos.write("hello,world".getBytes());
fos.close();
```

**为什么要close\(\)**

* 让流对象关闭，变成垃圾
* 通知系统释放与该文件相关的资源

**字节输出流操作步骤**

* 创建字节输出流对象
* 写数据
* 释放资源

### 三个write\(\)方法

* public void write\(int b\)写一个字节
* public void write\(byte\[\] b\)写一个字节数组
* public void write\(byte\[\] b, int off, int len\)写一个字节数组的一部分

```java
FileOutputStream fos = new FileOutputStream("a.txt");
fos.write(97);//文件打开后显示字符a
fos.close();
```

```java
FileOutputStream fos = new FileOutputSteam("a.txt", true);//追加输入
fos.write("hello".getBytes());
fos.write({97,98,99});
fos.close();
```

```java
FileOutputStream fos = new FileOutputStream("a.txt", true);
byte[] bytes = {97,98,99,100};
fos.write(byte,0,3);
fos.close();
```

**如何实现换行？**

`fos.write("\r\n".getBytes());` windows系统换行为`\r\n` ，Linux系统为`\n` 。

**Demo**

```java
/**
 * 创建文件，并用字节流写入字符
 * @author YCS
 *
 */
public class FileTest {
    public static void main(String[] args){
        File file = new File("E:/test");
        file.mkdirs();
        File txtFile = new File(file,"a.txt");
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream(txtFile);
            fos.write("hello\r\nworld".getBytes());
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }finally{
            if(fos != null){//fos有可能还没创建出来，比如文件不存在时。
                try {
                    fos.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## FileInputStream

`FileInputStream` 用于读取诸如图像数据之类的原始字节流。

步骤：

* 创建字节输入流对象
* 调用`read()` 方法读取数据
* 释放资源

### 三个read\(\)方法

* `int read()` 一次读一个字节，返回-1为文件末尾
* `int read(byte[] b)` 一次读取一个字节数组，返回本次读入的字节数
* `int read(byte[] b, int off, int len)` 一次读取一个字节数组，指定起始和结束索引。

### 文件复制

```java
/**
 * 文件拷贝
 * @author YCS
 *
 */
public class FileTest {
    public static void main(String[] args){
        File src = new File("E:/test");
        File dest = new File("E:/test/tohere");
        copy(new File(src,"test.png"),new File(dest,"test2.png"));
        System.out.println("done");
    }

    public static void copy(File src, File dest){
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(src);
            fos = new FileOutputStream(dest);
            byte[] buffer = new byte[2014];
            int length = 0;
            while((length = fis.read(buffer)) != -1){
                fos.write(buffer, 0, length);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally{
            if(fos != null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fis != null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

**Tips**

计算机怎么识别中文字符？

```java
String s = "ab哈哈cd";
byte[] bytes = s.getBytes();
System.out.println(Arrays.toString(bytes));
//[97, 98, -27, -109, -120, -27, -109, -120, 99, 100]
```

中文字符的字节码的第一个字节是负的，把负的2个\(unicode\)或3个\(utf8\)拼一块得到中文字符。示例中使用utf8。

**\r 和\n 各占一个字符**

## 字节缓冲流

字节流一次读写一个数组比一次读写一个字节快很多，这是加入数组这种缓冲区的效果**（装饰设计模式）** ，java还提供了字节缓冲区流。`BufferedOutputStream` ,`BufferedInputStream` 。

### BufferedOutputStream构造方法

* `BufferedOutputStream(OutputStream)` 创建一个新的缓冲输出流，以将数据写入指定的底层输出流
* `BufferedOutputStream(OutputStream out, int size)` 指定缓冲区大小

为什么不传一个具体的文件，而是一个OutputStream对象？

字节缓冲区流仅仅提供缓冲区，为高效而设计。真正的读写操作，还要靠基本的流对象实现。

```java
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("e:\\TestData\\b.txt"));
bos.write("hello".getBytes());
bos.close();//关闭这个即可
```

### BufferedInputStream构造方法

* `BufferedInputStream(InputStream in)` 
* `BufferedInputStream(InputStream in, int size)` 

### 字节

运行时间比较：long time = System.cuttentTimeMillis\(\);

