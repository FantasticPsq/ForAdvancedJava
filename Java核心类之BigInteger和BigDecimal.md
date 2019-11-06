### Java核心类之`BigInteger`和`BigDecimal`  ###

1. `BigInteger`:在`Java`中，由`CPU`原生提供的整型的最大范围是`64`位的`long`型整数，它可以直接通过`CPU`计算，速度非常快。但问题是，如果使用的整数范围超过了`long`怎么办？这个时候，就只能用软件来模拟一个大整数。`java.math.BigInteger`就是用来表示任意大小的整数，它内部用一个`int[]`数组来模拟一个非常大的整数。

   1. `BigInteger`类的基本结构是：

      ```java
      java.lang.Object
       |_java.lang.Number
        |_java.math.BigInteger
      ```

      `BigInteger`已经实现的接口是：`Serializable`,`Comparable`。

      `BigInteger`内部实现较为麻烦，和`long`相比，`BigInteger`不会有范围限制。但是做运算时只能使用实例方法，如`add(),pow(),multiply()`等。

    2. `BigInteger`也可以转换为`long`类型，如：

       ```java
       BigInteger i = new BigInteger("123456789000");
       System.out.println(i.longValue()); // 123456789000
       System.out.println(i.multiply(i).longValueExact()); // //java.lang.ArithmeticException: BigInteger out of long range
       //转换时最好使用longValueExact()方法，他会在超出long范围时抛出异常，其他类型也是一样，如int,最好使用intValueExact().
       ```

2. `BigDecimal`:`BigDecimal`可以表示任意大小且精度完全准确的浮点数。如：

   ```java
   
   BigDecimal bd = new BigDecimal("123.4567");
   System.out.println(bd.multiply(bd)); // 15241.55677489
   ```

   1. `BigDecimal`用`scale()`表示小数位数，例如：

      ```java
      BigDecimal d1 = new BigDecimal("123.45");
      BigDecimal d2 = new BigDecimal("123.4500");
      BigDecimal d3 = new BigDecimal("1234500");
      System.out.println(d1.scale()); // 2,两位小数
      System.out.println(d2.scale()); // 4
      System.out.println(d3.scale()); // 0
      ```

      `stripTrailingZeros()`可以将一个`BigDecimal`格式化为一个相等的，但去掉了末尾的0的`BigDecimal`,如:

      ```java
      BigDecimal d1 = new BigDecimal("123.4500");
      BigDecimal d2 = d1.stripTrailingZeros();
      System.out.println(d1.scale()); // 4
      System.out.println(d2.scale()); // 2,因为去掉了00
      
      BigDecimal d3 = new BigDecimal("1234500");
      BigDecimal d4 = d3.stripTrailingZeros();
      System.out.println(d3.scale()); // 0
      System.out.println(d4.scale()); // -2,表示这个数(d4)是整数，而且末尾有2个0
      ```

   2. `BigDecimal`做加减乘运算时，精度都不会丢失，但是做除法时，存在无法除尽的情况，这时就需要指定精度，如：

      ```java
      BigDecimal d1 = new BigDecimal("123.456");
      BigDecimal d2 = new BigDecimal("23.456789");
      BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入(RoundingMode.HALF_UP),
      ```

   3. 特别需要注意的是：在比较两个`BigDecimal`是否相等时，使用`equals()`不仅需要两个`BigDecimal`的值相等，还要求他们的`scale()`相等：

      ```java
      BigDecimal d1 = new BigDecimal("123.456");
      BigDecimal d2 = new BigDecimal("123.45600");
      System.out.println(d1.equals(d2)); // false,因为scale不同
      System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为2
      System.out.println(d1.compareTo(d2)); // 0
      ```

      所以，最好总是使用`compareTo`方法来比较两个`BigDecimal`的值，它根据两个值的大小分别返回负数，正数和`0`,分别对应于大于，小于，等于。最好不要使用`equals()`。

   4. `BigDecimal`的部分源码如下：

      ```java
      public class BigDecimal extends Number implements Comparable<BigDecimal> {
          private final BigInteger intVal;
          private final int scale;
      }
      ```

      



