### 第三方算法库`BouncyCastle` ###

1. 简介：`BouncyCastle`就是一个提供了很多哈希算法和加密算法的第三方库，它提供了Java标准库没有的一些算法，例如，`RipeMD160`哈希算法。

2. 使用`BouncyCastle`：首先，我们必须把`BouncyCastle`提供的`jar`包放到`classpath`中，这个`jar`包就是`bcprov-jdk15-xxx.jar`。

   Java标准库的`java.security`包提供了一种标准机制，允许第三方提供商无缝接入。比如我们要使用`BouncyCastle`提供的`RipeMD160`算法，需要先把`BouncyCastle`注册一下：

   ```java
   public class Main {
       public static void main(String[] args) throws Exception {
           // 注册BouncyCastle:
           Security.addProvider(new BouncyCastleProvider());
           // 按名称正常调用:
           MessageDigest md = MessageDigest.getInstance("RipeMD160");
           md.update("HelloWorld".getBytes("UTF-8"));
           byte[] result = md.digest();
           System.out.println(new BigInteger(1, result).toString(16));
       }
   }
   ```

   注册(`Security.addProvider(new BouncyCastleProvider))`只需要一次，后续就可以直接使用。