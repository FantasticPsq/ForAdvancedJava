### Java核心类之String ###

1. `StringBuilder`

   1. `StringBuilder`是一个可变对象，可以预分配缓冲区，往`StringBuilder`中新增字符时，不会创建新的临时对象。而`String`直接拼接字符串时会创建新的字符串对象，丢弃旧的字符串对象，这不仅浪费内存而且会影响`GC`效率。

   2. `StringBuilder`还可以进行链式操作。从`StingBuilder`的源码可以看出，进行链式操作的关键是，定义的`append`方法返回的是`this`,这样就可以不断调用自身的其他方法。仿照此，我们也可以设计支持链式操作的类，如：

   ```java
   public class Main {
       public static void main(String[] args) {
           Adder adder = new Adder();
           adder.add(3)
                .add(5)
                .inc()
                .add(10);
           System.out.println(adder.value());
       }
   }
   
   class Adder {
       private int sum = 0;
   
       public Adder add(int n) {
           sum += n;
           return this;
       }
   
       public Adder inc() {
           sum ++;
           return this;
       }
   
       public int value() {
           return sum;
       }
   }
   ```

   3. 注意，对于普通的字符串`+`操作，并不需要我们将其改为`StringBuilder`,因为`java`编译器编译时就自动把多个连续的`+`操作编码为`StringConcatFactory`的操作，在运行期间，`StringConcatFactory`会自动把字符串连接操作优化为数组复制或者`StringBuilder`操作。

2. `StringJoiner`

   1. 要高效拼接字符串应该使用`StringBuilder`，然而由于用分隔符拼接数组的需求很常见，所以`Java`还提供了一个`StringJoiner`来干这个事，示例如下:

   ```java
   import java.util.StringJoiner;
   public class Main {
       public static void main(String[] args) {
           String[] names = {"Bob", "Alice", "Grace"};
           var sj = new StringJoiner(", ", "Hello ", "!");
           for (String name : names) {
               sj.add(name);
           }
           System.out.println(sj.toString());
       }
   }
   //结果为 Hello Bob, Alice, Grace!
   ```

   2. 那么`StringJoiner`内部究竟是怎么拼接字符串的呢？查看源码可以发现`StringJoiner`实际上使用了`StringBuilder`，所以拼接效率几乎和`StringBuilder`几乎一样。

   3. `String`还提供了一个·静态方法`join()`,这个方法在内部使用了`StringJoiner`来拼接字符串，当不需要指定开头和结尾的时候，用`String.join()`更加方便。

      ```java
      String[] names = {"Bob", "Alice", "Grace"};
      var s = String.join(", ", names);
      ```