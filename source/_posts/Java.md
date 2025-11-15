---
title: Java
date: 2020-11-01 00:00:00
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Java.png
tags:  
    - 技术
    - Java
---

# Java

> 学习程度
>
> Java 基础语法、流程控制、数组、方法、重载、封装、继承、多态、常用类String、集合、异常处理、多线程、IO流、网络编程



### 数组

```java
int[] ints = {1,2,3,4,5,6,7,8,9};
```



### 循环

foreach

```java
for ( int a : iList) {
}
```



### 集合

#### 常用的集合类有哪些


主要的两个大类有Map接口和Collection接口

| 实现类     |                           底层元素                           |
| ---------- | :----------------------------------------------------------: |
| Arraylist  |                          底层是数组                          |
| LinkedList |                        底层是双向链表                        |
| Vector     |          底层是数组，线程安全的，效率较低，使用较少          |
| HashSet    | 底层是HashMap，放到 HashSet集合中的元素等同于放到HashMap集合key部分了 |
| TreeSet    | 底层是TreeMap，放到 TreeSet集合中的元素等同于放到TreeMap集合key部分了 |
| HashMap    |                         底层是哈希表                         |
| Hashtable  |     底层也是哈希表，只不过线程安全的，效率较低，使用较少     |
| Properties |      是线程安全的，并且 key 和value只能存储字符串String      |
| TreeMap    |   底层是二叉树。TreeMap集合的 key可以自动按照大小顺序排序    |




#### ArrayList

```java
List<Integer> List = new ArrayList<>();
```


常用方法

| 方法名称                  |                             说明                             |
| ------------------------- | :----------------------------------------------------------: |
| add(参数)；               |                           添加元素                           |
| remove(int index) ；      |        移除此集合中指定位置上的元素；返回被删除的元素        |
| get(int index)；          |         返回此集合中指定位置上的元素；返回获取的元素         |
| set(int 索引，修改元素)； |                        将指定元素修改                        |
| size() ；                 | 返回此集合中的元素数。遍历集合时，可以控制索引范围，防止越界 |
| indexof()；               |            元素检索在集合中第几位，如果没有返回-1            |
| contains()；              |  在指定的集合中存在指定的某个元素 toArray():将集合转换数组   |
| clear()；                 |                             清空                             |




### Collections类

集合工具类，包含一些操作集合的静态方法(工具方法)

```java
//集合的元素默认排序（字符串排序按照Ascll码值进行排序）
Collections.sort(intList);

//集合的元素降序排序
Collections.sort(intList, new Comparator<Integer>() {
    public int compare(Integer o1, Integer o2) {
        return o2-o1;
    }
});

// 打乱集合顺序
Collections.shuffle(intList);
```



### IO流

#### 什么是IO？

- I 表示 intput，是数据从硬盘文件读入到内存的过程，称之输入，负责读。
- O 表示 output，是内存程序的数据从内存到写出到硬盘文件的过程，称之输出，负责写。



按流的方向分：

- 输入流
- 输出流

按流中的数据最小单位分：

- 字节流：操作所有类型的文件 
- 字符流：只能操作纯文本文件



#### 字节流读取

- 字节输入流（lnputStream）
  - 以内存为基准，来自磁盘文件/网络中的数据以字符的形式读入到内存中去的流称为字符输入流
- 字节输出流（outputStream）
  - 以内存为基准，把内存中的数据以字符写出到磁盘文件或者网络介质中去的流称为字符输出流

- 字符输入流（Reader）
  - 以内存为基准，来自磁盘文件/网络中的数据以字节的形式读入到内存中去的流称为字节输入流
- 字符输出流（Writer）
  - 以内存为基准，把内存中的数据以字节写出到磁盘文件或者网络中去的流称为字节输出流



 

| 方法名称  |                      说明                      |
| --------- | :--------------------------------------------: |
| read() ； | 每次读取一个字节返回，如果字节没有可读的返回-1 |



一次读取1个字节

```java
 // 1、创建一个文件字节输入流管道与源文件接通
InputStream is = new FileInputStream("src\\sss.txt");
  
 // 2、 每次读取一个字节返回 
int a = is.read();
System.out.println((char)a);

int b = is.read();
System.out.println((char)b); 
```



循环改进（读取不到字符返回-1）

```java
// 1、创建一个文件字节输入流管道与源文件接通
InputStream is = new FileInputStream("src\\sss.txt");

// 定义一个变量记录每次读取的字节
int b;
while ( (b = is.read()) != -1){
    System.out.println((char)b);
}
```



一次读取3个字节

```java
// 1、创建一个文件字节输入流管道与源文件接通
InputStream is = new FileInputStream("src\\sss.txt");

// 2、 定义一个字节数组，用于读取字节数组
//一次读取3个字节
byte[] buffer = new byte[3];
int len = is.read(buffer);
System.out.println("读取了"+len+"个字节");
String rs = new String(buffer);
System.out.println(rs);
```



循环改进

```java
// 1、创建一个文件字节输入流管道与源文件接通
InputStream is = new FileInputStream("src\\sss.txt");

// 2、 定义一个字节数组，用于读取字节数组
//一次读取3个字节
byte[] buffer = new byte[3];
//记录每次读取的字节数
int len;
while ((len = is.read(buffer)) != -1){
    //读取多少倒多少
    System.out.println(new String(buffer,0,len));
}
```



问题：读取中文字符输出无法避免乱码问题

解决：定义一个与文件一样大的字节数组，一次读取完文件的全部字节（可以避免乱码，但如果文件过大，可能引起内存溢出）

方法一

```java
// 1、创建一个文件字节输入流管道与源文件接通
File file = new File("src\\sss.txt");
InputStream is = new FileInputStream(file);

// 2、 定义一个字节数组与文件的大小一样大
byte[] buffer = new byte[(int) file.length()];
int len = is.read(buffer);
System.out.println("读取了多少个字节:"+len);
System.out.println("文件大小:"+ file.length());

// 3、 输出内容
System.out.println(new String(buffer));
```



方法二

```java
// 1、创建一个文件字节输入流管道与源文件接通
File file = new File("src\\sss.txt");
InputStream is = new FileInputStream(file);

// 2、读取全部字节数组
byte[] buffer = is.readAllBytes();
System.out.println(new String(buffer));
```



#### 字节流写入



```Java
// 1、创建一个文件字节输入流管道与源文件接通
File file = new File("src\\sss.txt");
// 创建后会先清空之前的数据，写新数据进去
// OutputStream os = new FileOutputStream(file);
// 追加数据 append 参数为 true 为追加数据
OutputStream os = new FileOutputStream(file,true);

// 2、写数据
// 写一个字节
os.write('a');
os.write(98);
//换行
os.write("\r\n".getBytes());
// 写一个字节数组
byte[] buffer = {'a',97,98,99};
os.write(buffer);
byte[] buffer2 = "我是云南的".getBytes();
os.write(buffer2);
// 写一个字节数组的一部分出去
byte[] buffer3 = {'a',97,98,99};
os.write(buffer3,0,3);

// 刷新数据 可以继续使用流
// os.flush();

// 释放资源 包含刷新 关闭后流不可再使用
os.close();
```



方法

| 方法名称                                 |           说明           |
| ---------------------------------------- | :----------------------: |
| os.write(int a);                         |       写入一个字节       |
| os.write(byte[] buffer);                 |     写入一个字节数组     |
| os.write(byte[] buffer,int pos,int len); | 写入一个字节数组的一部分 |



追加数据

```java
// 创建管道 append 参数为 true 为追加数据
OutputStream os = new FileOutputStream(file,true);
```



实现换行

```java
//换行
os.write("\r\n".getBytes());
```



写入数据生效

```java
// 刷新数据 可以继续使用流
// os.flush();

// 释放资源 包含刷新 关闭后流不可再使用
os.close();
```



#### 文件拷贝

需求：把某个视频复制到其它目录下

思路：

根据数据源创建字节输入流对象

根据目的地创建字节输出流对象

读写数据，复制视频

释放资源



```java
// 使用字节流完成文件的复制（支持一切文件类型的复制）
try {
    // 1、创建一个字节输入流管道与原文件接通
    InputStream inputStream = new FileInputStream("src\\sss.txt");

    // 2、创建一个字节输出流管道与目标文件接通
    OutputStream outputStream = new FileOutputStream("D:\\桌面\\new.txt");

    // 3、定义一个字节数组转移数据
    byte[] buffer = new byte[1024];
    // 记录每次读取的字节数
    int len;

    while ((len = inputStream.read(buffer))!= -1){
        outputStream.write(buffer,0,len);
    }

    System.out.println("复制完成");

    // 4、关闭流
    outputStream.close();
    inputStream.close();

} catch (Exception e) {
    e.printStackTrace();
}
```



#### 资源释放的方式

##### try-catch-finally 

```java
InputStream inputStream =null;
OutputStream outputStream =null;

// 使用字节流完成文件的复制（支持一切文件类型的复制）
try {
    // 1、创建一个字节输入流管道与原文件接通
    inputStream = new FileInputStream("src\\sss.txt");

    // 2、创建一个字节输出流管道与目标文件接通
    outputStream = new FileOutputStream("D:\\桌面\\new.txt");

    // 3、定义一个字节数组转移数据
    byte[] buffer = new byte[1024];
    // 记录每次读取的字节数
    int len;

    while ((len = inputStream.read(buffer))!= -1){
        outputStream.write(buffer,0,len);
    }

    System.out.println("复制完成");

} catch (Exception e) {
    e.printStackTrace();
} finally {
    // 无论代码是否正常结束还是出现异常都要执行这里
    // 哪怕上面有return语句执行，也必须执行完这里才可以
    // 开发中不建议在此处加return，如果加了返回永远是此处的数据
    // 4、关闭流
    try {
        // 如果在接通之前发生异常 outputStream = null 就不需要关闭
        if (outputStream!=null) outputStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }

    try {
        // 如果在接通之前发生异常 inputStream = null 就不需要关闭
        if (inputStream!=null) inputStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}try {

} catch (Exception e) {
    e.printStackTrace();
} finally {
    // 无论代码是否正常结束还是出现异常都要执行这里
    // 哪怕上面有return语句执行，也必须执行完这里才可以
    // 开发中不建议在此处加return，如果加了返回永远是此处的数据
    // 4、关闭流
    try {
        // 如果在接通之前发生异常 outputStream = null 就不需要关闭
        if (outputStream!=null) outputStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }

    try {
        // 如果在接通之前发生异常 inputStream = null 就不需要关闭
        if (inputStream!=null) inputStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

##### try-catch-resouce

`资源都是实现了Closeable/AutoCloseable方法的接口的类对象`

```java
// 使用字节流完成文件的复制（支持一切文件类型的复制）
try (
        // 这里面只能放置资源对象 用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
        // 1、创建一个字节输入流管道与原文件接通
        InputStream inputStream =new FileInputStream("src\\sss.txt");
        // 2、创建一个字节输出流管道与目标文件接通
        OutputStream outputStream =new FileOutputStream("D:\\桌面\\new.txt");
        ){
    // 3、定义一个字节数组转移数据
    byte[] buffer = new byte[1024];
    // 记录每次读取的字节数
    int len;

    while ((len = inputStream.read(buffer))!= -1){
        outputStream.write(buffer,0,len);
    }

    System.out.println("复制完成");

} catch (Exception e) {
    e.printStackTrace();
}
```



#### 字符流读取

使用字节流读取中文可能存在乱码或者内存溢出，字符流按照单个字符读取

| 方法名称 | 说明                             |
| -------- | -------------------------------- |
| read()； | 读取一个字符，没有可读字符返回-1 |



一次读取一个字符

```java
// 1、创建一个字符输入流管道与原文件接通
Reader reader = new FileReader("src\\sss.txt");
// 读取一个字符返回，没有可读字符返回-1
//int code = reader.read();
//System.out.println((char)code);
// 使用循环读取字符
int code1;
while ((code1 = reader.read()) != -1){
    System.out.println((char)code1);
}
```



一次读取一个字符数组

```java
// 1、创建一个字符输入流管道与原文件接通
Reader reader = new FileReader("src\\sss.txt");
// 2、使用循环每次读取一个字符数组
char[] buffer = new char[1024];
int len;
while ((len = reader.read(buffer)) != -1){
    String rs = new String(buffer,0,len);
    System.out.println(rs);
}
```



#### 字符流写入

```java
// 1、创建一个字符输入流管道与原文件接通
// 覆盖管道 每次启动都会清空文件之前的数据
//Writer writer = new FileWriter("src\\sss.txt");
// 追加数据
Writer writer = new FileWriter("src\\sss.txt",true);

writer.write("我");
writer.write(98);
writer.write('a');
writer.write("我是云南的abc");
// 换行
writer.write("\r\n");

char[] chars = "我是云南的abc".toCharArray();
writer.write(chars);
writer.write(chars,3,5);

writer.write("我是云南的abc",0,5);

// 刷新 流可以继续使用
// writer.flush();
// 关闭 包含刷新，流不能使用
writer.close();
```



#### 缓冲流

自带缓冲区，可以提高原始字节流、字符流读写数据的性能

##### 字节缓冲流

- 字节缓冲输入流
  - 字节缓冲输入流自带8KB缓冲池，直接从缓冲池读取数据，所以性能较好
  - 字节缓冲输出流自带8KB缓冲池，数据就直接写入到缓冲池中去，写入性能极高

- 字节缓冲输出流

##### 字符缓冲流

- 字符缓冲输入流
- 字符缓冲输出流



##### 字节缓冲流读取写入

```java
// 使用字节流 缓冲流完成文件的复制（支持一切文件类型的复制）
try (
        // 这里面只能放置资源对象 用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
        // 1、创建一个字节输入流管道与原文件接通
        InputStream inputStream =new FileInputStream("src\\sss.txt");
        //把原始字节输入流管道包装成高级的缓冲字节输入流
        InputStream bis =new BufferedInputStream(inputStream);

        // 2、创建一个字节输出流管道与目标文件接通
        OutputStream outputStream =new FileOutputStream("D:\\桌面\\new.txt");
        //把原始字节输出流管道包装成高级的缓冲字节输入流
        OutputStream bos =new BufferedOutputStream(outputStream);
){
    // 3、定义一个字节数组转移数据
    byte[] buffer = new byte[1024];
    // 记录每次读取的字节数
    int len;

    while ((len = bis.read(buffer))!= -1){
        bos.write(buffer,0,len);
    }

    System.out.println("复制完成");

} catch (Exception e) {
    e.printStackTrace();
}
```



##### 字符缓冲流读取

```java
// 使用字节流 缓冲流完成文件的复制（支持一切文件类型的复制）
try (
        // 这里面只能放置资源对象 用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
        // 1、创建一个字节输入流管道与原文件接通
        Reader reader = new FileReader("src\\sss.txt");
        // 把低级的字符输入流包装成高级的缓冲字符输入流
        BufferedReader br = new BufferedReader(reader);
){
    // 2、使用循环每次读取一个字符数组
    char[] buffer = new char[1024];
    int len;
    while ((len = br.read(buffer)) != -1){
        String rs = new String(buffer,0,len);
        System.out.println(rs);
    }

    //读一行
    //System.out.println(br.readLine());


} catch (Exception e) {
    e.printStackTrace();
}
```



##### 字符缓冲流写入

```java
// 1、创建一个字符输入流管道与原文件接通
// 覆盖管道 每次启动都会清空文件之前的数据
//Writer writer = new FileWriter("src\\sss.txt");
// 追加数据
Writer writer = new FileWriter("src\\sss.txt",true);

BufferedWriter bw = new BufferedWriter(writer);

bw.write("我");
bw.write(98);
bw.write('a');
bw.write("我是云南的abc");
// 换行
bw.write("\r\n");

char[] chars = "我是云南的abc".toCharArray();
bw.write(chars);
bw.write(chars,3,5);

bw.write("我是云南的abc",0,5);

// 刷新 流可以继续使用
// writer.flush();
// 关闭 包含刷新，流不能使用
bw.close();
```



#### 转换流

解决不同编码乱码

##### 字符转换输入流

可以指定编码把原始字节流转换成字符流，如此字符中的字符不乱码

InputStreamReader

```JAVA
InputStream is = new FileInputStream("src\\sss.txt");
// 把原始字节流转换成字符输入流
// 以指定编码GBK编码转换成输入流
Reader isr = new InputStreamReader(is,"GBK");

BufferedReader br = new BufferedReader(isr);
String line;
while ((line = br.readLine())!= null){
    System.out.println(line);
}
```



##### 字符转换输出流

指定字符写出去

```java
// 1、定义一个字节输出流
OutputStream os = new FileOutputStream("src\\sss.txt");
// 把原始字节流转换成字符输出流
// 2、以默认UTF-8的方式写字符出去
// Writer osw = new OutputStreamWriter(os);

// 3、以指定的GBK写字符出去
Writer osw = new OutputStreamWriter(os,"GBK");

BufferedWriter bw = new BufferedWriter(osw);

bw.write("云南的1");
bw.write("云南的2");
bw.write("云南的3");

bw.close();
```



#### 对象序列化

对象字节输出流：ObjectOutputStream

作用：以内存为基准，把内存中的对象存储到磁盘文件中去，称为对象的序列化

`Student 对象如果要序列化必须实现 Serializable 序列化接口`

```java
// 1、创建学生对象
Student s = new Student("云","yun","男",21);

// 2、对象序列化 使用对象字节输出流包装字节输出流管道
ObjectOutputStream oos = new ObjectOutputStream(new  FileOutputStream("src\\sss.txt"));

// 3、直接调用序列化方法
oos.writeObject(s);

// 4、释放资源
oos.close();

System.out.println("序列化完成");
```



#### 对象反序列化

对象字节输入流：ObjectInputStream

作用：以内存为基准，把存储到磁盘文件中去的对象数据恢复成内存中的对象，称为反序列化



使用对象字节输入流把文件中的对象数据恢复成内存中的java对象

```java
// 1、创建对象字节输入流管道包装低级的字节输入流管道
ObjectInputStream is = new ObjectInputStream(new FileInputStream("src\\sss.txt"));

// 2、调用对象字节输入流的反序列化方法
Student s = (Student) is.readObject();

System.out.println(s);
```



#### 打印流

打印任何数据出去

```java
//创建一个打印流对象
//PrintStream ps = new PrintStream(new FileOutputStream("src\\sss.txt"));

// 追加数据，在低级管道后加true
PrintStream ps = new PrintStream(new FileOutputStream("src\\sss.txt",true));

//PrintStream ps = new PrintStream("src\\sss.txt");

// 打印功能上与PrintStream的使用没有区别
//PrintStream ps = new PrintWriter("src\\sss.txt");

ps.println(97);
ps.println('a');
ps.println(true);
ps.println(1.2);
ps.println("我是云南的");

// 释放资源
ps.close();
```



#### 输出语句的重定向

属于打印流的一种应用，可以把输出语句的打印位置改到文件

```java
//打印到控制台
System.out.println("打印");

PrintStream ps = new PrintStream("src\\sss.txt");
//把系统打印流改成我们自己的打印流
System.setOut(ps);

//打印到文件
System.out.println("打印");
```



#### Properties

需求：使用Properties把键值对信息存入到属性文件中去

```java
 Properties properties = new Properties();
 properties.put("admin","123456");
 properties.put("root","root");

 System.out.println(properties);


 // 参数一 保存管道 字符输出流管道
 // 参数二 保存心得
 properties.store(new FileWriter("src/users.properties"),"this is users !!");
```



需求：Properties读取属性文件中的键值对信息

```java
// 需求：使用Properties把键值对信息存入到属性文件中去
 Properties properties = new Properties();

 System.out.println(properties);


 // 加载属性文件中的键值对数据到属性对象Properties中去
 properties.load(new FileReader("src/users.properties"));

 System.out.println(properties);

 String rs =properties.getProperty("admin");
 System.out.println(rs);
```



### IO框架

commons-io

提供一组有关IO操作的类库，可以提高IO功能开发的效率



需求：使用commons-io 简化 io 流读写

1. 在项目中创建一个文件夹：lib
2. 将commons-io-2.6jar文件复制到lib文件夹
3. 在jar文件上点击右键-选择Add as Library - 点击OK
4. 在类中导宝使用



```java
//拷贝
IOUtils.copy(new FileInputStream("路径"),new FileOutputStream("目标路径"));
```



### 多线程

#### 线程是什么？

线程是一个程序内部的一条执行路径（main方法的执行就是一条单独的执行路径）

程序中如果只有一条执行路径，那么这个程序就是单线程的程序



多线程是指从软硬件上实现多条执行流程



#### 多线程的创建方式

三种方式

##### 多线程实现方案一：继承Thread类

实现步骤：

1. 定义一个子类 MyThread 继承线程类 java.lang.Thread，重写run()方法
2. 创建 MyThread 类的对象
3. 调用线程对象的 start() 方法启动线程（启动后还是执行run方法的)



```java
public class Main {

    public static void main(String[] args) throws Exception {

        // 3、new一个新线程对象
        Thread thread = new MyThread();

        // 4、调用start方式启动线程（执行还是run方法）
        thread.start();

        for (int i = 0; i < 5; i++) {
            System.out.println("主线程执行输出："+i);
        }
    }

}

/**
 * 1、定义一个线程类继承Thread类
 */
class MyThread extends Thread{

    /**
     * 2、重写run方法，里面是定义线程以后要干啥
     */

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出："+i);
        }
    }
}
```



方式一优缺点:

优点：编码简单

缺点：线程类已经继承Thread，无法继承其他类，不利于扩展



###### 问题

- 为什么不直接调用了run方法，而是调用start启动线程

直接调用run方法会当成普通方法执行，此时相当于还是单线程执行

只有调用start方法才是启动一个新的线程执行



- 把主线程任务放在子线程之前了

这样主线程一直是先跑完的，相当于是一个单线程的效果了。



##### 多线程实现方案二：实现 Runnable 接口

实现步骤：

1. 定义一个线程任务类 MyRunnable 实现 Runnable 接口，重写 run() 方法
2. 创建 MyRunnable 任务对象
3. 把 MyRunnable 任务对象交给Thread处理。
4. 调用线程对象的 start() 方法启动线程



```java
public class Main {

    public static void main(String[] args) throws Exception {

        // 3、创建一个任务对象
        Runnable target = new MyRunnable();

        // 4、把任务对象交给 Thread 处理
        Thread thread = new Thread(target);

        // 5、启动线程
        thread.start();

        for (int i = 0; i < 5; i++) {
            System.out.println("主线程执行输出："+i);
        }
    }

}

/**
 * 1、定义一个任务类，实现 Runnable 接口
 */
class MyRunnable implements Runnable{

    /**
     * 2、重写run方法，定义线程任务
     */

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出："+i);
        }
    }
}
```



方式二优缺点:

优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强

缺点：编程多一层对象包装，如果线程有执行结果是不可以直接返回的



###### 匿名内部类方式实现：写法一

```java
// 创建一个任务对象
Runnable target = new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出："+i);
        }
    }
};

// 把任务对象交给 Thread 处理
Thread thread = new Thread(target);

// 启动线程
thread.start();
```



###### 匿名内部类方式实现：写法二

```Java
Thread thread1 = new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出："+i);
        }
    }
});

// 3、启动线程
thread1.start();
```



###### 匿名内部类方式实现：写法三

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出："+i);
        }
    }
}).start();
```



###### 匿名内部类方式实现：写法四

```java
new Thread(() -> {
    for (int i = 0; i < 5; i++) {
        System.out.println("子线程执行输出："+i);
    }
}).start();
```



##### 多线程实现方案三：利用 Casllable 和 FutureTask 来实现

前两种线程创建方式存在问题

- 重写的run方法均不能直接返回结果
- 不适合需要返回线程执行结果的业务场景

解决： JDK 5.0 提供了 Casllable 和 FutureTask 来实现

优点：可以得到线程执行的结果

实现步骤：

1. 得到任务对象

   - 定义类实现 Callable 接口，重写 call 方法，封装要做的事情。

   - 用 FutureTask 把 callable 对象封装成线程任务对象。

2. 把线程任务对象交给 Thread 处理。

3. 调用 Thread 的 start 方法启动线程，执行任务

4. 线程执行完毕后、通过FutureTask的get方法去获取任务执行的结果。



```java
public class Main {

    public static void main(String[] args) throws Exception {

        // 3、创建 callable 任务对象
        Callable<String> callable = new MyCallable(100);

        // 4、结合 FutureTask 完成：把Callable 任务对象 交给 FutureTask 对象
        // FutureTask 对象的作用1：是 Runnable 的对象（实现了Runnable接口），可以交给Thread了
        // FutureTask 对象的作用2：可以在线程执行完毕之后通过调用其get方法得到线程执行完毕的结果

        FutureTask<String> f = new FutureTask<>(callable);

        // 5、交给线程处理
        Thread t = new Thread(f);

        // 6、启动线程
        t.start();



        Callable<String> callable2 = new MyCallable(200);
        FutureTask<String> f2 = new FutureTask<>(callable2);
        Thread t2 = new Thread(f2);
        t2.start();

        try {
            // 如果f1任务没有执行完毕 这里的代码会等待 知道线程1执行完毕才提取结果
            String s1 = f.get();
            System.out.println("线程一结果：" + s1);
        }catch (Exception e){
            e.printStackTrace();
        }

        try {
            // 如果f2任务没有执行完毕 这里的代码会等待 知道线程2执行完毕才提取结果
            String s2 = f2.get();
            System.out.println("线程二结果：" + s2);
        }catch (Exception e){
            e.printStackTrace();
        }

    }

}

/**
 * 1、定义一个任务类，实现 Callable 接口 应该申明线程任务执行完毕后的结果的数据类型
 */
class MyCallable implements Callable<String> {

    private int n;

    public MyCallable(int n) {
        this.n = n;
    }

    /**
     * 2、重写call方法，定义线程任务
     */
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }
        return "子线程执行的结果："+sum;
    }

}
```



方式三优缺点:

优点：

线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强。

可以在线程执行完毕后去获取线程执行的结果。

缺点：编码复杂一点。



#### 线程的常用方法

Thread 常用方法︰

| 方法名称        |               说明                |
| --------------- | :-------------------------------: |
| getName()       |           获取线程名称            |
| setName()       |             设置名称              |
| currentThread() |         获取当前线程对象          |
| sleep(3000)     | 让当前线程进入休眠状态,单位为毫秒 |
| run()           |           线程任务方法            |
| start()         |           线程启动方法            |

Thread 常用方法构造器

| 方法名称                            | 说明                                       |
| ----------------------------------- | ------------------------------------------ |
| Thread(String name)                 | 可以为当前线程指定名称                     |
| Thread(Runnable target)             | 把Runnable对象交给线程对象                 |
| Thread(Runnable target,String name) | 把Runnable对象交给线程对象，并指定线程名称 |



```java
public class Main {

    // main 方法是由于主线程负责调度的
    public static void main(String[] args) throws Exception {
        Thread t = new MyThread("1号");
        //t.setName("1号");
        System.out.println(t.getName());
        t.start();

        Thread t2 = new MyThread("2号");
        //t2.setName("2号");
        System.out.println(t2.getName());
        t2.start();

        // 哪个线程执行它 它得到哪个线程对象（当前线程对象）
        // 主线程的名称就叫main
        Thread m = Thread.currentThread();
        System.out.println(m.getName());

        for (int i = 0; i < 5; i++) {
            System.out.println("main线程执行输出："+i);
            if (i==3){
                //让当前线程进入休眠状态
                Thread.sleep(3000);
            }
        }

    }

}

/**
 * 1、定义一个任务类，实现 Callable 接口 应该申明线程任务执行完毕后的结果的数据类型
 */
class MyThread extends Thread {

    public MyThread() {
    }

    public MyThread(String name) {
        // 为当前线程对象设置名称 送给父类的有参数构造器初始化名称
        super(name);
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()+"输出："+i);
        }
    }
}
```



#### 线程安全

**线程安全问题**
**多个线程同时操作同一个共享资源**的时候可能会出现业务安全问题，称为线程安全问题。



**线程安全问题出现的原因?**

1. 存在多线程并发
2. 同时访问共享资源
3. 存在修改共享资源



需求

- 两个人共同账户，余额十万，同时取十万

分析:

- 需要提供一个账户类，创建一个账户对象代表2个人的共享账户。
- 需要定义一个线程类，线程类可以处理账户对象。
- 创建2个线程对象，传入同一个账户对象。
- 启动2个线程，去同一个账户对象中取钱10万。



```java
public class Main {

    /**
     * 需求：模拟取钱案例
     */
    public static void main(String[] args) throws Exception {

        // 1、定义线程类，创建一个共享的账户对象
        Account account = new Account("ICBC-111",100000);

        // 2、创建2个线程对象 代表两个人同时进入
        new DrawThread(account,"小明").start();
        new DrawThread(account,"小红").start();

    }

}
```



取钱线程类

```java
package com.yichen;

/**
 * 取钱线程类
 */
public class DrawThread extends Thread{

    // 接受处理的账户对象
    private Account account;
    public  DrawThread(Account account,String name){
        super(name);
        this.account = account;
    }

    @Override
    public void run() {
        // 取钱
        account.drawMoney(100000);
    }
}
```



对象

```java
package com.yichen;

public class Account {

    private String cardId;
    private double money;

    public Account() {
    }

    public Account(String cardId, double money) {
        this.cardId = cardId;
        this.money = money;
    }

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    /**
     *
     * @param money
     */
    public void drawMoney(double money) {
        // 先获取是谁来取钱 线程的名字就是人名
        String name = Thread.currentThread().getName();
        // 1、判断账户是否够钱
        if (this.money>=money){
            // 2、取钱
            System.out.println(name + "取钱，吐出"+ money);
            // 3、更新余额
            this.money -= money;
            System.out.println(name + "取后剩余："+ this.money);
        }else{
            // 4、余额不足
            System.out.println(name + "余额不足");
        }
        
    }
}
```

结果

`小红取钱，吐出100000.0
小明取钱，吐出100000.0
小红取后剩余：0.0
小明取后剩余：-100000.0`



#### 线程同步

**为了解决线程安全问题**

- 取钱案例出现问题的原因?

多个线程同时执行，发现账户都是够钱的。

- 如何才能保证线程安全呢?

让多个线程实现先后依次访问共享资源，这样就解决了安全问题



**线程同步的核心思想**

加锁，把共享资源进行上锁，每次只能一个线程进入访问完毕以后解锁，然后其他线程才能进来。



##### 方式一：同步代码块

作用：把出现线程安全问题的核心代码给上锁。

原理：每次只能一个线程进入，执行完毕后自动解锁，其他线程才可以进来执行。

```java
synchronized(同步锁对象){
	操作共享资源的代码(核心代码)
}
```

**锁对象要求**

- 理论上：锁对象只要对于当前同时执行的线程来说是同一个对象即可

```java
public void drawMoney(double money) {
    // 先获取是谁来取钱 线程的名字就是人名
    String name = Thread.currentThread().getName();

    synchronized ("yichen") {
        // 1、判断账户是否够钱
        if (this.money>=money){
            // 2、取钱
            System.out.println(name + "取钱，吐出"+ money);
            // 3、更新余额
            this.money -= money;
            System.out.println(name + "取后剩余："+ this.money);
        }else{
            // 4、余额不足
            System.out.println(name + "余额不足");
        }
    }

}
```

**锁对象用任意唯一的对象好不好呢?**

不好，会影响其他无关线程的执行

**锁对象规范要求**

- 规范上：建议使用共享资源作为锁对象

  - ```java
    public void drawMoney(double money) {
        // 先获取是谁来取钱 线程的名字就是人名
        String name = Thread.currentThread().getName();
        // 同步代码块
        // this == Account 共享账户
        synchronized (this) {
            // 1、判断账户是否够钱
            if (this.money>=money){
                // 2、取钱
                System.out.println(name + "取钱，吐出"+ money);
                // 3、更新余额
                this.money -= money;
                System.out.println(name + "取后剩余："+ this.money);
            }else{
                // 4、余额不足
                System.out.println(name + "余额不足");
            }
        }
    }
    ```

- 对于实例方法建议使用this作为锁对象

- 对于静态方法建议使用字节码(类名.class)对象作为锁对象

  - ```java
    // 100个线程
    public static void  run(){
        synchronized (Account.class){
            
        }
    }
    ```



**同步代码块是如何实现线程安全的?**

- 对出现问题的核心代码使用synchronized进行加锁
- 每次只能一个线程占锁进入访问



**同步代码块的同步锁对象有什么要求?**

- 对于实例方法建议使用 this 作为锁对象
- 对于静态方法建议使用字节码（类名.class)对象作为锁对象



##### 方式二：同步方法

作用：把出现线程安全问题的核心方法给上锁。

原理：每次只能一个线程进入，执行完毕以后自动解锁，其他线程才可以进来执行。

```java
修饰符 synchronized 返回值 类型方法 名称(形参列表){
	操作共享资源的代码
}
```



```java
public synchronized void drawMoney(double money) {
    // 先获取是谁来取钱 线程的名字就是人名
    String name = Thread.currentThread().getName();
    // 同步代码块
    // this == Account 共享账户
    // 1、判断账户是否够钱
    if (this.money>=money){
        // 2、取钱
        System.out.println(name + "取钱，吐出"+ money);
        // 3、更新余额
        this.money -= money;
        System.out.println(name + "取后剩余："+ this.money);
    }else{
        // 4、余额不足
        System.out.println(name + "余额不足");
    }
}
```



**同步方法底层原理**

- 同步方法其实底层也是有隐式锁对象的，只是锁的范围是整个方法代码。

- 如果方法是实例方法：同步方法默认用this作为的锁对象。但是代码要高度面向对象!

- 如果方法是静态方法：同步方法默认用 类名.class 作为的锁对象



**是同步代码块好还是同步方法好一点?**

- 同步代码块锁的范围更小，同步方法锁的范围更大。



**同步方法是如何保证线程安全的?**

- 对出现问题的核心方法使用synchronized修饰
- 每次只能一个线程占锁进入访问



##### 方式三：Lock锁

- 为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock，更加灵活、方便。
- Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作。
- Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来构建Lock锁对象。

| 方法名称        |          说明          |
| --------------- | :--------------------: |
| ReentrantLock() | 获得Lock锁的实现类对象 |



**Lock 的 API**

| 方法名称 |  说明  |
| -------- | :----: |
| lock()   | 获得锁 |
| unlock() | 释放锁 |



```java
package com.yichen;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Account {

    private String cardId;
    private double money;

    // final修饰后 锁对象是唯一不可替换的
    private final Lock lock = new ReentrantLock();


    public Account() {
    }

    public Account(String cardId, double money) {
        this.cardId = cardId;
        this.money = money;
    }

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    /**
     *
     * @param money
     */
    public void drawMoney(double money) {
        // 先获取是谁来取钱 线程的名字就是人名
        String name = Thread.currentThread().getName();
        // 同步代码块
        // this == Account 共享账户
        // 1、判断账户是否够钱
        // 上锁
        lock.lock();
        try {
            if (this.money>=money){
                // 2、取钱
                System.out.println(name + "取钱，吐出"+ money);
                // 3、更新余额
                this.money -= money;
                System.out.println(name + "取后剩余："+ this.money);
            }else{
                // 4、余额不足
                System.out.println(name + "余额不足");
            }
        } finally {
            // 解锁
            lock.unlock();
        }
    }

}
```



#### 线程通信



**什么是线程通信、如何实现?**

所谓线程通信就是线程间相互发送数据，线程通信通常通过共享一个数据的方式实现。

线程间会根据共享数据的情况决定自己该怎么做，以及通知其他线程怎么做。



**线程通信常见模型**

生产者与消费者模型:生产者线程负责生产数据，消费者线程负责消费数据。

要求：生产者线程生产完数据后，唤醒消费者，然后等待自己;消费者消费完该数据后，唤

醒生产者，然后等待自己。



```java
public class Main {

    /**
     * 需求：模拟取钱案例
     */
    public static void main(String[] args) throws Exception {

        // 目标：了解线程通信的流程
        // 三个爸爸存钱(生产者)，两个孩子（消费者）取钱 模拟线程通讯思想（一存10万一取10万）

        // 1、创建账户对象，代表5个人共同操作的账户
        Account account = new Account("ICBC-122",0);

        // 2、创建两个取钱线程代表小明和小红
        new DrawThread(account,"小明").start();
        new DrawThread(account,"小红").start();


        // 3、创建三个存钱线程代表亲爹、干爹、岳父
        new DepositThread(account,"亲爹").start();
        new DepositThread(account,"干爹").start();
        new DepositThread(account,"岳父").start();


    }

}
```



存钱线程

```java
package com.yichen;

/**
 * 取钱线程类
 */
public class DepositThread extends Thread{

    // 接受处理的账户对象
    private Account account;
    public DepositThread(Account account, String name){
        super(name);
        this.account = account;
    }

    @Override
    public void run() {

        // 亲爹、干爹、岳父 存钱
        while (true){
            account.deposit(100000);
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }


    }
}
```



取钱线程

```java
package com.yichen;

/**
 * 取钱线程类
 */
public class DrawThread extends Thread{

    // 接受处理的账户对象
    private Account account;
    public  DrawThread(Account account,String name){
        super(name);
        this.account = account;
    }

    @Override
    public void run() {

        // 小明 小红 取钱
        while (true){
            account.drawMoney(100000);
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }


    }
}
```



对象

```java
package com.yichen;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Account {

    private String cardId;
    private double money;

    public Account() {
    }

    public Account(String cardId, double money) {
        this.cardId = cardId;
        this.money = money;
    }

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    /**
     * 亲爹、干爹、岳父 存钱
     * @param money
     */
    public synchronized void deposit(double money) {
        try {
            String name = Thread.currentThread().getName();
            if (this.money == 0){
                // 没钱了：存钱
                this.money += money;
                System.out.println(name + "存了" + money + "余额" + this.money);
                // 有钱了：唤醒别人，等待自己
                this.notifyAll();
                // 锁对象让当前线程进入等待
                this.wait();
            }else{
                // 有钱，不存
                this.notifyAll();
                // 锁对象让当前线程进入等待
                this.wait();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    /**
     * 小红 小明 取钱
     */
    public synchronized void drawMoney(double money) {

        try {
            String name = Thread.currentThread().getName();
            if (this.money >= money){
                // 钱够：可以取
                this.money -= money;
                System.out.println(name + "取了" + money + "余额" + this.money);

                // 没钱了
                // 唤醒所有线程,等待自己
                this.notifyAll();
                // 锁对象让当前线程进入等待
                this.wait();
            }else{
                // 钱不够：不可取
                // 唤醒所有线程,等待自己
                this.notifyAll();
                // 锁对象让当前线程进入等待
                this.wait();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }


    }


}
```



##### 线程通信的三个常见方法

| 方法名称  |                             说明                             |
| --------- | :----------------------------------------------------------: |
| wait()    | 当前线程等待，直到另一个线程调用notify(）或notifyA11()唤醒自己 |
| notify    |           唤醒正在等待对象监视器(锁对象)的单个线程           |
| notifyAll |           唤醒正在等待对象监视器(锁对象)的所有线程           |

**上述方法应该使用当前同步锁对象进行调用。**



#### 线程池



**什么是线程池?**

- 线程池就是一个可以复用线程的技术。



**不使用线程池的问题**

- 如果用户每发起一个请求，后台就创建一个新线程来处理，下次新任务来了又要创建新线程，而创建新线程的开销是很大的，这样会严重影响系统的性能。



##### 线程池实现的API、参数说明

**谁代表线程池?**

JDK 5.0起提供了代表线程池的接口:ExecutorService



**如何得到线程池对象**

- 方式一：使用 ExecutorService 的实现类 ThreadPoolExecutor 自创建一个线程池对象
- 方式二：使用Executors(线程池的工具类）调用方法返回不同特点的线程池对象





ThreadPoolExecutor构造器的参数说明

```java
public ThreadPoolExecutor( 
    int corePoolSize,
	int maximumPoolsize,
    long keepAliveTime,
    TimeUnit unit,
	BlockingQueue<Runnable> workQueue,
    ThreadFactory threadFactory,
	RejectedExecutionHandler handler)
```

参数一：指定线程池的线程数量（核心线程): corePoolSize	不能小于0

参数二：指定线程池可支持的最大线程数: maximumPoolsize	最大数量>=核心线程数量

参数三：指定临时线程的最大存活时间: keepAliveTime 	不能小于0

参数四：指定存活时间的单位(秒、分、时、天): unit	时间单位

参数五：指定任务队列: workQueue	不能为null

参数六：指定用哪个线程工厂创建线程: threadFactory	不能为null

参数七：指定线程忙，任务满的时候，新任务来了怎么办:handler	不能为null



##### 线程池处理Runnable任务

**使用ExecutorService的方法:**void execute(Runnable target)



**ExecutorService**的常用方法

| 方法名称                           |                             说明                             |
| ---------------------------------- | :----------------------------------------------------------: |
| void execute(Runnable command)     |     执行任务/命令，没有返回值，一般用来执行Runnable 任务     |
| Future<T> submit(callable<T> task) | 执行任务，返回未来任务对象获取线程结果，一般拿来执行callable任务 |
| void shutdown()                    |                  等任务执行完毕后关闭线程池                  |
| List<Runnable> shutdownNow()       |    立刻关闭，停止正在执行的任务，并返回队列中未执行的任务    |



ThreadPoolExecutor创建线程池对象示例

```java
ExecutorService pools = new ThreadPoolExecutor(3,5
	,8 ,TimeUnit.SECONDS，new ArrayBlockingQueue<>(6),
		Executors.defaultThreadFactory() , new ThreadPoolExecutor.AbortPolicy());
```



```java
public class Main {

    /**
     * 自定义一个线程池对象
     */
    public static void main(String[] args) throws Exception {

        // 1、创建线程池对象
        /**
         * ExecutorService executorService = new ThreadPoolExecutor(
         *                     int corePoolSize,
         *                     int maximumPoolSize,
         *                     long keepAliveTime,
         *                     TimeUnit unit,
         *                     BlockingQueue<Runnable> workQueue,
         *                     ThreadFactory threadFactory,
         *                     RejectedExecutionHandler handler
         *         );
         */
        ExecutorService executorService = new ThreadPoolExecutor(3,5,
                6,TimeUnit.SECONDS,new ArrayBlockingQueue<>(5),
                Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());

        // 2、给任务线程池处理

        Runnable target = new MyRunnable();
        executorService.execute(target);
        executorService.execute(target);
        executorService.execute(target);

        executorService.execute(target);
        executorService.execute(target);
        executorService.execute(target);
        executorService.execute(target);
        executorService.execute(target);

        // 会创建临时线程
        executorService.execute(target);
        executorService.execute(target);

        // 不会创建临时线程 拒绝策略被触发
        executorService.execute(target);

    }

}
```

MyRunnable

```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()+"输出" + i);
        }

        try {
            System.out.println(Thread.currentThread().getName()+"进入休眠了");
            Thread.sleep(1000000);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```



**新任务拒绝策略**

| 策略                                   |                            详解                            |
| -------------------------------------- | :--------------------------------------------------------: |
| ThreadPoolExecutor.AbortPolicy         | 丢弃任务并抛出RejectedExecutionException异常。是默认的策略 |
| ThreadPoolExecutor. DiscardPolicy:     |          丢弃任务，但是不抛出异常这是不推荐的做法          |
| ThreadPoolExecutor.DiscardOldestPolicy |      抛弃队列中等待最久的任务然后把当前任务加入队列中      |
| ThreadPoolExecutor.CallerRunsPolicy    |   由主线程负责调用任务的run()方法从而绕过线程池直接执行    |


**一般不会使用**

```java
// 立即关闭线程池 即使任务没有完成，会丢失任务
executorService.shutdownNow();

// 会等待全部任务执行完毕之后再关闭
executorService.shutdown();
```



##### 线程池处理Callable任务

使用ExecutorService的方法:
Future<T> submit(Callable<T> command)

```java
public class Main {
    
    public static void main(String[] args) throws Exception {

        // 1、创建线程池对象
        /**
         * ExecutorService executorService = new ThreadPoolExecutor(
         *                     int corePoolSize,
         *                     int maximumPoolSize,
         *                     long keepAliveTime,
         *                     TimeUnit unit,
         *                     BlockingQueue<Runnable> workQueue,
         *                     ThreadFactory threadFactory,
         *                     RejectedExecutionHandler handler
         *         );
         */
        ExecutorService executorService = new ThreadPoolExecutor(3,5,
                6,TimeUnit.SECONDS,new ArrayBlockingQueue<>(5),
                Executors.defaultThreadFactory(),new ThreadPoolExecutor.AbortPolicy());

        // 2、给任务线程池处理
        Future<String> futureTask = executorService.submit(new MyCallable(100));
        Future<String> futureTask2 = executorService.submit(new MyCallable(200));
        Future<String> futureTask3 = executorService.submit(new MyCallable(300));
        Future<String> futureTask4 = executorService.submit(new MyCallable(400));

        //String rs = futureTask.get();
        //System.out.println(rs);

        System.out.println(futureTask.get());
        System.out.println(futureTask2.get());
        System.out.println(futureTask3.get());
        System.out.println(futureTask4.get());
    }

}
```

MyCallable

```java
/**
 * 1、定义一个任务类，实现 Callable 接口 应该申明线程任务执行完毕后的结果的数据类型
 */
class MyCallable implements Callable<String> {

    private int n;

    public MyCallable(int n) {
        this.n = n;
    }

    /**
     * 2、重写call方法，定义线程任务
     */
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }
        return Thread.currentThread().getName() + "执行1-"+n+"的和的结果是：" + sum;
    }

}
```





##### Executors工具类实现线程池

Executors 得到线程池对象的常用方法
Executors：线程池的工具类通过调用方法返回不同类型的线程池对象。



| 方法名称                                                     |                             说明                             |
| ------------------------------------------------------------ | :----------------------------------------------------------: |
| public static ExecutorService newCachedThreadPool()          | 线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了一段时间则会被回收掉。 |
| public static ExecutorService newFixedThreadPool(int nThreads) | 创建固定线程数量的线程池，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程替代它。 |
| public static ExecutorService newSingleThreadExecutor ()     | 创建只有一个线程的线程池对象，如果该线程出现异常而结束，那么线程池会补充一个新线程。 |
| public static scheduledExecutorService newScheduledThreadPool(int corePoolSize) | 创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务。 |

注意：Executors的底层其实也是基于线程池的实现类ThreadPoolExecutor创建线程池对象的。

```java
public class Main {

    public static void main(String[] args) throws Exception {
        // 使用Executors的工具方法直接得到一个线程池对象
        // 1、创建固定线程数量的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(3);

        executorService.execute(new MyRunnable());
        executorService.execute(new MyRunnable());
        executorService.execute(new MyRunnable());
        executorService.execute(new MyRunnable());
        executorService.execute(new MyRunnable());

        // 没有多余的线程 执行
        executorService.execute(new MyRunnable());
    }

}
```

MyRunnable

```java
public class MyRunnable implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName()+"输出" + i);
        }

        try {
            System.out.println(Thread.currentThread().getName()+"进入休眠了");
            Thread.sleep(1000000);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```



**Executors使用可能存在的陷阱**
大型并发系统环境中使用Executors如果不注意可能会出现系统风险。

| 方法名称                                                     |                           存在问题                           |
| ------------------------------------------------------------ | :----------------------------------------------------------: |
| public static ExecutorService newFixedThreadPool(int nThreads) | 允许请求的任务队列长度是Integer.MAX_VALUE，可能出现OOM错误（ java.lang.OutOfMemoryError ) |
| public static ExecutorService newSingleThreadExecutor()      | 允许请求的任务队列长度是Integer.MAX_VALUE，可能出现OOM错误（ java.lang.OutOfMemoryError ) |
| public static ExecutorService newCachedThreadPool()          | 创建的线程数量最大上限是Integer.MAX_VALUE,线程数可能会随着任务1:1增长，也可能出现OOM错误( java.lang.OutOfMemoryError ) |
| public static scheduledExecutorService newScheduledThreadPool(int corePoolSize) | 创建的线程数量最大上限是Integer.MAX_VALUE,线程数可能会随着任务1:1增长，也可能出现OOM错误( java.lang.OutOfMemoryError ) |



**Executors工具类底层是基于什么方式实现的线程池对象?**

- 线程池ExecutorService的实现类:ThreadPoolExecutor



**Executors是否适合做大型互联网场景的线程池方案?**

- 不合适。
- 建议使用ThreadPoolExecutor来指定线程池参数，这样可以明确线程池的运行规则，规避资源耗尽的风险。



##### 线程池常见面试题



**临时线程什么时候创建啊?**

新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程。

心



**什么时候会开始拒绝任务?**

核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始任务拒绝。



#### 定时器

定时器是一种控制任务延时调用，或者周期调用的技术。

作用：闹钟、定时邮件发送。



定时器的实现方式

方式一：Timer

方式二： ScheduledExecutorService



| 构造器         |        说明         |
| -------------- | :-----------------: |
| public Timer() | 创建Timer定时器对象 |



| 方法                                                         |                   说明                   |
| ------------------------------------------------------------ | :--------------------------------------: |
| public void schedule(TimerTask task，long delay, long period) | 开启一个定时器,按照计划处理TimerTask任务 |



```java
public class Main {

    public static void main(String[] args) throws Exception {
        // Timer定时器的使用和了解
        // 1、创建Timer定时器 定时器本身就是一个单线程
        Timer timer = new Timer();

        // 2、调用方法 处理定时任务
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行A" + new Date());
            }
        },0,2000);

        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行B"+ new Date());
            }
        },0,2000);

    }

}
```



Timer定时器的特点和存在的问题

1. Timer是单线程，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入
2. 可能因为其中的某个任务的异常使Timer线程死掉，从而影响后续任务执行



#### scheduledExecutorService定时器（这个更好）

ScheduledExecutorservice是idk1.5中引入了并发包，目的是为了弥补Timer的缺陷,ScheduledExecutorService内部为线程池。

scheduledExecutorService的优点

基于线程池，某个任务的执行情况不会影响其他定时任务的执行。



```java
public class Main {

    public static void main(String[] args) throws Exception {
        // Timer定时器的使用和了解
        // 1、创建ScheduledExecutorService线程池，做定时器
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3);

        // 2、开启定时任务
        scheduledExecutorService.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行输出A---"+new Date());
                try {
                    Thread.sleep(100000);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },0,2,TimeUnit.SECONDS);


        scheduledExecutorService.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行输出B---"+new Date());
            }
        },0,2,TimeUnit.SECONDS);


        scheduledExecutorService.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行输出C---"+new Date());
                System.out.println(10/0);
            }
        },0,2,TimeUnit.SECONDS);

    }

}
```



#### 并发并行

正在运行的程序（软件）就是一个独立的进程，线程是属于进程的，多个线程其实是并发与并行同时进行的。



并发的理解：

- CPU同时处理线程的数量有限。
- CPU会轮询为系统的每个线程服务，由于CPU切换的速度很快，给我们的感觉这些线程在同时执行，这就是并发。



**简单说说并发和并行的含义**

并发：CPU分时轮询的执行线程。





#### 线程的生命周期



**线程的状态**

线程的状态：也就是线程从生到死的过程，以及中间经历的各种状态及状态转换。理解线程的状态有利于提升并发编程的理解能力



**Java线程的状态**

Java总共定义了6种状态

6种状态都定义在Thread类的内部枚举类中

源码

```java
public enum State {
    /**
     * Thread state for a thread which has not yet started.
     */
    NEW,

    /**
     * Thread state for a runnable thread.  A thread in the runnable
     * state is executing in the Java virtual machine but it may
     * be waiting for other resources from the operating system
     * such as processor.
     */
    RUNNABLE,

    /**
     * Thread state for a thread blocked waiting for a monitor lock.
     * A thread in the blocked state is waiting for a monitor lock
     * to enter a synchronized block/method or
     * reenter a synchronized block/method after calling
     * {@link Object#wait() Object.wait}.
     */
    BLOCKED,

    /**
     * Thread state for a waiting thread.
     * A thread is in the waiting state due to calling one of the
     * following methods:
     * <ul>
     *   <li>{@link Object#wait() Object.wait} with no timeout</li>
     *   <li>{@link #join() Thread.join} with no timeout</li>
     *   <li>{@link LockSupport#park() LockSupport.park}</li>
     * </ul>
     *
     * <p>A thread in the waiting state is waiting for another thread to
     * perform a particular action.
     *
     * For example, a thread that has called <tt>Object.wait()</tt>
     * on an object is waiting for another thread to call
     * <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on
     * that object. A thread that has called <tt>Thread.join()</tt>
     * is waiting for a specified thread to terminate.
     */
    WAITING,

    /**
     * Thread state for a waiting thread with a specified waiting time.
     * A thread is in the timed waiting state due to calling one of
     * the following methods with a specified positive waiting time:
     * <ul>
     *   <li>{@link #sleep Thread.sleep}</li>
     *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
     *   <li>{@link #join(long) Thread.join} with timeout</li>
     *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
     *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
     * </ul>
     */
    TIMED_WAITING,

    /**
     * Thread state for a terminated thread.
     * The thread has completed execution.
     */
    TERMINATED;
}
```



线程的六种状态：

1. 新建状态（ NEW )	创建线程对象
2. 就绪状态（RUNNABLE)	start方法
3. 阻塞状态（BLOCKED )	无法获得锁对象
4. 等待状态（WAITING )	wait方法
5. 计时等待（TIMED_WAITING )	sleep方法
6. 结束状态（TERMINATED )	全部代码运行完毕



### 网络编程

网络编程可以让程序与网络上的其他设备中的程序进行数据交互。

常见的通信模式有如下2种形式: 客户端 Client-Server(Cs)、浏览器 Browser/Server(BS)



实现网络编程关键的三要素

- iP地址：设备在网络中的地址，是唯一的标识。
- 端口：应用程序在设备中唯一的标识。
- 协议：数据在网络中传输的规则，常见的协议有UDP协议和TCP协议。



IP地址

lP ( Internet Protocol):全称”互联网协议地址”，是分配给上网设备的唯一标志。

常见的IP分类为：IPv4和IPv6



IP地址形式：

公网地址、和私有地址(局域网使用)。

192.168.开头的就是常见的局域网地址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用。



#### IP地址的代表类lnetAddress

```java
public class Main {

    public static void main(String[] args) throws Exception {
        // 获取本机地址对象
        InetAddress ip1 = InetAddress.getLocalHost();
        System.out.println(ip1);
        System.out.println(ip1.getHostName());
        System.out.println(ip1.getHostAddress());

        // 获取域名ip地址
        InetAddress ip2 = InetAddress.getByName("www.baidu.com");
        System.out.println(ip2);
        System.out.println(ip2.getHostName());
        System.out.println(ip2.getHostAddress());

        // 获取公网ip对象
        InetAddress ip3 = InetAddress.getByName("112.80.248.76");
        System.out.println(ip3);
        System.out.println(ip3.getHostName());
        System.out.println(ip3.getHostAddress());

        // 判断是否能通 Ping 5s 之前测试是否能可通
        System.out.println(ip3.isReachable(3000));
    }


}
```



#### 端口号

端口号：标识正在计算机设备上运行的进程（程序)，被规定为一个16位的二进制，范围是0~65535。

一个设备中，不能出现2个应用程序的端口号一样，如果一样会出现端口冲突错误。



端口类型

- 周知端口：0~1023，被预先定义的知名应用占用(如:HTTP占用80，FTP占用21)
- 注册端口：1024~49151，分配给用户进程或某些应用程序。(如: Tomcat占用8080，MySQL占用3306)
- 动态端口：49152到65535，之所以称为动态端口，是因为它一般不固定分配某种进程，而是动态分配。

注意：我们自己开发的程序选择注册端口，且一个设备中不能出现两个程序的端口号一样，否则出错。



#### 通信协议

计算机网络中，连接和通信数据的规则被称为网络通信协议

- OSI参考模型:世界互联协议标准，全球通信规范，由于此模型过于理想化，未能在因特网上进行广泛推广。
- TCP/IP参考模型(或TCP/IP协议):事实上的国际标准。



传输层的2个常见协议

- TCP(Transmission Control Protocol) ：传输控制协议
- UDP(User Datagram Protocol)：用户数据披协议



##### TCP协议

TCP是一种面向连接，安全、可靠的传输数据的协议

- 使用TCP协议，必须双方先建立连接，它是一种面向连接的可靠通信协议。
- 传输前，采用“三次握手”方式建立连接，所以是可靠的。
- 在连接中可进行大数据量的传输。
- 连接、发送数据都需要确认，且传输完毕后，还需释放已建立的连接，通信效率较低。



TCP协议通信场景

- 对信息安全要求较高的场景，例如：文件下载、金融等数据通信。



Socket

**客户端发送消息**

需求：客户端实现步骤

1. 创建客户端的Socket对象，请求与服务端的连接。
2. 使用socket对象调用getOutputStream()方法得到字节输出流。
3. 使用字节输出流完成数据的发送。
4. 释放资源:关闭socket管道。

客户端

```java
/**
 * Socket 网络编程入门案例的客户端开发 实现一发一收
 */
public class Client {
    public static void main(String[] args) {


        try {
            // 1、创建socket通信管道请求有服务端的连接
            // 参数一：服务端的ip地址
            // 参数二：服务端的端口
            Socket socket = new Socket("127.0.0.1",8888);

            // 2、从socket 通信管道中得到一个字符输出流 负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3、把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            // 4、发送消息
            ps.print("我是TCP的客户端，连接创建成功");
            ps.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```



服务端

```java
/**
 * 开发Socket网络编程入门服务端 实现接受消息
 */
public class server {

    public static void main(String[] args) throws Exception {
        System.out.println("==服务端==");
        // 1、注册端口
        ServerSocket serverSocket = new ServerSocket(9999);

        // 2、必须调用accept方法：等待接受客户端的socket的连接请求，建立socket通信管道
        Socket socket = serverSocket.accept();

        // 3、从Socket通信管道中得到一个字节输入流
        InputStream is = socket.getInputStream();

        // 4、把字节输入流包装成缓冲字符输入流进行消息的接受
        BufferedReader br = new BufferedReader(new InputStreamReader(is));

        // 5、按照行读取消息
        String msg;
        if ((msg = br.readLine()) != null){
            System.out.println(socket.getRemoteSocketAddress()+"说了："+msg);
        }


    }

}
```













##### UDP协议

- UDP是一种无连接、不可靠传输的协议。
- 将数据源IP、目的地IP和端口封装成数据包，不需要建立连接每个数据包的大小限制在64KB内
- 发送不管对方是否准备好，接收方收到也不确认，故是不可靠的可以广播发送，发送数据结束时无需释放资源，开销小，速度快。



UDP协议通信场景

- 语音通话，视频会话等。



DatagramPacket：数据包对象

| 构造器                                                       |                             说明                             |
| ------------------------------------------------------------ | :----------------------------------------------------------: |
| public DatagramPacket(byte[] buf， int length，InetAddress address,int port) | 创建发送端数据包对象<br/>buf:要发送的内容，字节数组<br />length:要发送内容的字节长度<br />address:接收端的IP地址对象<br />port:接收端的端口号 |
| public DatagramPacket( byte[] buf, int length)               | 创建接收端的数据包对象<br />buf:用来存储接收的内容<br />length:能够接收内容的长度 |



DatagramSocket：发送端和接收端对象

| 构造器                           |                        说明                        |
| -------------------------------- | :------------------------------------------------: |
| public DatagramSocket()          | 创建发送端的Socket对象，系统会随机分配一个端口号。 |
| public DatagramSocket( int port) |         创建接收端的Socket对象并指定端口号         |



DatagramSocket类成员方法

| 构造器                                |    说明    |
| ------------------------------------- | :--------: |
| public void send(DatagramPacket dp)   | 发送数据包 |
| public void receive(DatagramPacket p) | 接收数据包 |



接受端

```java
/**
 * 接受端
 */
public class server {

    public static void main(String[] args) throws Exception {

        System.out.println("======服务端启动======");

        // 创建接受端对象：注册端口
        DatagramSocket socket = new DatagramSocket(8888);

        // 创建一个数据包对象接受数据
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        // 等待接受数据即可
        socket.receive(packet);

        // 接受成功 取出数据
        // 读取多少倒出多少
        int len = packet.getLength();
        String rs = new String(buffer,0,len);
        System.out.println("收到数据：" + rs);

        socket.close();
    }

}
```



发送端

```java
/**
 * 发送端
 */
public class Client {
    public static void main(String[] args) throws Exception {
        System.out.println("======客户端启动======");

        // 创建发送端对象 发送端自带默认的端口号
        DatagramSocket socket = new DatagramSocket();

        // 创建一个数据包对象封装对象
        /**
         *  public DatagramPacket(byte buf[], int offset, int length,
         *  InetAddress address, int port)
         *  参数一：封装要发送的数据
         *  参数二：发送数据的大小
         *  参数三：服务端的IP地址
         *  参数四：服务端的端口
         */
        byte[] buffer = "我是云南的".getBytes();
        DatagramPacket packet = new DatagramPacket(buffer,buffer.length, InetAddress.getLocalHost(),8888);

        // 发送数据出去
        socket.send(packet);

        socket.close();
    }
}
```





需求：

使用UDP通信方式开发接收端和发送端

分析：

- 发送端可以一直发送消息。
- 接收端可以不断的接收多个发送端的消息展示。
- 发送端输入了exit则结束发送端程序。

客户端实现步骤：

1. 创建DatagramSocket对象（发送端对象)
2. 使用while死循环不断的接收用户的数据输入，如果用户输入的exit则退出程序
3. 如果用户输入的不是exit，把数据封装成DatagramPacket
4. 使用DatagramSocket对象的send方法将数据包对象进行发送

```java
/**
 * 发送端
 */
public class Client {
    public static void main(String[] args) throws Exception {
        System.out.println("======服客户端启动======");

        // 创建发送端对象 发送端自带默认的端口号
        DatagramSocket socket = new DatagramSocket();



        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请说：");
            String msg = sc.nextLine();

            if ("exit".equals(msg)){
                System.out.println("离线成功！！");
                socket.close();
                break;
            }

            // 创建一个数据包对象封装对象
            byte[] buffer = msg.getBytes();
            DatagramPacket packet = new DatagramPacket(buffer,buffer.length, InetAddress.getLocalHost(),8888);

            // 发送数据出去
            socket.send(packet);
        }

    }
}
```

接收端实现步骤：

1. 创建DatagramSocket对象并指定端口（接收端对象）
2. 创建DatagramPacket对象接收数据（数据包对象)
3. 使用while死循环不断的进行第4步
4. 使用DatagramSocket对象的receive方法传入DatagramPacket对象

```java
/**
 * 接受端
 */
public class server {

    public static void main(String[] args) throws Exception {

        System.out.println("======服务端启动======");

        // 创建接受端对象：注册端口
        DatagramSocket socket = new DatagramSocket(8888);

        // 创建一个数据包对象接受数据
        byte[] buffer = new byte[1024 * 64];
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        while (true) {
            // 等待接受数据即可
            socket.receive(packet);

            // 接受成功 取出数据
            // 读取多少倒出多少
            int len = packet.getLength();
            String rs = new String(buffer,0,len);
            System.out.println("收到来自：" + packet.getAddress() + ", 端口是"+packet.getPort()+"的消息：" + rs);

        }
    }
```



##### UDP通信-广播和组播

UDP的三种通信方式

- 单播：单台主机与单台主机之间的通信。
- 广播：当前主机与所在网络中的所有主机通信。
- 组播：当前主机与选定的一组主机的通信。



###### UDP实现广播

- 使用广播地址:255.255.255.255

- 具体操作：

  - 发送端发送的数据包的目的地写的是广播地址、且指定端口。(255.255.255.255,9999)

  - 本机所在网段的其他主机的程序只要匹配端口成功即就可以收到消息了。(9999)

```java
// 创建一个数据包对象封装对象
byte[] buffer = msg.getBytes();
DatagramPacket packet = new DatagramPacket(buffer,buffer.length, InetAddress.getByName("255.255.255.255"),9999);
```



###### UDP实现组播

- 使用组播地址:224.0.0.0 ~ 239.255.255.255

- 具体操作：

  - 发送端的数据包的目的地是组播IP（(例如:224.0.1.1，端口:9999)

  - 接收端必须绑定该组播IP(224.0.1.1)，端口还要对应发送端的目的端口9999，这样即可接收该组播消息。

  - DatagramSocket的子类MulticastSocket可以在接收端绑定组播IP



接收端

```java
System.out.println("======服务端启动======");

// 创建接受端对象：注册端口
MulticastSocket socket = new MulticastSocket(9999);
// 陈当前接收端加入到一个组播组中去：绑定对应的组播消息得到组播ip
// socket.joinGroup(InetAddress.getByName("224.0.1.1"));
socket.joinGroup(new InetSocketAddress(InetAddress.getByName("224.0.1.1"),9999),
               NetworkInterface.getByInetAddress(InetAddress.getLocalHost()));
```



发送端

```java
// 创建一个数据包对象封装对象
byte[] buffer = msg.getBytes();
DatagramPacket packet = new DatagramPacket(buffer,buffer.length, InetAddress.getByName("224.0.1.1"),9999);
```



###### UDP实现多发多收

客户端使用循环反复的发送消息

服务端使用循环反复的接受消息



问题：

客户端能实现多发，服务端只有一个线程就只能与一个客户端进行通信

实现步骤：

客户端

```java
public class Client {
    public static void main(String[] args) {

        System.out.println("==客户端==");

        try {
            // 1、创建socket通信管道请求有服务端的连接
            // 参数一：服务端的ip地址
            // 参数二：服务端的端口
            Socket socket = new Socket("127.0.0.1",9999);

            // 2、从socket 通信管道中得到一个字符输出流 负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3、把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true){
                System.out.println("请说：");
                String msg = sc.nextLine();
                ps.println(msg);
                ps.flush();
            }



        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```



服务端

```java
public class server {

    public static void main(String[] args) {
        try {
            System.out.println("==服务端==");
            // 1、注册端口
            ServerSocket serverSocket = new ServerSocket(9999);

            // 定义一个死循环由主线程负责不断的接收客户端的socket管道连接
            while (true) {

                // 2、每接收到一个客户端的socket管道，交给一个独立的子线程负责读取消息
                Socket socket = serverSocket.accept();
                System.out.println(socket.getRemoteSocketAddress()+"上线！！");

                // 3、开始创建独立线程处理socket
                new ServerReaderThread(socket).start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }


    }

}
```



线程 ServerReaderThread

```java
public class ServerReaderThread extends Thread{

    private Socket socket;

    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();

            // 把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            // 按照行读取消息
            String msg;

            while ((msg = br.readLine())!=null){
                System.out.println(socket.getRemoteSocketAddress()+"说："+msg);
            }


        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress()+"下线！！！");
        }
    }
}
```



###### 线程池优化

引入线程池处理多个客户端的消息，实现通信

客户端

```java
public class Client {
    public static void main(String[] args) {

        System.out.println("==客户端==");

        try {
            // 1、创建socket通信管道请求有服务端的连接
            // 参数一：服务端的ip地址
            // 参数二：服务端的端口
            Socket socket = new Socket("127.0.0.1",9999);

            // 2、从socket 通信管道中得到一个字符输出流 负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3、把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true){
                System.out.println("请说：");
                String msg = sc.nextLine();
                ps.println(msg);
                ps.flush();
            }



        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```



服务端

```java
public class server {

    // 使用静态变量记住一个线程池对象
    private static ExecutorService pool = new ThreadPoolExecutor(3,5,6, TimeUnit.SECONDS,
            new ArrayBlockingQueue<>(2),
            Executors.defaultThreadFactory(),
            new ThreadPoolExecutor.AbortPolicy());

    public static void main(String[] args) {
        try {
            System.out.println("==服务端==");
            // 1、注册端口
            ServerSocket serverSocket = new ServerSocket(9999);

            // 定义一个死循环由主线程负责不断的接收客户端的socket管道连接
            while (true) {

                // 2、每接收到一个客户端的socket管道
                Socket socket = serverSocket.accept();
                System.out.println(socket.getRemoteSocketAddress()+"上线！！");

                // 将任务(读取消息)交给线程池
                Runnable target =  new ServerReaderRunnable(socket);

                pool.execute(target);

            }
        } catch (Exception e) {
            e.printStackTrace();
        }


    }

}
```



线程 ServerReaderRunnable

```java
public class ServerReaderRunnable implements Runnable{

    private Socket socket;
    public ServerReaderRunnable(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();

            // 把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            // 按照行读取消息
            String msg;

            while ((msg = br.readLine())!=null){
                System.out.println(socket.getRemoteSocketAddress()+"说："+msg);
            }


        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress()+"下线！！！");
        }

    }
}
```



**本次使用线程池的优势在哪里?**

- 服务端可以复用线程处理多个客户端，可以避免系统瘫痪。
- 适合客户端通信时长较短的场景。



#### 即时通信

即时通信是什么含义，要实现怎么样的设计?

- 即时通信，是指一个客户端的消息发出去，其他客户端可以接收到
- 之前我们的消息都是发给服务端的
- 即时通信需要进行端转发的设计思想

##### 客户端

```java
/**
 * 即时通信
 * 客户端既要发也要收
 * 客户端发送消息
 * 客户端随时可能收到消息
 */

public class Client {
    public static void main(String[] args) {

        System.out.println("==客户端==");

        try {
            // 1、创建socket通信管道请求有服务端的连接
            // 参数一：服务端的ip地址
            // 参数二：服务端的端口
            Socket socket = new Socket("127.0.0.1",9999);

            // 创建一个独立的线程专门负责这个客户端的读消息（服务端随时可能转发消息过来）
            new ClientReaderThread(socket).start();


            // 2、从socket 通信管道中得到一个字符输出流 负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3、把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true){
                System.out.println("请说：");
                String msg = sc.nextLine();
                ps.println(msg);
                ps.flush();
            }



        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```



##### 客户端线程 ClientReaderThread

```java
public class ClientReaderThread extends Thread{

    private Socket socket;

    public ClientReaderThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();

            // 把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            // 按照行读取消息
            String line;

            while ((line = br.readLine())!=null){
                System.out.println("收到消息："+line);
            }

        } catch (Exception e) {
            System.out.println("断开连接");
        }
    }
}
```



##### 服务端

```java
public class server {

    // 使用静态变量记住一个线程池对象
    private static ExecutorService pool = new ThreadPoolExecutor(3,5,6, TimeUnit.SECONDS,
            new ArrayBlockingQueue<>(2),
            Executors.defaultThreadFactory(),
            new ThreadPoolExecutor.AbortPolicy());

    // 定义一个静态的List集合存储当前全部在线的socket管道
    public  static List<Socket> allOnlineSockets = new ArrayList<>();

    public static void main(String[] args) {
        try {
            System.out.println("==服务端==");
            // 1、注册端口
            ServerSocket serverSocket = new ServerSocket(9999);

            // 定义一个死循环由主线程负责不断的接收客户端的socket管道连接
            while (true) {

                // 2、每接收到一个客户端的socket管道
                Socket socket = serverSocket.accept();
                System.out.println(socket.getRemoteSocketAddress()+"上线！！");

                // 存入当前socket信息
                allOnlineSockets.add(socket);

                // 将任务(读取消息)交给线程池
                Runnable target =  new ServerReaderThread(socket);

                pool.execute(target);

            }
        } catch (Exception e) {
            e.printStackTrace();
        }


    }

}
```



##### 服务端线程 ServerReaderThread

```java
public class ServerReaderThread extends Thread{

    private Socket socket;
    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();

            // 把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            // 按照行读取消息
            String line;
            while ((line = br.readLine())!=null){
                System.out.println(socket.getRemoteSocketAddress()+"说："+line);
                // 把这个消息进行端口转发给全部客户端socket管道
                sendMsgToAll(line);

            }


        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress()+"下线！！！");
            // 移除
            server.allOnlineSockets.remove(socket);
        }

    }

    private void sendMsgToAll(String msg) throws Exception {

        for (Socket socket : server.allOnlineSockets) {
            PrintStream ps = new PrintStream(socket.getOutputStream());
            ps.println(msg);
            ps.flush();
        }

    }
}
```



#### B/S系统

**TCP通信如何实现BS请求网页信息回来呢?**

- 客户端使用浏览器发起请求（不需要开发客户端)
- 服务端必须按照浏览器的协议规则响应数据。



**浏览器使用什么协议规则呢?**

- HTTP协议

