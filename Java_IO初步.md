### Java IO初步 ###

1. `File`对象：

   1. 构造`File`对象，如：

      ```java
      File f = new File("C:\\Windows\\notepad.exe");
      //f.getPath()返回构造方法传入的路径
      //f.getAbsolutePath()返回绝对路径
      //f.getCanonicalPath()返回规范路径
      ```

   2. `File`目录或者文件：`File`对象既可以表示文件，也可以表示目录。需要注意的是，构造一个`File`对象，即使传入的文件或者目录不存在，代码也不会出错，因为构造一个`File`对象，并不会导致任何磁盘操作。只有当调用`File`对象的某些方法时才真正进行磁盘操作。可用`isFile()`判断是否是已经存在的文件，`isDirectory()`判断是否是已经存在的目录。

   3. 文件权限：`File`对象获取到一个文件时，还可以进一步判断文件的权限和大小：

      ```text
      boolean canRead()：是否可读；
      boolean canWrite()：是否可写；
      boolean canExecute()：是否可执行；
      long length()：文件字节大小。
      ```

   4. 创建和删除：当`File`对象表示一个**文件**时,可以通过`createNewFile()`创建一个新文件，用`delete`删除该文件。

2. `Path`对象：`Java`标准库还提供了一个`Path`对象，它在`java.nio.file`包中。`Path`对象和`File`对象类似。但是操作起来更简单，如：

   ```java
   public class Main {
       public static void main(String[] args) throws IOException {
           Path p1 = Paths.get(".", "project", "study"); // 构造一个Path对象
           System.out.println(p1);
           Path p2 = p1.toAbsolutePath(); // 转换为绝对路径
           System.out.println(p2);
           Path p3 = p2.normalize(); // 转换为规范路径
           System.out.println(p3);
           File f = p3.toFile(); // 转换为File对象
           System.out.println(f);
           for (Path p : Paths.get("..").toAbsolutePath()) { // 可以直接遍历Path
               System.out.println("  " + p);
           }
       }
   }
   ```

   如果需要对目录进行复杂的拼接，遍历等操作，使用`Path`对象更方便。