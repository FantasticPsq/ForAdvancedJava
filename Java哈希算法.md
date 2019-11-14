### Java哈希算法 ###

1. 简介：哈希算法(`Hash`)又称摘要算法(`Digest`),它的作用是：对任意一组输入数据进行计算，得到一个固定长度的输出摘要。

   哈希算法最重要的特点是：

   ```text
   相同的输入一定得到相同的输出;
   不同的输入大概率得到不同的输出。
   ```

   哈希算法的目的就是为了验证原始数据是否被篡改。

   Java字符串的`hashCode()`方法就是一个哈希算法，他的输入是任意字符串，输出是固定的4个字节`int`整数：

   ```java
   "hello".hashCode(); // 0x5e918d2
   "hello, java".hashCode(); // 0x7a9d88e8
   "hello, bob".hashCode(); // 0xa0dbae2f
   ```

   相同的字符串永远会计算出相同的`hashCode`，否则基于`hashCode`定位的`HashMap`就无法正常工作。

2. 哈希碰撞

   哈希碰撞是指，两个不同的输入得到了相同的输出：

   ```java
   "AaAaAa".hashCode(); // 0x7460e8c0
   "BBAaBB".hashCode(); // 0x7460e8c0
   ```

   这是问题就出现了：碰撞能不能避免?答案是不能，因为输出字符串长度是固定的，`String`的`hashCode()`方法输出的是4字节整数，最多只有4294967296种输出，但输入的数据长度是不固定的，有无数种输入。所以，哈希算法是把一个无限的输入集合映射到一个有限的输出集合，那么碰撞是必然的。

   碰撞并不可怕，我们担心的不是碰撞，而是碰撞的概率，因为碰撞的概率的高低关系到哈希算法的安全性。一个安全的哈希算法必须满足：

   ```text
   碰撞概率低
   不能猜测输出
   ```

   常用的哈希算法有：

   | 算法         | 输出长度（位） | 输出长度（字节） |
   | ------------ | -------------- | ---------------- |
   | `MD5`        | `128bits`      | `16bytes`        |
   | `SHA-1`      | 160 bits       | 20 bytes         |
   | `SHA-256`    | 256 bits       | 32 bytes         |
   | `RipeMD-160` | 160 bits       | 20 bytes         |
   | `SHA-512`    | 512 bits       | 64 bytes         |

    根据碰撞概率，哈希算法的输出长度越长就越难产生碰撞，也就越安全。

   Java标准库提供了常用的哈希算法，并且有一套统一的接口，下面是`MD5`的例子：

   ```java
   import java.math.BigInteger;
   import java.security.MessageDigest;
   public class Main {
       public static void main(String[] args) throws Exception {
           // 创建一个MessageDigest实例:
           MessageDigest md = MessageDigest.getInstance("MD5");
           // 反复调用update输入数据:
           md.update("Hello".getBytes("UTF-8"));  //输入数据
           md.update("World".getBytes("UTF-8"));  //输入数据
           byte[] result = md.digest(); // 16 bytes: 68e109f0f40ca72a15e05cc22786f8e6
           System.out.println(new BigInteger(1, result).toString(16));
       }
   }
   ```

   说明：使用`MessageDigest`时，我们首先根据哈希算法获取一个`MessageDigest`实例，然后反复调用`update(byte[])`输入数据，当输入结束后，调用`digest()`方法获得`byte[]`数组表示的摘要，最后，把它转换为十六进制的字符串。

3. 哈希算法的用用途举例：
   1. 通常我们从网上下载文件到本地，下载页面一般会显示哈希。要判断下载到本地的文件是原始的还是被篡改后的，只需要计算一下本地文件的哈希值，然后与官网上的哈希值对比，如果相同，说明文件下载正确。
   2. 存储用户口令。比如使用`MD5`对用户输入的原始口令的进行加密。在用户输入原始口令后，系统计算用户输入的原始口令的`MD5`并与数据库存储的`MD5`对比，如果一致，说明口令正确。**使用哈希口令时，还要注意彩虹表攻击**，它是有一个预先计算好的常用口令，有了这个，就可能比较容易地得到原始口令。不过，这个问题可以通过加盐的方式，即对每个口令额外添加随机数。**加盐可以使彩虹表失效**

4. `SHA-1`:`SHA-1`也是一种哈希算法，它的输出是`160bits`,即`20`个字节。`SHA-1`是由美国国家安全局开发的，`SHA`算法其实是一个系列，包括`SHA-0`(已废弃)，`SHA-1`,`SHA-256`,`SHA-512`等。

   在Java中使用`SHA-1`,和`MD5`完全一样，只需要把算法名称改为`SHA-1`:

   ```java
   public class Main {
       public static void main(String[] args) throws Exception {
           // 创建一个MessageDigest实例:
           MessageDigest md = MessageDigest.getInstance("SHA-1");
           // 反复调用update输入数据:
           md.update("Hello".getBytes("UTF-8"));
           md.update("World".getBytes("UTF-8"));
           byte[] result = md.digest(); // 20 bytes: 6f44e49f848dd8ed27f73f59ab5bd4631b3f6b0d
           System.out.println(new BigInteger(1, result).toString(16));
       }
   }
   ```

   需要注意的是，**`MD5`因为输出长度较短，短时间内破解是可能的，目前已经不推荐使用**。