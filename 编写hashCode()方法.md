### 编写`hashCode()`方法 ###

1.  某个自定义的对象要作为`Map`的`key`时，既要正确重写`equals()`方法，又要正确重写`hashCode()`方法，正确重写`hashCode()`方法应该符合以下要求：

   ```text
   1. 如果两个对象相等，则两个对象的hashCode()必须相等；
   2. 如果两个对象不相等，则两个对象的hashCode()尽量不要相等。
   ```

   其中第一条是必须保证的，否则`HashMap`无法正常工作。第二条应该尽量满足，否则`Map`内部会有冲突，导致效率低下和出现意想不到的结果。

2. 编写`hashCode`方法：

   ```java
   public class Person {
       String firstName;
       String lastName;
       int age;
       public class Person {
       String firstName;
       String lastName;
       int age;
   
       @Override
       int hashCode() {
           int h = 0;
           h = 31 * h + firstName.hashCode();  
           h = 31 * h + lastName.hashCode();
           h = 31 * h + age;
           return h;
       }
   }
   }
   //之所以反复使用31*h,是为了尽量把不同的Person实例的hashCode()均匀分布到整个int范围。
   ```

   和实现`equal()`类似的问题，如果