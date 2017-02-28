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

### 文件



