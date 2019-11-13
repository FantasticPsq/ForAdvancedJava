### 操作`Zip` ###

1. `ZipInputStream`是一种`FileInputStream`,它可以直接读取`Zip`包中的内容：

- ```
  ┌───────────────────┐
  │    InputStream    │
  └───────────────────┘
            ▲
            │
  ┌───────────────────┐
  │ FilterInputStream │
  └───────────────────┘
            ▲
            │
  ┌───────────────────┐
  │InflaterInputStream│
  └───────────────────┘
            ▲
            │
  ┌───────────────────┐
  │  ZipInputStream   │
  └───────────────────┘
            ▲
            │
  ┌───────────────────┐
  │  JarInputStream   │
  └───────────────────┘
  ```

  另一个`JarInputStream`是从`ZipInputStream`的一个派生，他增加的主要功能是直接读取`jar`文件里面的的`MANIFEST.MF`文件。因为本质上`jar`包就是`Zip`包，只是额外附加了一些固定的描述文件。

2. 读取`Zip`包：要创建一个`ZipInputStream`通常传入一个`FileInputStream`作为数据源，然后，循环调用`getNextEntry`方法，直到返回`null`，表示`zip`流结束。一个`ZipEntry`表示一个压缩文件或者目录，如果是压缩文件，我们就用`read()`方法不断读取，知道返回`-1`：

   ```java
   try (ZipInputStream zip = new ZipInputStream(new FileInputStream(...))) {
       ZipEntry entry = null;
       while ((entry = zip.getNextEntry()) != null) {
           String name = entry.getName();
           if (!entry.isDirectory()) {
               int n;
               while ((n = zip.read()) != -1) {
                   ...
               }
           }
       }
   }
   ```

3. 写入`zip`包：`ZipOutputStream`是一种`FilterOutputStream`，它可以直接写入内容到`Zip`包。要创建一个`ZipOutputStream`，通常是包装一个`FileOutputStream`，然后，每写入一个文件前，先掉用`putNextEntry()`,然后用`write()`写入`byte[]`数据，写入完毕后，调用`closeEntry()`结束这个文件的打包：

   ```java
   try (ZipOutputStream zip = new ZipOutputStream(new FileOutputStream(...))) {
       File[] files = ...
       for (File file : files) {
           zip.putNextEntry(new ZipEntry(file.getName()));
           zip.write(getFileDataAsBytes(file));
           zip.closeEntry();
       }
   }
   ```

   

