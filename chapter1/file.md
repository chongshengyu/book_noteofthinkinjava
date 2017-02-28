# File

位于`Java.io.File` ,`public class File extends Object implements Serializable,Comparable` ,File是文件和路径名的抽象表示形式（未必真实存在）

## 构造方法

`File(String pathName)` 根据路径构造File对象

`File(String paraent, String child)` 根据一个目录和一个子目录（或文件）构造File对象

`File(Fille parent, String child)` 使用父File对象和一个子文件（或目录）构造File对象

注意：若创建目录或文件时，没有写明绝对路径，则默认放在项目根目录下。

## 常用



