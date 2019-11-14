### `Hmac`算法 ###

1. `Hmac`算法就是一种基于密钥的消息认证码算法，它的全称是`Hash-based Message Authentication Code`,是一种更安全的消息摘要算法。`Hmac`算法总是和某种哈希算法配合起来使用，例如，我们使用`MD5`算法，对应的就是`HmacMD5`算法，它相当于"加盐"的`MD5`:

   ```text
   HmacMD5 约= md5(secure_random_key,input)
   ```

   可以看出，`HmacMD5`可以看作带有一个安全的`key`的`MD5`。使用`HmacMD5`而不是`MD5`加`salt`，有如下好处：

   ```text
   1.HmacMD5使用的key长度是64字节，更安全;
   2.Hmac是标准算法，同样适用于SHA-1等其他哈希算法;
   3.Hmac输出和原有的哈希算法长度一致。
   ```

   可见，`Hmac`本质上就是把`key`摘要的算法，验证此哈希表时，除了原始输入数据，还要提供`key`。

   为了保证安全，我们不会自己制定`key`，而是通过`Java`标准库的`KeyGenerator`生成一个安全的随机数`key`。如下面的例子：

   ```java
   public class Main {
       public static void main(String[] args) throws Exception {
           KeyGenerator keyGen = KeyGenerator.getInstance("HmacMD5");
           SecretKey key = keyGen.generateKey();
           // 打印随机生成的key:
           byte[] skey = key.getEncoded();
           System.out.println(new BigInteger(1, skey).toString(16));
           Mac mac = Mac.getInstance("HmacMD5");
           mac.init(key);
           mac.update("HelloWorld".getBytes("UTF-8"));
           byte[] result = mac.doFinal();
           System.out.println(new BigInteger(1, result).toString(16));
       }
   }
   ```

   可总结`Hmac`的使用步骤：

   ```text
   通过名称HmacMD5获取KeyGenerator实例；
   通过KeyGenerator创建一个SecretKey实例；
   通过名称HmacMD5获取Mac实例；
   用SecretKey初始化Mac实例；
   对Mac实例反复调用update(byte[])输入数据；
   调用Mac实例的doFinal()获取最终的哈希值
   ```

   恢复`SecretKey`的语句是`new SecretKeySpec(hkey,"HmacMD5")`。