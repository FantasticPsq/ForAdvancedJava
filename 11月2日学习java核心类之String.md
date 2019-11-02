## java核心类 ##

### 字符串（String） ###

1. 字符串是通过一个`char[]`数组实现的，内部定义的`private final char[]`字段，以及没有任何修改`char[]`的方法实现决定了`String`字符串是不可变的。

2. 字符串的比较

   ```Java
   //需要特别注意
   public class main{
   	public  static void main(String[] args){
   		String s1 = "hello";
   		String s2 = "heloo";
   		System.out.println(s1==s2);
   		System.out.println(s1.equals(s2));
   	}
   }
   说明：上面的结果为两个true,  因为java编译器在编译时会自动把所有相同的字符串当作一个对象放入常量池，  
   自然s1和s2无论是值还是地址都是相同的。但是如果把定义s2的语句换为：String s2 = "Hello".toLowerCase();  
   那么s1==s2会返回false,而s1.equals(s2)会返回true。也就是说，"=="是比较来两个对象在堆中的引用是否相同，  
   而"equals()"只是比较字符串的内容，即在堆中的符序列。
   ```

3. 其他方法

   ```shell
   //是否包含字串
   "hello".contains("ll");//true,注意contains()方法的参数是CharSequence而不是String,因为CharSequence是String的父类。CharSequence是一个接口，它只包括length(),charAt(int index),subSequence(int start,int end)这几个API接口。
   //搜索字串的更多方法
   "Hello".indexOf("l");//2,返回第一个l的索引
   "Hello".lastIndexOf("l");//3，返回最后一个l的索引
   "Hello".startWith("He");//true
   "Hello".endWith("lo");//true
   //提取子串
   "Hello".substring(2);//"llo"
   "Hello".substring(2,4);//"ll"
   //去除首尾空白字符trim(),空白字符包括空格,\t,\r,\n:
   "  \tHello\n\r  ".trim();//"Hello"，注意trim()并没有改变字符串的内容，而是返回//了一个新的字符串
   String还提供了isEmpty()和isBlank()来判断字符串是否为空和空白字符串
   "".isEmpty();//true
   " ".isEmpty();//false
   " \n\t\r".isBlank();//true
   " Hello ".isBlank();//false
   //替换字串
   "hello".replace('l','w');//"hewwo",所有字符l被替换
   "hello".replace("ll","**");//"he**o",所有字串"ll"被替换
   //还有正则替换的replaceAll
   //分割字符串
   String[] arr = {"A","B","C"};
   String[] s = String.join("***",arr);//"A***B***C"
   //类型转换
   String.valueOf(123);//"123"
   int n1 = Integer.parseInt("123");//123
   char[] cs = "Hello".toCharArray();
   String s = new String(cs);
   ```

4. 字符编码，ASCII编码不包含汉字等字符，Unicode是统一通用的，但是比较”笨重“，所以有了变长编码UTF-8,它容错性强，传输过程中某些字符出现错误不会影响其他字符，所以传输数据是一般用UTF-8编码

   ```java
   byte[] b1 = "Hello".getBytes();//按照ISO8895-1编码，最好不用
   byte[] b2 = "Hello".getByteS("UTF-8");//推荐UTF-8编码
   //注意，转换编码后，就不再是char类型，而是byte类型，也可将byte类型的数据转换为String
   byte[] b = ...;
   String s1 = new String(b,"GBK");
   String s2 = new String(b,"UTF-8")
   ```

5.较老版本的JDK的String总是以char[]存储的，它的定义如下:

    ```java
public final class String {
    private final char[] value;
    private final int offset;
    private final int count;
}
    ```

​	较新版本的JDK的String则以Byte[],如果每个`byte`存储一个字符串，否则，每两个`byte`存储一个字符串，这样就可以节省内存，因为大量长度较短的String通常只包含ASCII字符：

```java
public final class String {
    private final byte[] value;
    private final byte coder; // 0 = LATIN1, 1 = UTF16
```

6. 字符类String总结

   ```shell
   Java字符串String是不可变对象；
   字符串操作不改变原字符串内容，而是返回新字符串；
   常用的字符串操作：提取子串、查找、替换、大小写转换等；
   Java使用Unicode编码表示String和char；
   转换编码就是将String和byte[]转换，需要指定编码；
   转换为byte[]时，始终优先考虑UTF-8编码。
   ```

      
