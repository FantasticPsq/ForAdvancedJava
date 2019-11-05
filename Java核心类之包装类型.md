### Java核心类之包装类型 ###

1. 如何把一个基本类型变成对象？比如要把`int`类型变成一个引用类型，我们可以定义一个`Integer`类，它只包含实例字段`int`，这样，`Integer`类就可以视为`int`的包装类`(Wrapper Class)`:

   ```java
   public class Integer {
       private int value;
   
       public Integer(int value) {
           this.value = value;
       }
   
       public int intValue() {
           return this.value;
       }
   }
   Integer n = null;
   Integer n1 = new Integer(99);
   int n2 = n1.intValue(); //n2=99
   ```

   这样，我们就可以把`int`和`Integer`相互转化了。由此可见，包装类非常有用。（但是，不推荐通过new 操作创建Integer实例，会有编译警告，因为如果`new Integer(n)`中的`n`不是`int`类型程序会出错，所以最好使用`valueOf`静态方法来创建Integer对象。

2. 自动装箱(`Auto Boxing`)

   `Integer n  = 100; int x = n`这种直接把`int`变为`Integer`的赋值写法，叫做自动装箱，反过来，把`Integer`变为`int`的赋值写法，叫做自动拆箱(`Auto Unboxing`)。自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码，但是，并不推荐这两种做法，因为装箱和拆箱会影响代码的执行效率，因为编译后的`class`代码是严格区分基本类型和引用类型的，并且，自动拆箱可能会报`NullPointerException`，如：

   ```java
   public class Main {
       public static void main(String[] args) {
           Integer n = null;
           int i = n;
       }
   }
   ```

  3. 不变类

     1. 所有的包装类型都是不变类，`Integer`的核心代码如下：

     ```java
     public final class Integer{
         private final int value;
     }
     ```

     这说明，`Integer`对象一旦被创建就是不可变的。注意，绝不能用`==`比较两个`Integer`实例，必须使用`equals()`比较：

     ```java
     public class Main {
         public static void main(String[] args) {
             Integer x = 127;
             Integer y = 127;
             Integer m = 99999;
             Integer n = 99999;
             System.out.println("x == y: " + (x==y)); // true
             System.out.println("m == n: " + (m==n)); // false
             System.out.println("x.equals(y): " + x.equals(y)); // true
             System.out.println("m.equals(n): " + m.equals(n)); // true
         }
     }
     ```

     是否会发现结果很奇怪，两个较小的两个相同的`Integer`用`==`比较时，结果为true,而两个较大的相同的`Integer`进行比较时，结果却是`false`。究其原因是，`Ingeger`是不变类，编译器把`Integer x = 27`自动变为`Integer x = Integer.valueOf(127)`，为了节省内存，`Integer.valueOf(127)`对于较小的数，始终返回相同的实例，此时，`==`恰好为`True`,但是我们绝不能因为Java标准库的`Integer`内部有缓存优化就用`==`比较，必须使用`equals()`比较两个`Integer`

     2. 处理无符号数：

        ```java
        public class Main {
            public static void main(String[] args) {
                byte x = -1;
                byte y = 127;
                System.out.println(Byte.toUnsignedInt(x)); // 255
                System.out.println(Byte.toUnsignedInt(y)); // 127
            }
        }
        ```

        byte的-1是二进制的`11111111`,以无符号数转换为`int`以后就是255。类似的，可以把一个`short`按`unsigned`转换为`int`，把一个`int`按`unsigned`转换为`long`