---
title: IO流
date: 2020-05-25 10:15:43
tags:
- Java
categories:
- Java
mathjax: true
---

## 什么是IO?

I：Input

O：Output

通过IO可以完成硬盘文件的读和写



## IO流分类

有多种分类方式：

- 按照流的方向进行分类，以内存为参照物：
  - 往内存中去，叫做输入，或者叫做读，叫做**输入流**
  - 从内存中出来，叫做输出，或者叫做写，叫做**输出流**
- 按照读取数据方式不同分类：
  - 按照字节的方式读取数据，一次读取一个字节byte，等同于一次读取8个二进制位。这种流是万能的，什么类型的文件都可以读取，叫做**字节流**
  - 按照字符的方式读取数据，一次读取一个字符，**为了方便读取普通文本文件(txt文件、java文件等，能用记事本编辑的都是普通文本文件，与文件后缀无关)**，不能读取图片、声音、视频等文件，只能读取文本文件，连word文件都读取不了，叫做**字符流**

**Java中所有的流都是在java.io.*包下**



IO流的四大家族：

- java.io.InputStream：字节输入流
- java.io.OutputStream：字节输出流
- java.io.Reader：字符输入流
- java.io.Writer：字符输出流

上述为四大家族的首领，它们都是**抽象类**。**所有的流都实现了java.io.Closeable接口**，都是可关闭的，都有close()方法。流毕竟是一个管道，是内存和硬盘之间的通道，用完一定要关闭（养成这个好习惯），不然会占用很多资源

**所有的输出流都实现了java.io.Flushable接口**，都是可刷新的，都有flush()方法。养成一个好习惯，输出流在最终输出之后，一定要记得flush()刷新一下，刷新的作用就是清空管道，将管道中剩余未输出的数据强行输出！如果没有flush可能会丢失数据

**注意：在java中只要类名以Stream结尾的都是字节流，以Reader/Writer结尾的都是字符流**



java.io包下需要掌握的流有16个：

- 文件专属：
  - java.io.FileInputStream
  - java.io.FileoutputStream
  - java.io.FileReader
  - java.io.FileWriter
- 转换流：将字节流转换为字符流
  - java.io.InputStreamReader
  - java.io.OutputStramWriter
- 缓冲流专属：
  - java.io.BufferedReader
  - java.io.BufferedWriter
  - java.io.BufferedInputStream
  - java.io.BufferedOutputStream
- 数据流专属：
  - java.io.DataInputStream
  - java.io.DataOutputStream
- 对象专属流：
  - java.io.ObjectInputStream
  - java.io.ObjectOutputStream
- 标准输出流：
  - java.io.PrintWriter
  - java.io.PrintStream



## FileInputStream

- **文件字节输入流**，万能的，任何类型的文件都可以采用这个流来读
- 字节的方式，完成读的操作，从硬盘到内存
- 下面的代码是FileInputStream标准架子，要学会搭

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileInputStream file = null;
        try {
            //写成这个也是可以的，都是绝对路径
            //file = new FileInputStream("G:/test.txt");
            //这里也可以用相对路径，默认目录是此工程project的根目录！！！（每个工程下有很多模块）
            //比如("test.txt")表示在根目录下寻找该文件
            //如果不采用默认目录，一定是从该文件当前所在的位置作为起点开始找
            //例如("src/com/info.txt")表示在根目录下的src下的com中寻找info.txt文件
            file = new FileInputStream("G:\\test.txt"); //有异常，处理方式为catch (FileNotFoundException e)
            //read方法的返回值是读取到的"字节"本身,即每调用一次read方法，从FileInputStream中读取一个字节
            //如果已达到文件末尾，返回-1
            //利用while可以循环读，终止条件是返回值为-1
            int readData = file.read();//有异常，处理方式为catch (IOException e)
            System.out.println(readData);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //在finally语句块中确保流一定关闭
            if(file!=null){
                //关闭流的前提：流不是null
                try {
                    file.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

循环读取数据写法：

```java
int readData = 0;
while((readData=file.read())!=-1){
    System.out.println(readData);
}
```

上述代码存在缺点：一次读取一个字节，这样内存和硬盘交互太频繁，基本上时间都耗费在交互上面了。能不能一次读取多个字节？**可以！**

- **int read(byte[] b)**：一次最多读取b.length个字节，往byte[]数组当中读，减少交互次数，提高读取效率

```java
byte[] bytes = new byte[4];
//文件里面的内容为：abcdef
//返回值为读取到的字节的数量，不是字节本身
int readCount = file.read(bytes);
System.out.println(readCount);//返回4，第一次读取到了4个字节

int readCount = file.read(bytes);
System.out.println(readCount);//返回2，第一次读取到了2个字节。同时bytes数组前两个元素会被这次读取的元素覆盖

int readCount = file.read(bytes);
System.out.println(readCount);//返回-1，1个字节都没有读到
```



**FileInputStream最终版**：下面的内容要掌握

```java
public static void main(String[] args) {
        FileInputStream file = null;
        try {
            file = new FileInputStream("G:\\test.txt");
            byte[] bytes = new byte[4];
            int readCount = 0;
            while((readCount = file.read(bytes)) != -1){
                //把byte数组转换成字符串，读到多少个，转换多少个
                System.out.print(new String(bytes,0,readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(file!=null){
                try {
                    file.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



### 其它常用方法

- int available()：**返回流当中剩余的没有读到的字节数量**。这个方法的用处：读文件在生成byte数组时可以提前指定好，就不用循环了（byte[] bytes = new byte[file.available()]）。但这种方式不适合大文件

```java
file = new FileInputStream("G:\\test.txt"); //文件中内容为：abcdef
int readData = file.read();
System.out.println("剩余未读字节数："+file.available());//剩余未读字节数：5
```

- long skip(long n)：跳过n个字节不读

```java
file = new FileInputStream("G:\\test.txt");
file.skip(3); //跳过3个字节不读
byte[] bytes = new byte[file.available()];
int readCount = file.read(bytes);
System.out.println(new String(bytes)); //def
```



## FileOutputStream

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileOutputStream file = null;
        try {
            //file文件不存在的时候，会自动新建
            //这种方式谨慎使用，因为会先将原文件清空再写入
            file = new FileOutputStream("G:\\test1.txt");
            //以追加的方式写入该怎么办？如下所示
            //file = new FileOutputStream("G:\\test1.txt",true);
            byte[] bytes = {97,98,99,100};
            //将byte数组全部写出
            file.write(bytes);//abcd
            //将byte数组一部分写出,第二个参数为起始位置，第三个参数为写出的长度
            file.write(bytes,0,2);//ab
            //写入一个整型数据，会把十进制转化成二进制，然后转化为字符写进记事本
            file.write(100);
            //如果想写入一个字符串，要调用getBytes方法
            String s = "hello,world!";
            byte[] b = s.getBytes();
            file.write(b);
            //写完之后，最后一定要刷新
            file.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally{
            if(file!=null) {
                try {
                    file.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## 文件复制

**以内存为中介**，利用FileInputStream和FileOutputStream一边读一边写实现复制。利用字节流拷贝文件的时候，文件类型随意，什么样的文件都可以拷贝

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileInputStream fin = null;
        FileOutputStream fout = null;
        try {
            fin = new FileInputStream("G:\\test.txt");
            fout = new FileOutputStream("G:\\test3.txt",true);

            //一边读一边写
            byte[] bytes = new byte[1024 * 1024];//一次最多拷贝1M
            int readCount = 0;
            while((readCount=fin.read(bytes))!= -1){
                fout.write(bytes,0,readCount);
            }
            fout.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //这里要分开try-catch，如果一起try-catch的话，上面的fin流如果关闭时出异常会导致fout流关闭不了
            if(fin!=null) {
                try {
                    fin.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fout!=null) {
                try {
                    fout.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



## FileReader

**文件字符输入流，只能读取普通文本文件**。读取文本内容时，比较快捷方便

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileReader file = null;
        try {
            file = new FileReader("G:\\test.txt");
            //这里是char数组，注意和FileInputStream的区别
            //一次读取一个字符
            char[] chars = new char[4];
            int readCount = 0;
            while((readCount=file.read(chars))!=-1){
                System.out.print(new String(chars,0,readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(file!=null) {
                try {
                    file.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## FileWriter

文件字符输出流，**只能输出普通文本**，负责写

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileWriter fout = null;
        try {
            fout = new FileWriter("G:\\test4.txt");
            //char数组全部输出
            char[] chars = {'我','爱','你'};
            fout.write(chars);
            //char数组部分数组
            fout.write(chars,1,2);
            //可以直接写字符串，也可以写入一部分，规则与char数组一致
            String s = "我是中国人";
            fout.write(s);
            //也可以换行
            fout.write("\n");
            fout.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            if(fout!=null) {
                try {
                    fout.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## 普通文本文件复制

用字节流也可以实现，不过我们这里用上面的字符流来实现。结构都是一致的

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileReader fin = null;
        FileWriter fout = null;
        try {
            fin = new FileReader("G:\\test.txt");
            fout = new FileWriter("G:\\test4.txt");
            char[] chars = new char[1024*1024];
            int readCount = 0;
            while((readCount=fin.read(chars))!=-1){
                fout.write(chars,0,readCount);
            }
            fout.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(fin!=null) {
                try {
                    fin.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fout!=null) {
                try {
                    fout.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## BufferedReader

带有缓冲区的字符输出流，使用这个流的时候**不需要自定义char数组，自带缓冲**。我们看一下该流：

```java
public class FileInputStramTest {
    public static void main(String[] args){
        FileReader fin = null;
        BufferedReader br = null;
        try {
            fin = new FileReader("G:/test.txt");
            //当一个流的构造方法中需要一个流的时候，这个被传进来的流叫做：节点流
            //外部负责包装的这个流，叫做：包装流，也叫作：处理流
            //像当前这个程序来讲：FileReader就是一个节点流，BufferedReader就是一个包装流
            //BufferedReader构造方法只能传参Reader字符流
             br = new BufferedReader(fin);

            //readLine()方法读取一行，String类型，且不读取最后的换行符
            //如果没有内容被读取了，该方法将返回null
            String s = null;
            while((s = br.readLine())!=null){
                System.out.println(s);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭流
            //对于包装流来说，只需要关闭最外层流就行，里面的节点流会自动关闭
            if(br!=null) {
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## InputStreamReader

转换流 java.io.InputStreamReader ，是Reader的子类，是从字节流到字符流的桥梁。**该类读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集**，默认跟编译工具或系统的编码方式一样（看是在IDE中运行还是在DOS窗口编译运行），IDE一般使用UTF-8，系统默认的编码为GBK。

注意其和FileReader的区别：

- InputStreamReader是底层先读取字节，然后通过指定或默认的字符集将其转换为字符流
- FileReader直接从文件中读取字符，采用的是文件的编码方式，而不能手动指定

**继承自父类的共性成员方法**

该类继承于 Reader 类，继承了父类的共性成员方法：

```java
int read() 读取单个字符并返回。
int read(char[] cbuf)一次读取多个字符,将字符读入数组。
void close() 关闭该流并释放与之关联的所有资源。
```

**构造方法**

```java
InputStreamReader(InputStream in) 创建一个使用默认字符集的InputStreamReader。InputStreamReader(InputStream in, String charsetName) 创建使用指定字符集的InputStreamReader。
```

参数：

InputStream in：字节输入流，用来读取文件中保存的字节

String charsetName:指定的编码表名称,不区分大小写,可以是utf-8/UTF-8,gbk/GBK,...不指定默认使用UTF-8

**注意**：**构造方法中指定的编码方式要和文件的编码方式相同,否则会发生乱码**

```java
public class FileInputStramTest {
    public static void main(String[] args) {
        FileInputStream fin = null;
        InputStreamReader in = null;
        try {
            fin = new FileInputStream("G:/test1.txt");
            in = new InputStreamReader(fin);
            char[] chars = new char[4];
            int readCount = 0;
            while((readCount=in.read(chars))!=-1){
                System.out.print(new String(chars,0,readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //只需要关闭最外层的包装流即可，里面的节点流会自动关闭
            if(in!=null) {
                try {
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



## BufferedWriter

带有缓冲的字符输出流

```java
public class encode {
    public static void main(String[] args) throws Exception {
        //BufferedWriter构造函数中要传入的是一个Writer
        //第一种
        BufferedWriter br1 = new BufferedWriter(new FileWriter("G:/copy.txt"));
        //第二种
        BufferedWriter br2 = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("G:/copy.txt",true)));
        br1.write("hello");
        br1.write("world");
        br1.write("\n");
        br1.flush();
        br2.write("hello");
        br2.write("kitty");
        br2.flush();
        br1.close();
        br2.close();
    }
}
```



## 数据专属流

- **DataOutputStream**

这个流可以将数据连同数据类型一并写入文件。**注意：这个文件不是普通文本文件（用记事本打不开）**

```java
public class encode {
    public static void main(String[] args) throws Exception {
        DataOutputStream dos = new DataOutputStream(new FileOutputStream("G:/data"));
        byte b = 10;
        short s = 200;
        int i = 300;
        //八中数据类型都有专门的写入方法
        dos.writeByte(b);
        dos.writeShort(s);
        dos.writeInt(i);
        dos.flush();
        dos.close();
    }
}
```

上述代码生成的data文件用记事本打开会出现乱码，因为它不是普通文本文件。这个时候该怎么办呢？就有用到它的好兄弟：DataInputStream

- **DataInputStream**

DataOutputStream写的文件，只能用DataInputStream去读，并且读的时候需要提前知道写入的顺序。读的顺序要和写的顺序一致，才可以正常取出数据

```java
public class encode {
    public static void main(String[] args) throws Exception {
        DataInputStream dos = new DataInputStream(new FileInputStream("G:/data"));
        byte b = dos.readByte();
        short s = dos.readShort();
        int i = dos.readInt();
        System.out.println(b);
        System.out.println(s);
        System.out.println(i);
        dos.close();
    }
}
/*
10
200
300
*/
```



## 标准输出流

这里只需要掌握PrintStream就可以了，**默认输出到控制台，也可以修改输出方向，叫做重定向**

```java
public class encode {
    public static void main(String[] args) throws Exception {
        //如果想在重定向后再改回默认输出至控制台。需要在最开始保存最原始的输出流
        PrintStream out = System.out;
        //标准输出流不需要手动close关闭
        PrintStream ps1 = System.out;
        ps1.println("hello");
        ps1.println("world");

        //也可以修改输出方向，后面的System.out.println都将输出至log文件
        PrintStream ps2 = new PrintStream(new FileOutputStream("G:/log"));
        System.setOut(ps2);
        System.out.println("hello");
        System.out.println("kitty");

        //重定向后恢复至默认输出控制台
        System.setOut(out);
        System.out.println("case");

    }
}
```

有的小伙伴可能要问，这个有啥用？**在写日志工具类输出日志的时候会很方便**

```java
//Logger.java
**
 * 日志工具类
 */
public class Logger {
    public static void log(String msg){
        try {
            //指向一个日志文件
            PrintStream ps = new PrintStream(new FileOutputStream("G:/log.txt",true));
            //改变输出方向
            System.setOut(ps);
            //日期当前时间
            Date nowTime = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
            String s = sdf.format(nowTime);
            System.out.println(s+ ":" + msg);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

//LogTest.java
public class LogTest {
    public static void main(String[] args) {
        //测试日志工具类是否好用
        //此时日志信息就会写入到log.txt文件里
        //写入的日志内容为：2020-05-28 11:56:06 948:发生异常
        Logger.log("发生异常");
    }
}
```



## File类

- File类和四大家族没有关系，它的父类是Object，所以File类不能完成文件的读和写
- File对象代表什么？
  - 代表文件和目录路径名的抽象表示形式
  - C:\Drivers是一个File对象
  - C:\Drivers\log.txt也是一个File对象
  - 一个File对象可能对应的是目录，也可能是文件
- File类常用方法：

```java
public class encode {
    public static void main(String[] args) throws Exception {
        //创建一个File对象
        File f1 = new File("D:/file");

        //判断是否存在，存在返回true，否则发挥false
        System.out.println(f1.exists());

        //如果D:\file不存在，则以文件的形式创建出来
        if(!f1.exists())
            f1.createNewFile();

        //如果D:\file不存在，则以目录的形式创建出来
        if(!f1.exists())
            f1.mkdir();

        //可以创建多重目录
        File f2 = new File("G:/A/B/C");
        if(!f2.exists())
            f2.mkdirs();

        File f3 = new File("G:/pytorch/log.txt");
        //获取文件的父路径
        //第一种方式
        String s = f3.getParent();
        System.out.println(s); //G:\pytorch
        //第二种方式
        File parentFile = f3.getParentFile();
        //获取绝对路径
        System.out.println(parentFile.getAbsolutePath());//G:\pytorch

        //获取当前文件名或目录名
        File f4 = new File("G:/pytorch/A/b/c");
        System.out.println(f4.getName()); //log.txt

        //判断是否是一个目录
        System.out.println(f4.isDirectory());//false

        //判断是否是一个文件
        System.out.println(f4.isFile());//true

        //获取文件最后一次修改时间,返回的是long，毫秒值，从1970到修改时间的毫秒数
        long m = f4.lastModified();
        Date time = new Date(m);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        String str = sdf.format(time);
        System.out.println(str);

        //获取文件大小,返回字节数
        System.out.println(f4.length());

        //获取当前目录下所有子文件或子目录
        File f5 = new File("G:/pytorch");
        File[] files = f5.listFiles();
        for(File file:files){
            System.out.println(file.getAbsolutePath());
        }
    }
}
```



下面讲一个**目录拷贝**的例子：把一个目录中的全部内容拷贝到另一个目录中

```java
public class encode {
    public static void main(String[] args) throws Exception {
        //拷贝源
        File srcFile = new File("G:/pytorch");
        //拷贝目标
        File destFile = new File("F:/");
        copyDir(srcFile,destFile);
    }

    //拷贝方法
    private static void copyDir(File srcFile, File destFile) {
        if(srcFile.isFile()){
            //srcFile如果是一个文件，递归结束
            FileInputStream in = null;
            FileOutputStream out = null;
            try {
                in = new FileInputStream(srcFile.getAbsolutePath());
                String path = (destFile.getAbsolutePath().endsWith("\\") ? destFile.getAbsolutePath() : destFile.getAbsolutePath() + "\\") + srcFile.getAbsolutePath().substring(3);
                out = new FileOutputStream(path);
                byte[] bytes = new byte[1024*1024];
                int readCount = 0;
                while((readCount=in.read(bytes))!=-1){
                    out.write(bytes,0,readCount);
                }
                out.flush();
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                if(in!=null) {
                    try {
                        in.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if(out!=null) {
                    try {
                        out.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
            return;
        }
        File[] files = srcFile.listFiles();
        //file可能是文件，也可能是目录
        for(File file:files){
            //建立目标目录
            if(file.isDirectory()){
                String srcDir = file.getAbsolutePath();
                String destDir = (destFile.getAbsolutePath().endsWith("\\") ? destFile.getAbsolutePath() : destFile.getAbsolutePath() + "\\") + srcDir.substring(3);
                File newFile = new File(destDir);
                if(!newFile.exists())
                    newFile.mkdirs();
            }
            copyDir(file,destFile);
        }
    }
}
```



## 对象流

**序列化**（Serialize）：内存中的java对象存储到文件中，将java对象的状态保存下来的过程，用ObjectOutputStream实现

**反序列化**（DeSerialize）：将硬盘上的数据重新恢复到内存中，恢复成java对象，用ObjectInputStream实现



**序列化的实现**

```java
//Student.java
public class Student implements Serializable {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

//SerializeTest.java
public class SerializeTest {
    public static void main(String[] args) throws Exception{
        //创建java对象
        Student s = new Student("zhangsan",20);
        //序列化,ObjectOutputStream是一个包装流，参数要求是一个OutputStream对象
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"));
        //序列化对象
        oos.writeObject(s);
        oos.flush();
        oos.close();
    }
}
```

注意：参与序列化和反序列化的对象必须实现Serializable接口，否则会出现异常：**java.io.NotSerializableException**

Serializable接口是一个**标志接口**（内部什么都没有！！）那么它有什么用呢？起到标志的作用，这个接口是给java虚拟机参考的，java虚拟机看到这个接口后，会为该类自动生成一个**序列化版本号**。



**反序列化实现**

```java
public class DeSerializeTest {
    public static void main(String[] args) throws Exception{
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("students"));
        //Student对象
        Object o = ois.readObject();
        System.out.println(o);
        ois.close();
    }
}
```



**一次序列化多个对象**

可以将对象放到集合当中，**序列化集合**

```java
public class SerializeTest {
    public static void main(String[] args) throws Exception{
        //创建java对象
        Student s1 = new Student("zhangsan",20);
        Student s2 = new Student("lisi",23);
        Student s3 = new Student("wangwu",11);
        List<Student> list = new ArrayList<>();
        list.add(s1);
        list.add(s2);
        list.add(s3);
        //序列化
        //参与序列化的ArrayLis以及集合中的元素都需要实现Serializable接口
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"));
        //序列化对象
        oos.writeObject(list);
        oos.flush();
        oos.close();
    }
}
```

**反序列化集合**

```java
public class DeSerializeTest {
    public static void main(String[] args) throws Exception{
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("students"));

        //返回的对象是一个List集合
        Object o = ois.readObject();
        //System.out.println(o instanceof List);//true
        //强转
        List<Student> lists = (List<Student>)o;
        for(Student list:lists){
            System.out.println(list);
        }
        ois.close();
    }
}
/*
Student{name='zhangsan', age=20}
Student{name='lisi', age=23}
Student{name='wangwu', age=11}
*/
```



## transient关键字

如果有些属性不想被序列化怎么办？用Transient修饰

```java
public class Student implements Serializable {
    //transient表示游离的，不参与序列化
    private transient String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

上述Student类的name被transient修饰，则不会被序列化。我们序列化之后，再反序列化，可以看到输出结果如下：

```java
//对象的name属性全部为null
Student{name='null', age=20}
Student{name='null', age=23}
Student{name='null', age=11}
```



## 序列化版本号

序列化版本号有什么用？

- 曾经写了一个类，并且将该类的对象序列化到文件中。过年之后，这个类的源代码被修改了，此时需要重新编译，编译之后就生成了全新的字节码文件，并且字节码文件再次运行的时候，java虚拟机生成的序列化版本号也会发生相应的改变。

此时，如果直接反序列化当年的文件，会出现如下的异常：

```java
java.io.InvalidClassException: com.bit.auto.Student; 
	local class incompatible: 
	stream classdesc serialVersionUID = 280454362619280233(现在的序列化版本号) 
	local class serialVersionUID = -9192231206928936373(当年的序列化版本号)
```

必须重新进行序列化，才会消除上述异常



这种自动生成序列化版本号有什么缺陷？

- 一旦代码确定之后，不能进行后续的修改。因为只要修改，必然会重新编译，此时会生成全新的序列化版本号，这个时候JVM会认为这是一个全新的类



最终结论：凡是一个类实现了Serializable接口，建议给该类提供一个固定不变的序列化版本号。这样，以后这个类即使代码修改了，但是版本号不变，JVM会认为是同一个类

```java
public class Student implements Serializable {
    //手动指定的序列化版本号
    public static final long serialVersionUID = 1L; 
    //transient表示游离的，不参与序列化
    private transient String name;
    private int age;
    int no;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```



**可以用IntelliJ IDEA生成序列化版本号**：

File -> Settings -> Inspections -> Serializable class without serialVersionUID打上对勾，然后当该类实现了Serializable接口时，将光标放在类名上，然后Alt+Enter，可以生成序列化版本号



## IO和Properties联合使用

Properties是一个Map集合，key和value都是String类型，想将文件copy.txt中的数据加载到Properties中.

copy.txt文件中的内容如下所示：

```java
username=admin
password=123
```

如何加载？

```java
public class rt {
    public static void main(String[] args) throws Exception{
        //新建一个输入流对象
        FileInputStream in = new FileInputStream("G:/copy.txt");
        //新建一个Properties集合
        Properties pro = new Properties();
        //调用Properties对象的load方法将文件中的数据加载到Map集合中
        //文件中的数据顺着管道加载到Map集合中
        //等号左边做key，等号右边做value
        pro.load(in);

        //等号key获取value
        String v = pro.getProperty("username");
        System.out.println(v); //admin
        in.close();
    }
}
```

这是非常好的一个设计理念！

- 以后经常改变的数据，可以单独写到一个文件中，使用程序动态读取。将来只需要修改这个文件的内容，java代码不需要改动，不需要重新编译，服务器也不需要重启，就可以拿到动态的信息
- 类似于以上机制的这种文件，被称为**配置文件**。并且当配置文件中的内容格式是：key=value的时候（等号两边最好不要用空格），我们把这种配置文件叫做**属性配置文件**。
- java中规定：属性配置文件建议以.properties结尾，但这不是必须的。其中Properties对象是专门存放属性配置文件内容的一个类。**属性配置文件中万一key重复怎么办：value覆盖**
- **属性配置文件中#开头是注释**