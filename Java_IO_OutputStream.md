### `Java_IO_OutputStream` ###

1. `OutputStream`也是抽象类，它是所有输出流的超类。这个抽象类定义的一个最重要的方法是`void write(int b)`，签名如下：

   ```java
   public abstract void write(int b) throws IOException;
   ```

   这个方法会写入一个字节到输出流，但**需要注意的是**，虽然传入的是`int`参数，但只会写入一个字节，即只写入`int`最低`8`位表示字节的部分（相当于`b&0xff`)

2. `flush`方法：如果不相等缓存区满了以后再发送信息，可以在每次向缓存区写入信息后就立即使用`flush`方法强制把信息发送。

3. `void write(byte[])`方法：每次写入一个字节未免麻烦，更常见的是一次性写入多个字节。这时，可以用`OutputStream`提供的重载方法`void write(byte[])`来实现：

   ```java
   public void writeFile() throws IOException {
       try (OutputStream output = new FileOutputStream("out/readme.txt")) {
           output.write("Hello".getBytes("UTF-8")); // Hello
       } // 编译器在此自动为我们写入finally并调用close()
   }
   ```

   同`InputStream`的`read()`方法，`write()`方法也是阻塞的。