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

### 文件



