# File

位于`Java.io.File` ,`public class File extends Object implements Serializable,Comparable` ,File是文件和路径名的抽象表示形式（未必真实存在）

## 构造方法

`File(String pathName)` 根据路径构造File对象

`File(String paraent, String child)` 根据一个目录和一个子目录（或文件）构造File对象

`File(Fille parent, String child)` 使用父File对象和一个子文件（或目录）构造File对象

`File(URI uri)`

注意：若创建目录或文件时，没有写明绝对路径，则默认放在项目根目录下。

## 常用API

```java
public boolean delete()//彻底删除，不放回收站；只能从底层开始删除，不能级联删除
public boolean renameTo(File dest)
public boolean isDirectory()
public boolean isFile()
public boolean exists()
public boolean canRead()
public boolean canWrite()
public boolean isHidden()
public boolean createNewFile()//当且仅当不存在具有此抽象路径名指定名称的文件时，不可分地创建一个新的空文件。
public boolean mkdir()//创建抽象路径名指定的目录
public boolean mkdirs()//创建抽象路径名指定的目录，包括所有必需但不存在的父目录
public String getAbsolutePath()
public String getPath()//将此抽象路径名转换为一个路径名字符串
public String getName()//返回由此抽象路径名表示的文件或目录的名称
public long length()//以字节为单位
public long lastModified()
public String[] list()
public File[] listFiles()
public String[] list(FilenameFilter filter)
public File[] listFiles(FilenameFilter filter)
```

**Tips**

* 截取文件名时，`File.getName().split("\\.")[0]` 点号前面要加下划线

## Demo

```java
/**
 * 在不同深度的目录中新建 jpg格式文件，并递归重命名为jpeg格式
 * @author YCS
 *
 */
public class FileTest {
    public static void main(String[] args){
        File file = new File("E:/test/test/testfile");
        file.mkdirs();//要先创建目录，在创建文件
        File file1 = new File("E:/test/a.jpg");
        File file2 = new File("E:/test/test/testfile/b.jpg");
        File file3 = new File("E:/test/test/testfile/c.jpg");
        File file4 = new File("E:/test/test/testfile/d.tmp");

        try {
            file1.createNewFile();
            file2.createNewFile();
            file3.createNewFile();
            file4.createNewFile();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        setSuffix(new File("E:/test"),"jpg","jpeg");
    }
    public static void setSuffix(File file, final String src, final String dest){

        File[] jpgFiles = file.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                File fileTmp = new File(dir,name);
                if(fileTmp.isDirectory()){
                    setSuffix(fileTmp,"jpg","jpeg");
                }else{
                    if(name.split("\\.")[1].equals(src)){
                        return true;
                    }
                }
                return false;
            }
        });
        for(File f:jpgFiles){
            f.renameTo(new File(file.getPath(),f.getName().split("\\.")[0]+"."+dest));//修改后缀名
        }
    }
}
```

# IO 



