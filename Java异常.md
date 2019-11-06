### Java异常 ###

1. `Java`异常是一种`class`它的继承关系如下：

   ```java
                        ┌───────────┐
                        │  Object   │
                        └───────────┘
                              ▲
                              │
                        ┌───────────┐
                        │ Throwable │
                        └───────────┘
                              ▲
                    ┌─────────┴─────────┐
                    │                   │
              ┌───────────┐       ┌───────────┐
              │   Error   │       │ Exception │
              └───────────┘       └───────────┘
                    ▲                   ▲
            ┌───────┘              ┌────┴──────────┐
            │                      │               │
   ┌─────────────────┐    ┌─────────────────┐┌───────────┐
   │OutOfMemoryError │... │RuntimeException ││IOException│...
   └─────────────────┘    └─────────────────┘└───────────┘
                                   ▲
                       ┌───────────┴─────────────┐
                       │                         │
            ┌─────────────────────┐ ┌─────────────────────────┐
            │NullPointerException │ │IllegalArgumentException │...
   ```

   由此可知：`Throwable`是异常体系的根，它继承自`object`。`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，如：

   ```shell
   OutOfMemoryError：内存耗尽
   NoClassDefFoundError：无法加载某个Class
   StackOverflowError：栈溢出
   ```

   `Exception`是运行时错误，它可以被捕获平且处理。

   `Java`规定：

   1. 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类。这种类型的异常被称为`Checked Exception`。
   2.  不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。

2. 异常捕获：
   1. 所有`Checked Exception`都一定要被捕获，所有未被捕获的异常，在`main()`函数中必须被捕获，不会出现漏写`try`代码的情况。这是由编译器保证的，`main()`方法也是最后捕获`Exception`的机会。
   2. 如果是测试，又不想写任何`try`代码，可以将主函数定义为`throws Exception`。
   3.  存在多个`catch`时，`catch`的顺序非常重要:__子类必须写在前面__。
   4. 如果方法声明了可能抛出的异常，`try`语句可以没有`catch`，只使用`try ... finally`结构。`finally`是保证某些代码是必须执行的。

3. 抛出异常：

   1. 当某个方法抛出异常的时候，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止。
   2. 通过`printStackTrace()`可以打印出方法的调用栈。这对调试非常重要。
   3. 如果`catch`到异常后，又在`catch`中抛出新的异常，就相当于把原先的异常类型转换了。要获取原始异常可以使用`Throwable.getCause()`方法，如果返回`null`,说明已经是“根异常了”。而且在`catch`中抛出异常不会影响`fianlly`的执行。

   4. 自定义异常：

      1. `Java`标准库定义的常用异常包括：

         ```shell
         Exception
         │
         ├─ RuntimeException
         │  │
         │  ├─ NullPointerException
         │  │
         │  ├─ IndexOutOfBoundsException
         │  │
         │  ├─ SecurityException
         │  │
         │  └─ IllegalArgumentException
         │     │
         │     └─ NumberFormatException
         │
         ├─ IOException
         │  │
         │  ├─ UnsupportedCharsetException
         │  │
         │  ├─ FileNotFoundException
         │  │
         │  └─ SocketException
         │
         ├─ ParseException
         │
         ├─ GeneralSecurityException
         │
         ├─ SQLException
         │
         └─ TimeoutException
         ```

         在大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。可以自定义一个`BaseException`作为“根异常”，然后再在此基础上派生出新的异常类型。而`BaseException`需要一个合适的`Exception`派生，通常建议从`Exception`派生：

         ```java
         public class BaseException extends RuntimeException {
         }
         ```

      5. 使用断言，`JDK Logging`,`Commons Logging`,`Log4j`以及`SLF4J`，`Logback`将在后面学习。

   