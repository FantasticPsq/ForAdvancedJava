### Java集合 ###

1. `Collection`:

   1. `Collection`是`Java`自带的`java.util`包提供的集合类，它是所有其他集合类的根接口。
   2. `Java`访问集合总是通过统一的方式——迭代器(`Iterator`)来实现，它最明显的好处是无需知道集合内元素是按什么方式存储的。

2. `List`:

   1. `List`是最基础的一种集合：它是一种有序链表。实现`List`方法有数组(`ArrayList`)实现和“链表”(`LinkedList`)实现。`List<E>`的常用接口如下:

      ```shell
      在末尾添加一个元素：void add(E e)
      在指定索引添加一个元素：void add(int index, E e) //在LinkedList中效率高
      删除指定索引的元素：int remove(int index)
      删除某个元素：int remove(Object e) //remove()方法在LinkedList中效率很高
      获取指定索引的元素：E get(int index) //get()方法在ArrayList中效率很高
      获取链表大小（包含元素的个数）：int size()
      ```

   2.  `List`中允许添加重复元素和`null`。

   3. 除了使用`ArrayList`和`LinkedList`,我们还可以通过`List`接口提供的`of()`方法快速创建元素：

      ```java
      List<Integer> list = List.of(1, 2, 5);
      ```

      但是`List.of()`方法不接受`null`值，如果传入`null`会抛出`NullPointerException`异常。

   4. 遍历`List`,可以用`for`循环配合`get(int)`方法遍历:

      ```java
      import java.util.List;
      public class Main {
          public static void main(String[] args) {
              List<String> list = List.of("apple", "pear", "banana");
              for (int i=0; i<list.size(); i++) {
                  String s = list.get(i);
                  System.out.println(s);
              }
          }
      }
      ```

      但是这种方法并不推荐，一是因为代码复杂，而是因为`get(int)`只有`ArrayList`的实现是高效的，对于`LinkedList`来说，索引越大，访问速度越慢。

      所以，我们要始终使用__迭代器`Iterator`__来访问`List`.`Iterator`本身是一个对象，但它是由`List`的实例调用`iterator`方法时创建的。

      `Iterator`有两个方法:`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素：

      ```java
      import java.util.Iterator;
      import java.util.List;
      public class Main {
          public static void main(String[] args) {
              List<String> list = List.of("apple", "pear", "banana");
              for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
                  String s = it.next();
                  System.out.println(s);
              }
          }
      }
      ```

      上面的代码有点复杂，而`Java`的`for each`为我们提供了方便：

      ```java
      public class Main {
          public static void main(String[] args) {
              List<String> list = List.of("apple", "pear", "banana");
              for (String s : list) {
                  System.out.println(s);
              }
          }
      }
      ```

      只要实现了`Iterator`接口的集合类都可以直接使用`for each`循环来遍历,`Java`编译器会自动把`for each`循环变成`Iterator`的调用。

   5. `List`和`Array`转换：

      1. 把`List`变为`Array`有三种方法，`toArray()`直接返回一个`object[]`数组。这种方法会丢失类型信息，一般不用。第二种方式是给`toArray(T[])`传入一个相同类型的`Array`,`List`自动把元素复制到传入的`Array`中，如：

         ```java
         public class Main {
             public static void main(String[] args) {
                 List<Integer> list = List.of(12, 34, 56);
                 Integer[] array = list.toArray(new Integer[3]);
                 for (Integer n : array) {
                     System.out.println(n);
                 }
             }
         }
         ```

         `toArray(T[])`这个方法的泛型参数<T>并不是`List`接口定义的泛型参数<E>,所以，我们可以传入其它类型的数组，如：

         ```java
         public class Main {
             public static void main(String[] args) {
                 List<Integer> list = List.of(12, 34, 56);
                 Number[] array = list.toArray(new Number[3]);
                 for (Number n : array) {
                     System.out.println(n);
                 }
             }
         }
         ```

         但是，如果传入类型不匹配的数组，会抛出`ArrayStoreException`。传入数组时最好传入大小“恰好匹配的数组”：

         ```java
         Integer[] array = list.toArray(new Integer[list.size()])
         ```

         最后一种写法是通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法：

         ```java
         Integer[] array = list.toArray(Integer[]::new);
         ```

   2. 把数组变为`List`方法就是`List.of()`。对于`JDK 11`之前的版本，可以使用`Arrays.asList(T...)`方法。需要注意的是，返回`List`不一定就是`ArrayLIst`或者`LinkedList`,因为`List`只是一个接口，如果我们调用`List.of()`,它返回的是一个只读的`List`，__对只读的`List`调用`add(),remove()`方法会抛出`UnsupportedOperationException`。

   

   

   