### Java集合之Map总体认识 ###

1. `Map<K,V>`是一种键值映射表。使用`get()`方法时，如果`key`不存在，返回的是`null`。

   `Map`中不存在重复的`key`。

2. 遍历`Map`：

   1. 要遍历`key`可以使用`for each`循环遍历`Map`实例的`keySet()`方法返回地集合，它不包含重复的`key`：

   ```java
   public class Main {
       public static void main(String[] args) {
           Map<String, Integer> map = new HashMap<>();
           map.put("apple", 123);
           map.put("pear", 456);
           map.put("banana", 789);
           for (String key : map.keySet()) {
               Integer value = map.get(key);
               System.out.println(key + " = " + value);
           }
       }
   }
   ```

   2. 同时遍历`key`和`value`可以使用`for each`循环遍历`Map`对象的`entrySet()`集合，它包含每一个`key-value`映射：

      ```java
      public class Main {
          public static void main(String[] args) {
              Map<String, Integer> map = new HashMap<>();
              map.put("apple", 123);
              map.put("pear", 456);
              map.put("banana", 789);
              for (Map.Entry<String, Integer> entry : map.entrySet()) {
                  String key = entry.getKey();
                  Integer value = entry.getValue();
                  System.out.println(key + " = " + value);
              }
          }
      }
      ```

   3. `Map`与`List`不同的是，`Map`存储的是`key-value`键值映射，它不保证顺序，遍历时候的顺序和`put`的顺序不一定相同，使用`Map`时任何依赖顺序的逻辑都是不可靠的。所以，__遍历`Map`时，不可假设输出`key`是有序的！__