### Java读取`classpath`资源 ###

1. 原因：由于存在不同系统中路径不同的问题，从`classpath`读取资源文件就可以避免不同环境文件下路径不一样的问题：如果我们把资源文件放在`classpath`中，就不用关心它的实际存在路径。

2. 读取`classpath`中的资源文件，路径总是以`/`开头，先获取当前的`class`对象，然后调用`getResourceAsStream()`就可以直接从`classpath`读取任意的资源文件：

   ```java
   try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
       // TODO:
   }
   ```

   调用`getResourceAsStream()`需要特别注意的是，**如果资源文件不存在，它将返回`null`**，因此，我们需要检查返回的`InputStream`是否为`null`，表示资源文件再`classpath`中没有找到：

   ```java
   try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
       if (input != null) {
           // TODO:
       }
   }
   ```

   如果把默认的配置放到`jar`包·中，在从外部文件系统读取一个可选的配置文件，就可以做到既有默认的配置文件，又可以让用户自己修改配置：

   ```java
   Properties props = new Properties();
   props.load(inputStreamFromClassPath("/default.properties"));
   props.load(inputStreamFromFile("./conf.properties"));
   ```

   

