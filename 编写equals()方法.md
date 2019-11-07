### 编写`equals()`方法 ###

1. `List`提供了`boolean contains(object 0)`方法来判断是否包含某个指定的元素，还提供了`int indexOf(object o)`方法返回某个元素的索引，如果元素不存在就返回`-1`。如：

   ```java
   public class Main {
       public static void main(String[] args) {
           List<String> list = List.of("A", "B", "C");
           System.out.println(list.contains("C")); // true
           System.out.println(list.contains("X")); // false
           System.out.println(list.indexOf("C")); // 2
           System.out.println(list.indexOf("X")); // -1
       }
   }
   ```

   `List`内部是通过`eqauls()`进行比较，而不是`==`。`contains()`方法可以这样实现:

   ```java
   public class ArrayList {
       Object[] elementData;
       public boolean contains(Object o) {
           for (int i = 0; i < size; i++) {
               if (o.equals(elementData[i])) {
                   return true;
               }
           }
           return false;
       }
   }
   ```

   因此，要正确使用`List`的`contains()`或者`indexOf()`等方法，或者作为`Map`的`Key`对象（此时还要重写`hashCode()`方法_,后面将学习)，就必须正确重写`equals()`方法。而`Java`标准库类都已经正确实现`equals()`方法。但是非标准类就要自己实现`equals()`方法了。

2. 如何正确编写`equals()`方法：

   编写`equals()`方法必须满足以下几个条件：

   ```text
   自反性（Reflexive）：对于非null的x来说，x.equals(x)必须返回true；
   对称性（Symmetric）：对于非null的x和y来说，如果x.equals(y)为true，则y.equals(x)也必须为true；
   传递性（Transitive）：对于非null的x、y和z来说，如果x.equals(y)为true，y.equals(z)也为true，那么x.equals(z)也必须为true；
   一致性（Consistent）：对于非null的x和y来说，只要x和y状态不变，则x.equals(y)总是一致地返回true或者false；
   对null的比较：即x.equals(null)永远返回false
   ```

   简单编写符合要求的`equals()`方法如下：

   ```java
   public class Person {
       public String name;
       public int age;
       public boolean equals(Object o) {
       if (o instanceof Person) {   //如果是同一实例，就比较，否则返回false
           Person p = (Person) o;
           boolean nameEquals = false;
           if (this.name == null && p.name == null) {
               nameEquals = true;
           }
           if (this.name != null) {
               nameEquals = this.name.equals(p.name);
           }
           return nameEquals && this.age == p.age; //对于引用字段用eqauls()比较，基本类型字段用==比较
       }
       return false;
   }
   }
   ```

   我们可以发现，如果`Person`有好几个引用类型的变量，上面的方法未免复杂，因此可以考虑使用`objects.equals()`静态方法，它省去了判断`null`的麻烦，两个`null`进行比较时，返回也是`true`：

   ```java
   public boolean equals(Object o) {
       if (o instanceof Person) {
           Person p = (Person) o;
           return Objects.equals(this.name, p.name) && this.age == p.age;
       }
       return false;
   }
   ```

   