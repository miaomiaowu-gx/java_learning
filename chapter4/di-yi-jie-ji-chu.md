## 第一节 基础

### 1.1 概念与常识

##### 【1】Java 语言特点?

* 面向对象（封装，继承，多态）；
* 平台无关性（ Java 虚拟机实现平台无关性）；
* 支持多线程；
* 支持网络编程并且很方便（ Java 语言诞生本身就是为简化网络编程设计的，因此 Java 语言不仅支持网络编程而且很方便）；
* 编译与解释并存；
* 可靠性、安全性。

##### 【2】为什么说 Java 语言“编译与解释并存”？

高级编程语言按照程序的执行方式分为【编译型和解释型】两种。简单来说，编译型语言是指编译器针对特定的操作系统将源代码**一次性**翻译成可被该平台执行的**机器码**；解释型语言是指解释器对源程序逐行解释成特定平台的**机器码并立即执行**。

Java 语言既具有编译型语言的特征，也具有解释型语言的特征，因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（*.class 文件），这种字节码必须由 Java 解释器来解释执行。因此，可以认为 Java 语言编译与解释并存。

##### 【3】Java 和 C++ 的区别?

1. 都是**面向对象**的语言，都支持**封装、继承和多态**
2. Java 不提供指针来直接访问内存，程序内存更加安全
3. Java 的类是**单继承**的，C++ 支持**多重继承**；虽然 Java 的类不可以多继承，但是**接口可以多继承**。
4. **Java 有自动内存管理垃圾回收机制(GC)**，不需要程序员手动释放无用内存
5. 在 C 语言中，字符串或字符数组最后都会有一个额外的字符'\0'来表示结束。但是，Java 语言中没有结束符这一概念。
  * [原因](https://blog.csdn.net/sszgg2006/article/details/49148189)：Java 里面一切都是对象，是对象的话，字符串肯定就有长度，既然有长度，编译器就可以确定要输出的字符个数，当然也就没有必要去浪费那 1 字节的空间用以标明字符串的结束了。比如，数组对象里有一个属性 length，就是数组的长度，String 类里面有方法 length() 可以确定字符串的长度，因此对于输出函数来说，有直接的大小可以判断字符串的边界，编译器就没必要再去浪费一个空间标识字符串的结束。
  
##### 【4】Java 程序从源代码到运行 

一般有下面 3 步：

![](/chapter4/img4/01-Java-run.png)

4.1 什么是字节码?采用字节码的好处是什么?

1) 在 Java 中，JVM 可以理解的代码就叫做字节码（即扩展名为 .class 的文件），它不面向任何特定的处理器，只面向虚拟机。
2) Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以 Java 程序运行时比较高效，而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。

4.2 需格外注意[.class->机器码]步骤

&emsp;&emsp;在这一步 JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)，所以后面引进了 JIT 编译器。
&emsp;&emsp;JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。机器码的运行效率高于 Java 解释器的。这也解释了为什么经常会说 Java 是编译与解释共存的语言。

##### 【5】区分 JVM JDK 和 JRE

* [JVM] Java 虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。
* [JDK] JDK 是 Java Development Kit 缩写，它是功能齐全的 Java SDK。它**拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）**。它能够创建和编译程序。
* [JRE] JRE 是 Java 运行时环境。它是运行已编译 Java 程序所需的所有内容的集合，**包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件**。但是，它不能用于创建新程序。

> 如果只是为了运行一下 Java 程序的话，只需要安装 JRE 就可以。如果需要进行一些 Java 编程方面的工作，需要安装 JDK。
> 但是，这不是绝对的。有时，即使不打算在计算机上进行任何 Java 开发，仍然需要安装 JDK。例如，如果要使用 JSP 部署 Web 应用程序，那么从技术上讲，只是在应用程序服务器中运行 Java 程序。那为什么需要 JDK 呢？因为应用程序服务器会将 JSP 转换为 Java servlet，并且需要使用 JDK 来编译 servlet。

##### 【6】什么是 Java 程序的主类?应用程序和小程序的主类有何不同?

1) 主类是 Java 程序执行的入口点。一个程序中可以有多个类，但只能有一个类是主类。
2) 在 Java 应用程序中，这个主类是指包含 main() 方法的类。而在 Java 小程序中，这个主类是一个**继承自系统类 JApplet 或 Applet 的子类**。应用程序的主类不一定要求是 public 类，但**小程序的主类要求必须是 public 类**。

##### 【7】Java 应用程序与小程序之间有哪些差别?

简单说应用程序是从主线程启动(也就是 main() 方法)。applet 小程序没有 main() 方法，主要是嵌在浏览器页面上运行(调用init()或者run()来启动)，嵌入浏览器这点跟 flash 的小游戏类似。

### 1.2 Java 基础语法

#### 1.2.1 语法

##### 【1】字符型常量和字符串常量的区别?

1. 形式上: 字符常量是[单引号]引起的一个字符; 字符串常量是[双引号]引起的0个或若干个字符。
2. 含义上: 字符常量相当于一个整型值( ASCII 值)，可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)。
3. 占内存大小：字符常量只占 2 个字节; 字符串常量占若干个字节 (注意： char 在 Java 中占两个字节)
  * 字符封装类 Character 有一个成员常量 Character.SIZE 值为 16，单位是 bits。该值除以 8(1byte=8bits) 后就可以得到 2 个字节。
  
![](/chapter4/img4/02-java-size.jpg)
  
##### 【2】Java 中的注释：单行注释、多行注释、文档注释。

代码的注释不是越详细越好。实际上好的代码本身就是注释，要尽量规范和美化自己的代码来减少不必要的注释。若编程语言足够有表达力，就不需要注释，尽量通过代码来阐述。

去掉下面复杂的注释，只需要创建一个与注释所言同一事物的函数即可

```java
// check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```

应替换为

```java
if (employee.isEligibleForFullBenefits())
```

##### 【3】Java 泛型及类型擦除？

🍓🍓🍓 一定要看 [详细解释](https://www.cnblogs.com/wuqinglong/p/9456193.html)

Java 泛型: `List<Object>`、`List<String>` 等
类型擦除：Java的泛型是伪泛型，这是因为Java在编译期间，所有的泛型信息都会被擦掉。Java的泛型基本上都是在编译器这个层次上实现的，在生成的字节码中是不包含泛型中的类型信息的，使用泛型的时候加上类型参数，在编译器编译的时候会去掉，这个过程成为类型擦除。如在代码中定义 List<Object> 和 List<String> 等类型，在编译后都会变成 List，JVM 看到的只是List，而由泛型附加的类型信息对 JVM 是看不到的。类型擦除也是 Java 的泛型与 C++ 模板机制实现方式之间的重要区别。



泛型一般有三种使用方式:泛型类、泛型接口、泛型方法。

1) 泛型类

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{ 
    private T key;
    public Generic(T key) { 
        this.key = key;
    }
    public T getKey(){ 
        return key;
    }
}
//实例化泛型类
Generic<Integer> genericInteger = new Generic<Integer>(123456);
```

2) 泛型接口

```java
public interface Generator<T> {
    public T method();
}

//实现泛型接口，不指定类型：
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}

//实现泛型接口，指定类型：
class GeneratorImpl<T> implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```

3) 泛型方法

```java
public static < E > void printArray( E[] inputArray )
{         
     for ( E element : inputArray ){        
        System.out.printf( "%s ", element );
     }
     System.out.println();
}

//使用
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray );
printArray( stringArray );
```

##### 【4】常用的通配符为： T，E，K，V，？

本质上这些个都是通配符，没什么区别，只不过是编码时的一种约定俗成的东西。通常情况下，
* `？` 表示不确定的 java 类型
* `T (type)` 表示具体的一个 java 类型
* `K V (key value)` 分别代表 java 键值中的 Key Value
* `E (element)` 代表 Element

[通配符详解](https://juejin.im/post/6844903917835419661)

##### 【5】== 和 equals 的区别

* `==` : 作用是[判断两个对象的地址是不是相等]。即**判断两个对象是不是同一个对象**。(基本数据类型==比较的是值，**引用数据类型**==比较的是**内存地址**)
  * 因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。
* `equals()` : 作用也是[通过对象地址判断两个对象是否相等]，它不能用于比较基本数据类型的变量。equals()方法存在于 Object 类中，而 Object 类是所有类的直接或间接父类。
  * 情况 1：**类没有覆盖 equals()方法**。则通过 equals()比较该类的两个对象时，等价于通过“==”比较这两个对象。使用的默认是 Object类equals()方法。
  * 情况 2：**类覆盖了 equals()方法**。一般，都覆盖 equals()方法来两个对象的内容相等；若它们的内容相等，则返回 true(即，认为这两个对象相等)。


> 使用默认的“equals()”方法，等价于“==”方法。因此，通常会重写equals()方法：若两个对象的内容相等，则equals()方法返回true；否则，返回fasle。

````java
//Object 类 equals()方法：
//🍓Object 的 equals 方法是比较的对象的内存地址
public boolean equals(Object obj) {
     return (this == obj);
}
```

举个例子：

```java
public class test1 {
    public static void main(String[] args) {
        String a = new String("ab"); // a 为一个引用
        String b = new String("ab"); // b为另一个引用,对象的内容一样
        String aa = "ab"; // 放在常量池中
        String bb = "ab"; // 从常量池中查找
        if (aa == bb) // true
            System.out.println("aa==bb");
        if (a == b) // false，非同一对象
            System.out.println("a==b");
        if (a.equals(b)) // true
            System.out.println("aEQb");
        if (42 == 42.0) { // true
            System.out.println("true");
        }
    }
}
```

说明：
* String 中的 equals 方法是被重写过的，**因为 Object 的 equals 方法是比较的对象的内存地址，而 String 的 equals 方法比较的是对象的值**。
* 当创建 String 类型的对象时，虚拟机会**在常量池中查找**有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。

String 类 equals() 方法：

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

##### 【6】hashCode()与 equals()

面试官可能会问你：“你重写过 hashcode 和 equals 么，为什么重写 equals 时必须重写 hashCode 方法？”

1) hashCode()介绍

1. hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。
2. hashCode() 定义在 JDK 的 Object 类中，这就意味着 Java 中的任何类都包含有 hashCode() 函数。
  * 虽然，每个 Java 类都包含 hashCode() 函数。但是，仅仅当创建并某个“类的散列表”(Java集合中本质是散列表的类，如HashMap，Hashtable，HashSet)时，该类的 hashCode() 才有用(作用是：确定该类的每一个对象在散列表中的位置；其它情况下(例如，创建类的单个对象，或者创建类的对象数组等等)，类的 hashCode() 没有作用。
  * 即，hashCode() 在散列表中才有用，在其它情况下没用。
3. 需要注意的是：Object 的 hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法通常用来将对象的 内存地址 转换为整数之后返回。
```java
public native int hashCode();
```

2) 



3) 





#### 1.2.2 基本数据类型






#### 1.2.3 方法（函数）


### 1.5 Java 面向对象



### 1.6 Java 核心技术










