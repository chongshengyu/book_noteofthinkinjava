\# 转换流

字节流操作中文不方便，所以Java提供了转换流。字符流 = 字节流 + 编码表

\#\# 编码表

ASCII码表

'a' 97

'A' 65

'0' 48

8位。最高位为符号位，其余为数值位。



ISO-8859-1，拉丁码表。8位表示一个数据

GB2312，简体中文

GBK，中文编码表的扩充。

GB18030，GBK的取代版本。

Unicode：国际标准码，两个字节表示一个字符。Java语言使用Unicode。

UTF8：最多用三个字节表示。



\#\# String 指定字符集

以下是String类的两个方法：

`byte[] getBytes(String charsetName)`：使用指定的字符集合把字符串编码为字节数组

\`String\(byte\[\] bytes, String charsetName\)\`：通过指定的字符集解码字节数组



\#\# 转换流

字节流转字符流：

OutputStreamWriter\(OutputStream out\)：用默认编码把字节流数据转换为字符流。

OutputStreamWriter\(OutputStream out, String charsetName\)：用指定编码把字节流转换为字符流。

字符流 = 字节流 + 编码。



public void write\(int c\)：写一个字符

public void write\(char\[\] cbuf\)：写字符数组

public void write\(char\[\] cbuf, int off, int len\)

public void write\(String str\)：写字符串

public void write\(String str, int off, int len\)

\`\`\`

OutputStreamWriter osw = new OutputStreamWriter\(new FileOutputStream\("a.txt"\)\);

osw.write\("中国aa"\);

osw.write\('中'\);

osw.close\(\);

\`\`\`

写字符流必须flush\(\)刷缓冲区，才能写入。直接close\(\)也可以。

close\(\)关闭流对象，但要先刷新缓冲区。关闭后，流对象不可用，不可写入。而flush\(\)仅刷新缓冲区。



InputStreamReader\(InputStream is\)

InputStreamReader\(InputStream is, String charsetName\)



int read\(\)：一次读一个字符

int read\(char\[\] chs\)：一次读一个字符数组



\`\`\`

InputStreamReader isr = new InputStreamReader\(new FileInputStream\("a.txt"\)\);

int ch = 0;

while\(\(ch=isr.read\(\)\)!=-1\){

    System.out.print\(\(char\)ch\);

}

\`\`\`

\`\`\`

InputStreamReader isr = new InputStreamReader\(new FileInputStream\("a.txt"\)\);

char\[\] chs = new char\[1024\];

int len = 0;

while\(\(len=isr.read\(chs\)\)!=-1\){

    System.out.print\(new String\(chs,0,len\)\);

}

\`\`\`



文本文件复制

\`\`\`

InputStreamReader isr = new InputStreamReader\(new FileInputStream\("a.txt"\)\);

OutputStreamWriter osw = new OutputStreamWriter\(new FileOutputStream\("b.txt"\)\);

char\[\] chs = new char\[1024\];

int len = 0;

while\(\(len=isr.read\(chs\)\)!=-1\){

    osw.write\(chs,0,len\);

}

osw.close\(\);

isr.close\(\);

\`\`\`



\#\# 转换流的简化写法

为了简化书写，转换流提供了对应的子类。

FileWriter，继承OutputStreamWriter

FileReader，继承InputStreamReader

FileWriter和FileReader不能指定编码方式，选用平台默认方式。



构造方法

FileWriter\(File file\)

FileWriter\(File file, boolean append\)

FileWriter\(String fileName\)

FileWriter\(String fileName, boolean append\)

\`\`\`

FileReader fr = new FileReader\("a.txt"\);

FileWriter fw = new FileWriter\("b.txt"\);

int len = 0;

char\[\] chs = new char\[1024\];



while\(\(len=fr.read\(chs\)\)!=-1\){

    fw.write\(chs,0,len\);

}

fw.close\(\);

fr.close\(\);

\`\`\`



\#\# 字符缓冲流

BufferedWriter 将文本写入字符输出流，缓冲各个字符，实现单个字符、数字和字符串的高效写入。

BufferedReader 从字符输入流中读取文本，缓冲各字符，实现单个字符、数字和字符串的高效读取。

\`\`\`

BufferedReader br = new BufferedReader\(new FileReader\("a.txt"\)\);

BufferedWriter bw = new BufferedWriter\(new FileWriter\("b.txt"\)\);



int len = 0;

char\[\] chs = new char\[1024\];

while\(\(len = br.read\(chs\)\)!=-1\){

    bw.write\(chs,0,len\);

}

bw.close\(\);

br.close\(\);

\`\`\`



\#\# 缓冲流的特殊功能

void BufferedWriter.newLine\(\)：写入一个行分隔符。根据系统决定换行符

String BufferedReader.readLine\(\)：一次读取一行数据。不包含终止符。到达流末尾返回null

\`\`\`

BufferedReader br = new BufferedReader\(new FileReader\("a.txt"\)\);

BufferedWriter bw = new BufferedWriter\(new FileWriter\("b.txt"\)\);



String s = null;

while\(\(s = br.readLine\(\)\)!=null\){

    bw.write\(s\);

    bw.newLine\(\);

    bw.flush\(\);//手动刷新

}

bw.close\(\);

br.close\(\);

\`\`\`



字符流复制文件有5种方法：

基本2种（还有一类简写），高效2种，高效特殊1种。



\#\# 例题

\#\#\# 复制文本文件的5种方式

用记事本打开能读懂，所以用字符流方便一些。字符流有5种方式

OutputStreamWriter（2种） FileWriter（2种） BufferedWritr（1种）

\`\`\`

public class Test {

    public static void main\(String\[\] args\) throws IOException {

        method1\(\);

        method2\(\);

        method3\(\);

        method4\(\);

        method5\(\);

    }



    private static void method5\(\) throws IOException{

        //高效字符流特殊功能

        BufferedWriter bw = new BufferedWriter\(new FileWriter\("b.txt"\)\);

        BufferedReader br = new BufferedReader\(new FileReader\("a.txt"\)\);



        String s = null;

        while\(\(s = br.readLine\(\)\)!=null\){

            bw.write\(s\);

            bw.newLine\(\);

        }

        bw.close\(\);

        br.close\(\);

    }



    private static void method4\(\) throws IOException{

        //高效字符流一次读一个字符数组

        FileWriter fw = new FileWriter\("b.txt"\);

        FileReader fr = new FileReader\("a.txt"\);



        int len = 0;

        char\[\] chs = new char\[1024\];

        while\(\(len = fr.read\(chs\)\)!=-1\){

            fw.write\(chs, 0, len\);

        }

        fw.close\(\);

        fr.close\(\);

    }



    private static void method3\(\) throws IOException{

        //高效字符流一次读一个字符

        FileWriter fw = new FileWriter\("b.txt"\);

        FileReader fr = new FileReader\("a.txt"\);



        int out = 0;

        while\(\(out = fr.read\(\)\)!=-1\){

            fw.write\(out\);

        }

        fw.close\(\);

        fr.close\(\);

    }



    private static void method2\(\) throws IOException{

        //基本字符流一次读一个字符数组

        InputStreamReader isr = new InputStreamReader\(\(new FileInputStream\("a.txt"\)\)\);

        OutputStreamWriter osw = new OutputStreamWriter\(new FileOutputStream\("b.txt"\)\);



        int len = 0;

        char\[\] chs = new char\[1024\];

        while\(\(len = isr.read\(chs\)\)!=-1\){

            osw.write\(chs,0,len\);

        }

        isr.close\(\);

        osw.close\(\);

    }



    private static void method1\(\) throws IOException{

        //基本字符流一次读一个字符

        InputStreamReader isr = new InputStreamReader\(new FileInputStream\("a.txt"\)\);

        OutputStreamWriter osw = new OutputStreamWriter\(new FileOutputStream\("b.txt"\)\);



        int out = 0;

        while\(\(out=isr.read\(\)\)!=-1\){

            osw.write\(out\);

        }

        osw.close\(\);

        isr.close\(\);

    }

}

\`\`\`

\#\#\# 复制图片的4种方式

FileOutputStream（2种） BufferedOutputStream（2种）

\`\`\`

public class Test {

    public static void main\(String\[\] args\) throws IOException {

        method1\(\);

        method2\(\);

        method3\(\);

        method4\(\);

    }



    private static void method4\(\) throws IOException{

        BufferedInputStream bis = new BufferedInputStream\(new FileInputStream\("a.png"\)\);

        BufferedOutputStream bos = new BufferedOutputStream\(new FileOutputStream\("b.png"\)\);



        int len = 0;

        byte\[\] bytes = new byte\[1024\];

        while\(\(len = bis.read\(bytes\)\)!=-1\){

            bos.write\(bytes, 0, len\);

        }

        bos.close\(\);

        bis.close\(\);

    }



    private static void method3\(\) throws IOException{

        BufferedInputStream bis = new BufferedInputStream\(new FileInputStream\("a.png"\)\);

        BufferedOutputStream bos = new BufferedOutputStream\(new FileOutputStream\("b.png"\)\);



        int out = 0;

        while\(\(out = bis.read\(\)\)!=-1\){

            bos.write\(out\);

        }

        bos.close\(\);

        bis.close\(\);

    }



    private static void method2\(\) throws IOException{

        FileOutputStream fos = new FileOutputStream\("b.png"\);

        FileInputStream fis = new FileInputStream\("a.png"\);



        int len = 0;

        byte\[\] bytes = new byte\[1024\];

        while\(\(len = fis.read\(bytes\)\)!=-1\){

            fos.write\(bytes,0,len\);

        }

        fos.close\(\);

        fis.close\(\);

    }



    private static void method1\(\) throws IOException{

        FileOutputStream fos = new FileOutputStream\("b.png"\);

        FileInputStream fis = new FileInputStream\("a.png"\);



        int out = 0;

        while\(\(out = fis.read\(\)\)!=-1\){

            fos.write\(out\);

        }

        fos.close\(\);

        fis.close\(\);

    }

}

\`\`\`



\#\#\# 集合中的元素存到文本文件

FileWriter,BufferedWriter

\`\`\`

ArrayList&lt;String&gt; arrayStr= new ArrayList&lt;&gt;\(\);

arrayStr.add\("Hello"\);

arrayStr.add\("Java"\);

arrayStr.add\(",Hello"\);

arrayStr.add\("世界"\);



FileWriter fw = new FileWriter\("out.txt"\);

for\(String s:arrayStr\){

    fw.write\(s\);

}

fw.write\("\r\n"\);



BufferedWriter bw = new BufferedWriter\(new FileWriter\("out.txt",true\)\);

for\(String s:arrayStr\){

    fw.write\(s\);

}



fw.close\(\);

bw.close\(\);

\`\`\`

\#\#\# 文本文件到集合

\`\`\`

ArrayList&lt;String&gt; arrayStr= new ArrayList&lt;&gt;\(\);

BufferedReader br = new BufferedReader\(new FileReader\("out.txt"\)\);



String s = null;

while\(\(s = br.readLine\(\)\)!=null\){

    arrayStr.add\(s\);

}

br.close\(\);

for\(String ss : arrayStr\){

    System.out.println\(ss\);

}

\`\`\`



\#\#\# 复制单级目录

\`\`\`

File src = new File\("test"\);

File dest = new File\("test1"\);

if\(!dest.exists\(\)\)

    dest.mkdir\(\);



File\[\] files = src.listFiles\(\);

BufferedOutputStream bos = null;

BufferedInputStream bis = null;



for\(File f:files\){

    bis = new BufferedInputStream\(new FileInputStream\(f\)\);

    bos = new BufferedOutputStream\(new FileOutputStream\(new File\(dest,f.getName\(\)\)\)\);



    int len = 0;

    byte\[\] bytes = new byte\[1024\];

    while\(\(len = bis.read\(bytes\)\)!=-1\){

        bos.write\(bytes,0,len\);

    }

    bos.close\(\);

    bis.close\(\);

}

\`\`\`



\#\#\# 把单级目录下的所有java文件拷贝到另一目录，并重命名后缀名为jad



\`\`\`

public class Test {

    public static void main\(String\[\] args\) throws IOException {

        File src = new File\("test"\);

        File dest = new File\("test1"\);

        if\(!dest.exists\(\)\)

            dest.mkdir\(\);



        File\[\] files = src.listFiles\(new FilenameFilter\(\) {

            @Override

            public boolean accept\(File dir, String name\) {

                if\(new File\(dir,name\).isFile\(\) && name.endsWith\(".java"\)\)

                    return true;

                return false;

            }

        }\);



        for\(File f:files\){

            copyFile\(f,new File\(dest,f.getName\(\).replace\(".java",".jad"\)\)\);

        }

    }



    private static void copyFile\(File src, File dest\) throws IOException{

        BufferedOutputStream bos = new BufferedOutputStream\(new FileOutputStream\(dest\)\);

        BufferedInputStream bis = new BufferedInputStream\(new FileInputStream\(src\)\);



        int len = 0;

        byte\[\] bytes = new byte\[1024\];

        while\(\(len = bis.read\(bytes\)\)!=-1\){

            bos.write\(bytes, 0, len\);

        }

        bos.close\(\);

        bis.close\(\);

    }

}

\`\`\`



\#\#\# 复制多级目录

\`\`\`

public class Test {

    public static void main\(String\[\] args\) throws IOException {

        File src = new File\("test"\);

        File dest = new File\("test1"\);

        copyFolder\(src,dest\);

    }

    private static void copyFolder\(File src, File dest\) throws IOException{

        if\(!dest.exists\(\)\)

            dest.mkdir\(\);

        File\[\] files = src.listFiles\(\);

        for\(File f:files\){

            if\(f.isDirectory\(\)\)

                copyFolder\(new File\(src, f.getName\(\)\),new File\(dest,f.getName\(\)\)\);

            else if\(f.isFile\(\)\)

                copyFile\(new File\(src,f.getName\(\)\),new File\(dest,f.getName\(\)\)\);

        }

    }



    private static void copyFile\(File src, File dest\) throws IOException{

        BufferedOutputStream bos = new BufferedOutputStream\(new FileOutputStream\(dest\)\);

        BufferedInputStream bis = new BufferedInputStream\(new FileInputStream\(src\)\);



        int len = 0;

        byte\[\] bytes = new byte\[1024\];

        while\(\(len = bis.read\(bytes\)\)!=-1\){

            bos.write\(bytes, 0, len\);

        }

        bos.close\(\);

        bis.close\(\);

    }

}

\`\`\`

\#\#\# 继承关系

\`\`\`

OutputStream

\|---FileOutputStream

	\|---BufferedOutputStream



-------------------------

InputStream

\|---FileInputStream

	\|---BufferedInputStream



-------------------------

Writer

\|---BufferedWriter	

\|---OutputStreamWriter

	\|---FileWriter



--------------------------

Reader

\|---BufferedReader

\|---InputStreamReader

	\|---FileReader

\`\`\`

\#\#\# 键盘录入学生成绩信息并按总分增序输出到文件

可以用TreeSet和Compatator做。





\#\#\# 文件中的字符串排序后输出到文件

\#\#\# 用Reader模拟BufferedReader的readLine\(\)功能



21.27之后都没有看。

 







