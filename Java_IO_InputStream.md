### `Java_IO_InputStream` ###

1. `InputStream`是一个**抽象类**，不是一个**接口**，它是所有输入流的超类。其最重要的方法就是`int_read()`，声明如下：

   ```java
   public abstract int read() throws IOException;
   ```

   这个方法会读取输入流的下一个字节，并且返回字节表示的`int`值(0~255)。如果已经读到末尾，返回`-1`表示不能再继续读取了。

   `FileInputStream`是`InputStream`的一个子类，如下演示了如何完整读取一个`FileInputStream`的所有字节：

   ```java
   public void readFile() throws IOException {
       InputStream input = null;
       try {
           input = new FileInputStream("src/readme.txt");
           int n;
           while ((n = input.read()) != -1) { // 利用while同时读取并判断
               System.out.println(n);
           }
       } finally {
           if (input != null) { input.close(); }
       }
   }
   ```

   用`try ... finally`来编写上述代码当然可以，但是`Java`给我们提供了更好的方法，那就是`Java 7`新引入的`try(resource)`的语法，如下：

   ```java
   public void readFile() throws IOException {
       try (InputStream input = new FileInputStream("src/readme.txt")) {
           int n;
           while ((n = input.read()) != -1) {
               System.out.println(n);
           }
       } // 编译器在此自动为我们写入finally并调用close()
   }
   ```

   实际上，编译器并不会特别地为`InputStream`加上自动关闭。不过，编译器看到`try(resource = ...)`时，查看其中对象是否实现了`java.lang.AutoCloseable`接口，如果实现了，就会自动加上`finally`语句并且调用`close()`方法。`InputStream`和`OutputStream`都实现了这个接口，因此，都可以用在`try(resource)`中。

2. 缓冲：在读取流的时候，一次读取一个字节并不是最高效的方法。很多流都支持一次性读取多个字节到缓存区，对于文件和网络流来说，利用缓冲区一次性读取多个字节效率往往要高很多，`InputSream`提供了两个重载方法来支持读取多个字节：

   ```text
   int read(byte[] b)：读取若干字节并填充到byte[]数组，返回读取的字节数
   int read(byte[] b, int off, int len)：指定byte[]数组的偏移量和最大填充数
   ```

   此方法定义一个`byte[]`数组作为缓冲区，`read()`方法尽可能多地读取字节到缓冲区，但不会超过缓冲区的大小。`read()`方法返回的不再是字节的`int`值，而是返回实际读取了多少个字节。如果返回`-1`表示没有更多数据了。

   利用缓冲区一次性读取多个字节的代码如下：

   ```java
   public void readFile() throws IOException {
       try (InputStream input = new FileInputStream("src/readme.txt")) {
           // 定义1000个字节大小的缓冲区:
           byte[] buffer = new byte[1000];
           int n;
           while ((n = input.read(buffer)) != -1) { // 读取到缓冲区
               System.out.println("read " + n + " bytes.");
           }
       }
   }
   ```

   

3. 阻塞：再调用`InputSream`的`read()`方法时，我们说`read()`方法是阻塞的，因为必须等`read()`方法返回后才能继续执行后面的代码。再者，因为读取`IO`流执行比执行不同代码速度要慢很多，因此，无法确定`read()`方法调用到底要花多长时间。

4. `InputStream`实现类:用`fileInputSteam`可以从文件获取输入流，这是`InputStream`常用的一个实现类。另外，`ByteArrayInputStream`可以在内存中模拟一个`InputStream`。如下代码：

   ```java
   public class Main {
       public static void main(String[] args) throws IOException {
           byte[] data = { 72, 101, 108, 108, 111, 33 };
           try (InputStream input = new ByteArrayInputStream(data)) {
               String s = readAsString(input);
               System.out.println(s);
           }
       }
   
       public static String readAsString(InputStream input) throws IOException {
           int n;
           StringBuilder sb = new StringBuilder();
           while ((n = input.read()) != -1) {
               sb.append((char) n);
           }
           return sb.toString();
   ```

   这是面向抽象编程原则的应用：接受`InputStream`抽象类型而不是具体的`FileInputStream`类型，从而使得代码可以处理`InputStream`的任意类。