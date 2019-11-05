###  Java核心类之枚举 ###

1. 普通的类实现枚举，容易出现运行时错误，并且编译器并不能自动检查某个值是否在枚举的集合内等，为了解决这些问题，需要用`enum`来实现枚举类：

   枚举类是用关键字`enum`来定义的，`enum`定义枚举有以下好处：

   1. `enum`常量本身带有类型信息，编译器会自动检测出类型错误。
   2. 不可能引用到非枚举的值，因为编译无法通过。
   3. 不同类型的枚举不能互相比较或者赋值，因为类型不符。

2. `enum`的比较：

   需要注意的是，引用不能直接通过`==`进行比较，要始终使用`equals()`方法，但是__`enum`类型可以除外__

3. `enum`定义的类型就是`class`,只是它有以下几个特点：
   1. 定义的`enum`总是继承自`java.lang.Enum`,并且无法被继承。
   2. 只能定义出`enum`的实例，无法通过`new`操作创建`enum`实例；
   3. 定义的每个实例都是引用类型的唯一实例。
   4. 可以将`enum`类型用于`switch`语句。

4. `enum`的方法：

   1. `name()`方法返回常量名。判断枚举常量的名字的时候要始终使用`name()`方法，绝不能调用`toString`

   2. `ordinal()`方法返回定义常量的顺序，从0开始计数。注意，改变枚举常量定义的顺序就会导致`ordinal()`返回值发生变化。为了防止顺序发生变化，可以为每个枚举常量添加一个字段。如下代码：

      ```java
      public class Main {
          public static void main(String[] args) {
              Weekday day = Weekday.SUN;
              if (day.dayValue == 6 || day.dayValue == 0) {
                  System.out.println("Work at home!");
              } else {
                  System.out.println("Work at office!");
              }
          }
      }
      
      enum Weekday {
          MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);
      
          public final int dayValue;
      	//枚举也是类，可以有构造函数，只是外部无法通过new操作调用。
          private Weekday(int dayValue) {  
              this.dayValue = dayValue;
          }
      }
      //运行结果为"work at home"
      ```

      