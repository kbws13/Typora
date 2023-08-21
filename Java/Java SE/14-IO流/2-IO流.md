# 原理

I/O是Input/Output的缩写， I/O技术是非常实用的技术，用于 处理设备之间的数据传输。如读/写文件，网络通讯等

Java程序中，对于数据的输入/输出操作以“流(stream)” 的 方式进行。

java.io包下提供了各种“流”类和接口，用以获取不同种类的 数据，并通过标准的方法输入或输出数据

![image-20230104130823712](2-IO%E6%B5%81.assets/image-20230104130823712.png)

# 流的分类

![image-20230104131054013](2-IO%E6%B5%81.assets/image-20230104131054013.png)

## IO流体系

![image-20230104131113334](2-IO%E6%B5%81.assets/image-20230104131113334.png)

## 节点流和处理流

节点流

直接从数据源或目的地读写数据

![image-20230104131619457](2-IO%E6%B5%81.assets/image-20230104131619457.png)

处理流

不直接连接到数据源或目的地，而是“连接”在已存 在的流（节点流或处理流）之上，通过对数据的处理为程序提 供更为强大的读写功能

![image-20230104131740567](2-IO%E6%B5%81.assets/image-20230104131740567.png)

## InputStream&Reader

- InputStream 和 Reader 是所有输入流的基类。 

- InputStream（典型实现：FileInputStream） 

    - int read() 
    - int read(byte[] b) 

    - int read(byte[] b, int off, int len) 

- Reader（典型实现：FileReader） 

    - int read() 

    - int read(char [] c)

    - int read(char [] c, int off, int len) 

程序中打开的文件 IO 资源不属于内存里的资源，垃圾回收机制无法回收该资 源，所以应该显式关闭文件 IO 资源。 FileInputStream 从文件系统中的某个文件中获得输入字节。FileInputStream  用于读取非文本数据之类的原始字节流。要读取字符流，需要使用 FileReader

### InputStream

- `int read()` 从输入流中读取数据的下一个字节。返回 0 到 255 范围内的 int 字节值。如果因 为已经到达流末尾而没有可用的字节，则返回值 -1。 

- `int read(byte[] b)` 从此输入流中将最多 b.length 个字节的数据读入一个 byte 数组中。如果因为已 经到达流末尾而没有可用的字节，则返回值 -1。否则以整数形式返回实际读取 的字节数。 

- `int read(byte[] b, int off,int len) `将输入流中最多 len 个数据字节读入 byte 数组。尝试读取 len 个字节，但读取 的字节也可能小于该值。以整数形式返回实际读取的字节数。如果因为流位于 文件末尾而没有可用的字节，则返回值 -1。 

- `public void close() throws IOException` 关闭此输入流并释放与该流关联的所有系统资源

### Reader

- `int read()` 读取单个字符。作为整数读取的字符，范围在 0 到 65535 之间 (0x00-0xffff)（2个 字节的Unicode码），如果已到达流的末尾，则返回 -1 

- `int read(char[] cbuf)` 将字符读入数组。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。 

- `int read(char[] cbuf,int off,int len) `将字符读入数组的某一部分。存到数组cbuf中，从off处开始存储，最多读len个字 符。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。 

- `public void close() throws IOException` 关闭此输入流并释放与该流关联的所有系统资源。

## OutputStream & Writer

OutputStream 和 Writer 也非常相似：

- void write(int b/int c);

- void write(byte[] b/char[] cbuf); 

- void write(byte[] b/char[] buff, int off, int len);

- void flush(); 

- void close(); 需要先刷新，再关闭此流 

因为字符流直接以字符作为操作单位，所以 Writer 可以用字符串来替换字符数组， 即以 String 对象作为参数 

- void write(String str);

- void write(String str, int off, int len);

FileOutputStream 从文件系统中的某个文件中获得输出字节。FileOutputStream  用于写出非文本数据之类的原始字节流。要写出字符流，需要使用 FileWriter

### OutputStream

- `void write(int b)`将指定的字节写入此输出流。write 的常规协定是：向输出流写入一个字节。要写 入的字节是参数 b 的八个低位。b 的 24 个高位将被忽略。 即写入0~255范围的

- `void write(byte[] b) `将 b.length 个字节从指定的 byte 数组写入此输出流。write(b) 的常规协定是：应该 与调用 write(b, 0, b.length) 的效果完全相同。 

- `void write(byte[] b,int off,int len) `将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。 

- `public void flush()throws IOException` 刷新此输出流并强制写出所有缓冲的输出字节，调用此方法指示应将这些字节立 即写入它们预期的目标。 

- `public void close() throws IOException` 关闭此输出流并释放与该流关联的所有系统资源

### Writer

- `void write(int c) `写入单个字符。要写入的字符包含在给定整数值的 16 个低位中，16 高位被忽略。 即 写入0 到 65535 之间的Unicode码。 

- `void write(char[] cbuf)` 写入字符数组。 

- `void write(char[] cbuf,int off,int len)` 写入字符数组的某一部分。从off开始，写入len个字符 

- `void write(String str)` 写入字符串。 

- `void write(String str,int off,int len)` 写入字符串的某一部分。 

- `void flush()` 刷新该流的缓冲，则立即将它们写入预期目标。 

- `public void close() throws IOException` 关闭此输出流并释放与该流关联的所有系统资源。

# 节点流

## 读取文件

1. 建立一个流对象，将已存在的一个文件加载进流

    `FileReader fr = new FileReader(new File(“Test.txt”)); `

2. 创建一个临时存放数据的数组

    `char[] ch = new char[1024]; `

3. 调用流对象的读取方法将流中的数据读入到数组中

    `fr.read(ch); `

4. 关闭资源

    ` fr.close();`

![image-20230104133641167](2-IO%E6%B5%81.assets/image-20230104133641167.png)

示例：

```java
BufferedReader bf = null;
int readRow = 0;//统计行数
int[][] sparseArr2 = new int[count+1][3];
try {
    bf = new BufferedReader(new FileReader("./map.txt"));
    String line = null;
    //统计行数
    while ((line = bf.readLine())!=null){
        String[] temp = line.split("\t");
        for (int i = 0; i < temp.length; i++) {
            sparseArr2[readRow][i] = Integer.parseInt(temp[i]);
        }
        readRow++;
    }
} catch (IOException e) {
    throw new RuntimeException(e);
} finally {
    if(bf!=null){
        try {
            bf.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



## 写入文件

1. 创建流对象，建立数据存放文件

    `FileWriter fw = new FileWriter(new File(“Test.txt”));`

2. 调用流对象的写入方法，将数据写入流

    `fw.write(“atguigu-songhongkang”); `

3. 关闭流资源，并将流中的数据清空到文件中

    `fw.close();`

![image-20230104134421506](2-IO%E6%B5%81.assets/image-20230104134421506.png)

示例：

```java
BufferedWriter writer = null;
try {
    writer = new BufferedWriter(new FileWriter("./map.txt",true));
    for (int i = 0; i < sparseArr.length; i++) {
        for (int j = 0; j < sparseArr[0].length; j++) {
            writer.write(sparseArr[i][j]+"\t");
        }
        writer.write("\n");
    }
    writer.newLine();
} catch (IOException e) {
    throw new RuntimeException(e);
}finally {
    if(writer!=null){
        try {
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



## 注意

![image-20230104134450674](2-IO%E6%B5%81.assets/image-20230104134450674.png)

# 缓冲流

![image-20230104134755287](2-IO%E6%B5%81.assets/image-20230104134755287.png)

![image-20230104134804732](2-IO%E6%B5%81.assets/image-20230104134804732.png)

![image-20230104134813837](2-IO%E6%B5%81.assets/image-20230104134813837.png)

![image-20230104134823592](2-IO%E6%B5%81.assets/image-20230104134823592.png)

# 转换流

![image-20230104134837853](2-IO%E6%B5%81.assets/image-20230104134837853.png)

## InputStreamReader

实现将字节的输入流按指定字符集转换为字符的输入流

 需要和InputStream“套接”。

![image-20230104134912040](2-IO%E6%B5%81.assets/image-20230104134912040.png)

## OutputStreamWriter

实现将字符的输出流按指定字符集转换为字节的输出流

需要和OutputStream“套接”

![image-20230104134939150](2-IO%E6%B5%81.assets/image-20230104134939150.png)

![image-20230104134947513](2-IO%E6%B5%81.assets/image-20230104134947513.png)

# 标准输入、输出流

System.in和System.out分别代表了系统标准的输入和输出设备

默认输入设备是：键盘，输出设备是：显示器

System.in的类型是InputStream

System.out的类型是PrintStream，其是OutputStream的子类 FilterOutputStream 的子类

重定向：通过System类的setIn，setOut方法对默认设备进行改变。

- `public static void setIn(InputStream in)`

- `public static void setOut(PrintStream out)`

![image-20230104135154694](2-IO%E6%B5%81.assets/image-20230104135154694.png)



# 打印流

实现将基本数据类型的数据格式转化为字符串输出

打印流：PrintStream和PrintWriter

- 提供了一系列重载的print()和println()方法，用于多种数据类型的输出

- PrintStream和PrintWriter的输出不会抛出IOException异常

- PrintStream和PrintWriter有自动flush功能

- PrintStream 打印的所有字符都使用平台的默认字符编码转换为字节。 在需要写入字符而不是写入字节的情况下，应该使用 PrintWriter 类。

- System.out返回的是PrintStream的实例

![image-20230104135226667](2-IO%E6%B5%81.assets/image-20230104135226667.png)

# 数据流

![image-20230104135246182](2-IO%E6%B5%81.assets/image-20230104135246182.png)

![image-20230104135255218](2-IO%E6%B5%81.assets/image-20230104135255218-16728115755111.png)

# 对象流

