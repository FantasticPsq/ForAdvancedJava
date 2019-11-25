### Java反射 ###

1. 什么是反射？反射就是`Reflection`,Java的反射是指程序在运行期间可以拿到一个对象的所有信息。

   正常情况下，如果我们要调用一个对象的方法，或者访问一个对象的字段，通常会传入对象实例,但是如果不能获得对象实例，只有`Object`实例，那就不好办了，所以，反射是为了解决在运行期间，对某个实例一无所知的情况下，如果调用其方法。

2. `Class`类：

   除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。

   仔细想想，我们可以得出这样的结论：`class`的本质是数据类型，无继承关系的数据类型无法赋值。

   而`class`是由`JVM`在执行过程中动态加载的。`JVM`在第一次读取到一种`class`类型时，将其加载进内存。

   没加载一种`class`,`JVM`就为其创建一个`class`类型实例，并关联起来。注意：这里的`Class`类型时一个名叫`Class`的`class`。其定义为:

   ```java
   public final class Class{
       private Class(){}
   }
   ```

   以`String`类型为例，当`JVM`加载`String`时，他首先读取`String.class`文件到内存，然后为`String`类创建一个`Class`实例并关联起来：

   ```java
   Class cls = new Class(String);
   ```

   由于`Class`类的构造方法时`private`,只有`JVM`能创建`Class`实例,我们自己的`Java`程序是无法创建`Class`实例的。

   所以，`JVM`持有的每个`Class`实例都指向一个数据类型(`class`或者`interface`),而且该实例中保存了该`class`的所有信息，包括类名，包名，父类，实现的接口，所有方法，字段等，因此，如果获取了某个`class`实例，我们就可以通过这个`Class`实例获取到该实例对应的`class`的所有信息。

   这种通过`Class`实例获取`class`信息·的方法称为反射。

3. 获取一个`class`的`Class`实例：

   方法一：直接通过一个`class`静态变量`class`获取：

   ```java
   Class cls = String.class;
   ```

   方法二：如果有一个实例变量，可以通过该实例提供的`getClass()`方法获取：

   ```java
   String s = "hello";
   Class cls  = s.getClass();
   ```

   方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

   ```java
   Class cls = Class.forName("java.lang.String");
   ```

   注意：数组（如`String[]`)也是一种`class`,而且不同于`String.class`,它的类名是`[Ljava.lang.String`。此外，`JVM`也为每一种基本类型如`int`也创建了`Class`,通过`int.class`访问。

4. 动态加载;

   