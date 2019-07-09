# Java核心技术（卷一）

## Java 的基本程序设计概述

- 略

## Java程序设计环境

- 略

## Java的基本程序设计结构

### 数 据 类 型

| 类型   | 存储需求 | 取值范围                                                 |
| ------ | -------- | -------------------------------------------------------- |
| int    | 4 字节   | -2 147 483 648 - 2 147 483 647 (正好超过 20 亿)          |
| short  | 2 字节   | -32 768 - 32 767                                         |
| long   | 8字节    | -9 223 372 036 854 775 B08 - 9 223 372 036 854 775 807   |
| byte   | 1 字节   | -128 - 127                                               |
| float  | 4 字节   | 大约 ± 3.402 823 47E+38F (有效位数为 6 ~ 7 位）          |
| double | 8 宇节   | 大约 ± 1.797 693 134 862 315 70E+308 (有效位数为 15 位） |

- char 类型（特 殊 字 符 的 转 义 序 列）

  | 转义序列 | 名称   | Unicode 值 |
  | -------- | ------ | ---------- |
  | \b       | 退格   | \u0008     |
  | \t       | 制表   | \u0009     |
  | \n       | 换行   | \u000a     |
  | \r       | 回车   | \u000d     |
  | \\"      | 双引号 | \u0022     |
  | \\'      | 单引号 | \u0027     |
  | \\\      | 反斜杠 | \u005c     |

## 对象和类

- final实例域
  - 可以将实例域定义为 final。 构建对象时必须初始化这样的域。也就是说， 必须确保在每
    一个构造器执行之后，这个域的值被设置， 并且在后面的操作中， 不能够再对它进行修改。
  - final 修饰符大都应用于基本 （primitive ) 类型域，或不可变（immutable) 类的域（如果类
    中的每个方法都不会改变其对象， 这种类就是不可变的类。
  - 对于可变的类， 使用 final 修饰符可能会对读者造成混乱。

## 继承

- final 类和方法不可继承

## 接口、lambda 表达式与内部类

### lambda 表达式的语法

- lambda 表达式是一个可传递的代码块， 可以在以后执行一次或多次。

  ```java
  (String first, String second) ->
  {
  if (first.lengthO < second.lengthO) return -1;
  else if (first.lengthO > second.lengthO) return 1;
  else return 0;
  }
  ```

- 即使 lambda 表达式没有参数， 仍然要提供空括号，就像无参数方法一样：

  ```java
  () -> { for (int i = 100;i >= 0;i ) System.out.println(i); }
  ```

- 如果可以推导出一个 lambda 表达式的参数类型，则可以忽略其类型

  ```java
  Comparator<String> comp
  = (first, second) // Same as (String first, String second)
  -> first.lengthO - second.lengthO;
  ```

- 如果方法只有一 参数， 而且这个参数的类型可以推导得出，那么甚至还可以省略小括号：

  ```java
  ActionListener listener = event ->
  System.out.println("The time is " + new Date()");
  // Instead of (event) -> . . . or (ActionEvent event) -> . . .
  ```

- 无需指定 lambda 表达式的返回类型。

  ```java
  (String first, String second) -> first.lengthO - second.length
  ```

### 函 数 式 接 口

- Java 中 已 经 有 很 多 封 装 代 码 块 的 接 口。
- 对于只有一个抽象方法的接口， 需要这种接口的对象时， 就可以提供一个 lambda 表达
  式。这种接口称为函数式接口 （ functional interface )。

-  Comparator 就是只有一个方法的接口， 所以可以提供一个 lambda 表达式：

  ```java
  Arrays.sort (words,
  (first, second) -> first.lengthO - second.lengthO) ;
  //在底层， Arrays.sort 方法会接收实现了 Comparator<String> 的某个类的对象。 在这个对象上调用 compare 方法会执行这个 lambda 表达式的体。这些对象和类的管理完全取决于具体实现， 与使用传统的内联类相比，这样可能要高效得多。最好把 lambda 表达式看作是一个函数，而不是一个对象， 另外要接受 lambda 表达式可以传递到函数式接口。
  ```

- lambda 表达式可以转换为接口

  ```java
  Timer t = new Timer(1000, event ->
  {
  System.out.println("At the tone, the time is " + new DateO);
  Toolkit.getDefaultToolkit().beep();
  }；
  ```

- 在 Java 中，对 lambda 表达式所能做的也只是能转换为函数式接口

- Java API 在java.util.fimction 包中定义了很多非常通用的函数式接口。其中一个接口
  BiFunction 描述了参数类型为 T 和 U 而且返回类型为 R 的函数。可以把我们的字符
  串比较 lambda 表达式保存在这个类型的变量中：

  ```java
  BiFunction<String, String, Integer〉comp
  = (first, second) -> first.lengthO - second.length();
  ```

- ArrayList 类有一个 removelf 方法， 它的参数就是一个 Predicate。这个接口专门用来传递
  lambda 表达式。例如，下面的语句将从一个数组列表删除所有 null 值：

  ```java
  list.removelf(e -> e == null);
  ```

#### 方法引用

- 假设你希望只要出现一个定时器事件就打印这个事件对象。 当然，为此也可以调用:

  ```java
  Timer t = new Timer(1000, event -> System.out.println(event)):
  ```

- 但是，如果直接把 println 方法传递到 Timer 构造器就更好了。具体做法如下：

  ```java
  Timer t = new Timer(1000, Systei.out::println);
  //表达式 System.out::println 是一个方法引用（ method reference ), 它等价于 lambda 表达式x 一> System.out.println(x) 
  ```

- 要用：: 操作符分隔方法名与对象或类名。主要有 3 种情况：

  1. object::instanceMethod

  2. Class::staticMethod

  3. Class:.instanceMethod

- 在前 2 种情况中，方法引用等价于提供方法参数的 lambda 表达式。前面已经提到，
  System.out::println 等价于 x -> System.out.println(x) 类似地，Math::pow 等价于（x，y) ->
  Math.pow(x, y)
  对于第 3 种情况， 第 1 个参数会成为方法的目标。例如

  ```java
  String::compareToIgnoreCase
  // 等同于
  (x, y) -> x.compareToIgnoreCase(y) 
  ```

- 可以在方法引用中使用 this 参数。例如

  ```java
  this::equals
  //等同于
  x -> this.equals(x)。
  //使用super 也是合法的。下面的方法表达式:
  super::instanceMethod
  ```

#### 构造器引用

- 构造器引用与方法引用很类似，只不过方法名为 new。例如，Person::new 是 Person 构造
  器的一个引用。

- 假设你有一个字符串列表。可以把它转
  换为一个 Person 对象数组，为此要在各个字符串上调用构造器，调用如下：

  ```java
  ArrayList<String> names = . . .;
  Stream<Person> stream = names.stream().map(Person::new);
  List<Person> people = stream.col1ect(Col1ectors.toList());
  ```

#### 变量作用域

- lambda 表达式中捕获的变量必须实际上是最终变量 ( effectivelyfinal)。
- lambda 表达式中声明与一个局部变量同名的参数或局部变量是不合法的。
- 在一个 lambda 表达式中使用 this 关键字时， 是指创建这个 lambda 表达式的方法的 this
  参数。

#### 处理 lambda 表达式

- 使用 lambda 表达式的重点是延迟执行 deferred execution ) 毕竟， 如果想耍立即执行代
  码，完全可以直接执行， 而无需把它包装在一个丨ambda 表达式中。之所以希望以后再执行
  代码， 这有很多原因， 如：
  1. 在一个单独的线程中运行代码；
  2. 多次运行代码；
  3. 在算法的适当位置运行代码（例如， 排序中的比较操作；
  4. 发生某种情况时执行代码（如， 点击了一个按钮， 数据到达， 等等；
  5. 只在必要时才运行代码。

#### Comparator

-  Comparator 接口包含很多方便的静态方法来创建比较器。 这些方法可以用于 lambda 表
  达式或方法引用

  ```java
  //Person 对象数组，按名字对这些对象排序
  Arrays.sort(people, Comparator.comparing(Person::getName));
  //可以把比较器与 thenComparing 方法串起来
  Arrays.sort(people ,Comparator.comparing(Person::getlastName).
              thenConiparing(Pe rson::getFi rstName));
  //根据人名长度完成排序
  Arrays.sort(people, Comparator.companng(Person::getName,(s, t) -> Integer.compare(s.length(), t.length())))；
  //comparing 和 thenComparing 方法都有变体形式，可以避免 int、 long 或 double 值
  //的装箱。要完成前一个操作， 还有一种更容易的做法：
  Arrays.sort(people, Comparator.comparinglnt(p -> p.getName().length()));
  ```

### 内部类

- 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据。
- 内部类可以对同一个包中的其他类隐藏起来。
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名 （anonymous) 内部类比较
  便捷。

## 异常、断言和日志

- 略

## 泛型程序设计

### 定义简单泛型类

- Pair<T,U>

### 泛 型 方 法

- public static  <T>  T getMiddle(T... a)

### 类型变量的限定

- public static  <T extends Comparable> T min (T[ ] a) . . .

### 类 型 擦 除

- 无论何时定义一个泛型类型， 都自动提供了一个相应的原始类型 （ raw type )。原始类型
  的名字就是删去类型参数后的泛型类型名。擦除（ erased) 类型变M, 并替换为限定类型（无
  限定的变量用 Object。)

  ```java
  // Pair<T>> 的原始类型
  public class Pair
  {
  private Object first;
  private Object second;
  public Pair(Object first, Object second)
  {
  this,first = first;
  this.second = second;
  public Object getFirstO { return first; }
  public Object getSecondO { return second; }
  public void setFirst(Object newValue) { first = newValue; }
  public void setSecond(Object newValue) { second = newValue; }
  }
  ```

  

- 原始类型用第一个限定的类型变量来替换， 如果没有给定限定就用 Object 替换

  ```java
  public class Interval <T extends Comparable & Serializable〉implements Serializable
  {
  private T lower;
  private T upper;
  public Interval (T first, T second)
  {
  if (first.compareTo(second) <= 0) { lower = first; upper = second; }
  else { lower = second; upper = first; }
  }
  }
  // 原始类型
  public class Interval implements Serializable
  {
  private Comparable lower;
  private Coiparable upper;
  public Interval (Coiparable first, Coiparable second) { . . . }
  }
  
  ```

  

#### 翻译泛型表达式

- 当程序调用泛型方法时，如果擦除返回类型， 编译器插入强制类型转换。例如，下面这
  个语句序列

  ```java
  Pair<Employee> buddies = . .
  Employee buddy = buddies.getFirst()；
  ```

- 擦除 getFirst 的返回类型后将返回 Object 类型。编译器自动插人 Employee 的强制类型转换。
  也就是说，编译器把这个方法调用翻译为两条虚拟机指令：
  1. 对原始方法 Pair.getFirst 的调用。
  2. 将返回的 Object 类型强制转换为 Employee 类型。
- 当存取一个泛型域时也要插人强制类型转换。

#### 翻译泛型方法

```java
public static <T extends Comparable〉T nrin(T[] a)
//是一个完整的方法族，而擦除类型之后
public static Comparable min(Comparable!] a)
```

- Java 泛型转换的事实：
  1. 虚拟机中没有泛型，只有普通的类和方法。
  2. 所有的类型参数都用它们的限定类型替换。
  3. 桥方法被合成来保持多态。
  4. 为保持类型安全性，必要时插人强制类型转换。

#### 约束与局限性

- 不能用基本类型实例化类型参数

- 运行时类型查询只适用于原始类型

  ```java
  if (a instanceof Pair<String>) // Error
  if (a instanceof Pair<T>) // Error
  Pair<St「ing> p = (Pair<String>) a; // Warning-can only test that a is a Pair
  ```

- 不能创建参数化类型的数组

  ```java
  Pair<String>[] table = new Pair<String>[10]; // Error
  ```

- 不能实例化类型变置

  ```java
  public Pair() { first = new T(); second = new T(); } // Error
  ```

- 不能构造泛型数组

  ```java
  public static <T extends Comparable〉T[] minmax(T[] a) { T[] mm = new T[2]; . . . } // Error
  ```

- 泛型类的静态上下文中类型变量无效

  ```java
  public class Singleton<T>
  {
  private static T singlelnstance; // Error
  public static T getSinglelnstanceO // Error
  {
  if (singleinstance == null) construct new instance of T
  return singlelnstance;
  }
  }
  ```

- 不能抛出或捕获泛型类的实例

  ```java
  public class Problem<T> extends Exception { /* . . . */ } // Error can't extend Throwable
  public static <T extends Throwable〉void doWork(Class<T> t){
  	try{
  	do work
  	}
  	catch (T e) // Error can 't catch type variable
  	{
  	Logger,global.info(...)
  	}
  }	
  ```

  

- 可以消除对受查异常的检查

  - @SuppressWamings 注解

- 注意擦除后的冲突





## 并发

### 中断线程

- void interrupts()

   向线程发送中断请求。线程的中断状态将被设置为 true。如果目前该线程被一个 sleep调用阻塞，那么，InterruptedException 异常被抛出。

- static boolean interrupted() 

  测试当前线程（即正在执行这一命令的线程）是否被中断。注意，这是一个静态方法。这一调用会产生副作用—它将当前线程的中断状态重置为 false。

- boolean islnterrupted()

  测试线程是否被终止。不像静态的中断方法，这一调用不改变线程的中断状态。

### 线 程 属 性

#### 未捕获异常处理器

- 线程的 run方法不能抛出任何受查异常， 但是，非受査异常会导致线程终止。在这种情况下，线程就死亡了。

- 但是，不需要任何 catch子句来处理可以被传播的异常。相反，就在线程死亡之前， 异常被传递到一个用于未捕获异常的处理器。

- 该处理器必须属于一个实现 Thread.UncaughtExceptionHandler 接口的类。这个接口只有—个方法。

  ```java
  void uncaughtException(Thread t, Throwable e)
  ```

- 可以用 setUncaughtExceptionHandler 方法为任何线程安装一个处理器。也可以用 Thread类的静态方法 setDefaultUncaughtExceptionHandler 为所有线程安装一个默认的处理器。替换处理器可以使用日志 API 发送未捕获异常的报告到日志文件。

- 如果不安装默认的处理器， 默认的处理器为空。但是， 如果不为独立的线程安装处理器，此时的处理器就是该线程的 ThreadGroup 对象。

- ThreadGroup 类实现 Thread.UncaughtExceptionHandler 接口。它的 uncaughtException 方
  法做如下操作：

  1.  如果该线程组有父线程组， 那么父线程组的 uncaughtException 方法被调用。
  2. 否则， 如果 Thread.getDefaultExceptionHandler 方法返回一个非空的处理器， 则调用
     该处理器。
  3. 否则，如果 Throwable 是 ThreadDeath 的一个实例， 什么都不做
  4. 否则，线程的名字以及 Throwable 的栈轨迹被输出到 System.err 上。这是你在程序中肯定看到过许多次的栈轨迹。

## 同步

### 锁对象

- ReentrantLock

### 条件对象

- Condition
- 等待获得锁的线程和调用 await 方法的线程存在本质上的不同。一旦一个线程调用 await方法， 它进人该条件的等待集。当锁可用时，该线程不能马上解除阻塞。相反，它处于阻塞状态，直到另一个线程调用同一条件上的 signalAll 方法时为止

### Synchronized 关键字

- 每一个对象有一个内部锁， 并且该锁有一个内部条件。由锁来管理那些试图进入synchronized 方法的线程，由条件来管理那些调用 wait 的线程。
- 将静态方法声明为 synchronized 也是合法的。如果调用这种方法，该方法获得相关的类对象的内部锁。
- 内部锁和条件存在一些局限：
  1. 不能中断一个正在试图获得锁的线程。
  2. 试图获得锁时不能设定超时。
  3. 每个锁仅有单一的条件， 可能是不够的。
- 在代码中应该使用哪一种？ Lock 和 Condition 对象还是同步方法？下面是一些建议：
  - 最好既不使用 Lock/Condition 也不使用 synchronized 关键字。在许多情况下你可以使用 java.util.concurrent 包中的一种机制，它会为你处理所有的加锁。
  - 如果 synchronized 关键字适合你的程序， 那么请尽量使用它，这样可以减少编写的代码数量，减少出错的几率。
  - 如果特别需要 Lock/Condition 结构提供的独有特性时，才使用 Lock/Condition。

### 同步阻塞

```java
synchronized (obj) // this is the syntax for a synchronized block
{
	critical section
}

```

### volatile

- volatile 关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为 volatile ,那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

- Volatile 变量不能提供原子性。

  ```java
  //不能确保翻转域中的值。不能保证读取、 翻转和写入不被中断。
  public void flipDoneO { done = !done; } // not atomic
  ```

### final 变置

- 这个域声明为 final 时,可以安全地访问一个共享域。

### 原子性

- java.util.concurrent.atomic 包中有很多类使用了很高效的机器级指令（而不是使用锁） 来保证其他操作的原子性。
- 如果认为可能存在大量竞争， 只需要使用 LongAdder 而不是 AtomicLong。方法名稍有区别。调用 increment 让计数器自增，或者调用 add 来增加一个量， 或者调用 sum 来获取总和。

### 线程局部变量

- 在线程间共享变量的风险。有时可能要避免共享变量， 使用ThreadLocal 辅助类为各个线程提供各自的实例。

### 读 / 写锁



## 阻 塞 队 列

- 对于许多线程问题， 可以通过使用一个或多个队列以优雅且安全的方式将其形式化。生产者线程向队列插人元素， 消费者线程则取出它们。使用队列，可以安全地从一个线程向另一个线程传递数据
- 当试图向队列添加元素而队列已满， 或是想从队列移出元素而队列为空的时候， 阻塞队列（blocking queue ) 导致线程阻塞。在协调多个线程之间的合作时，阻塞队列是一个有用的工具。工作者线程可以周期性地将中间结果存储在阻塞队列中。其他的工作者线程移出中间结果并进一步加以修改。队列会自动地平衡负载。如果第一个线程集运行得比第二个慢， 第二个线程集在等待结果时会阻塞。如果第一个线程集运行得快， 它将等待第二个队列集赶上来

## 线程安全的集合

- 如果多线程要并发地修改一个数据结构， 例如散列表， 那么很容易会破坏这个数据结构。可以通过提供锁来保护共享数据结构， 但是选择线程安全的实现作为替代可能更容易。

### 高效的映射、集和队列

- java.util.concurrent 包提供了映射、 有序集和队列的高效实现：ConcurrentHashMap、ConcurrentSkipListMap > ConcurrentSkipListSet 和 ConcurrentLinkedQueue
- 这些集合使用复杂的算法，通过允许并发地访问数据结构的不同部分来使竞争极小化。
- 与大多数集合不同，size 方法不必在常量时间内操作。确定这样的集合当前的大小通常需要遍历。
- 集合返回弱一致性（ weakly consistent) 的迭代器。这意味着迭代器不一定能反映出它们被构造之后的所有的修改，但是，它们不会将同一个值返回两次，也不会拋出ConcurrentModificationException 异常。

### 映射条目的原子更新

### 对并发散列映射的批操作

### 并发集视图

### 写数组的拷贝

- CopyOnWriteArrayList 和 CopyOnWriteArraySet 是线程安全的集合，其中所有的修改线程对底层数组进行复制。如果在集合上进行迭代的线程数超过修改线程数， 这样的安排是很有用的。当构建一个迭代器的时候， 它包含一个对当前数组的引用。如果数组后来被修改了，迭代器仍然引用旧数组， 但是，集合的数组已经被替换了。因而，旧的迭代器拥有一致的（可能过时的）视图，访问它无须任何同步开销

### 并行数组算法

## Callable 与 Future

- Runnable 封装一个异步运行的任务，可以把它想象成为一个没有参数和返回值的异步方法。Callable 与 Runnable 类似，但是有返回值。Callable 接口是一个参数化的类型， 只有一个方法 call。

  ```java
  // 类型参数是返回值的类型。
  public interface Ca11able<V>
  {
  	V call() throws Exception;
  }
  
  
  ```

-  Future 保存异步计算的结果。可以启动一个计算，将 Future 对象交给某个线程，然后忘掉它。Future 对象的所有者在结果计算好之后就可以获得它

- FutureTask 包装器是一种非常便利的机制， 可将 Callable转换成 Future 和 Runnable, 它
  同时实现二者的接口

  ```java
  Callable<Integer> nyComputation = . . .;
  FutureTask<Integer> task = new FutureTask<Integer>(myConiputation);
  Thread t = new Thread(task); // it's a Runnable
  t.start()；
  Integer result = task.get()；// it's a Future
  ```

  

## 执 行 器

- 构建一个新的线程是有一定代价的， 因为涉及与操作系统的交互。如果程序中创建了大
  量的生命期很短的线程，应该使用线程池（ thread pool)。一个线程池中包含许多准备运行的
  空闲线程。将 Runnable 对象交给线程池， 就会有一个线程调用 run 方法。 当 run 方法退出
  时，线程不会死亡，而是在池中准备为下一个请求提供服务。

- 另一个使用线程池的理由是减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。如果有一个会创建许多线程的算法， 应该使用一个线程数“ 固定的” 线程池以限制并发线程的总数。

  | 方法                             | 描述                                                         |
  | -------------------------------- | ------------------------------------------------------------ |
  | newCachedThreadPool              | 必要时创建新线程；空闲线程会被保留 60 秒                     |
  | newFixedThreadPool               | 该池包含固定数量的线程；空闲线程会一直被保留                 |
  | newSingleThreadExecutor          | 只有一个线程的 “ 池”， 该线程顺序执行每一个提交的任务（类似于Swing 事件分配线程） |
  | newScheduledThreadPool           | 用于预定执行而构建的固定线程池， 替代 java.util.Timer        |
  | newSingleThreadScheduledExecutor | 用于预定执行而构建的单线程 “ 池”                             |

### 线程池

- newCachedThreadPoo丨方法构建了一个线程池， 对于每个任务， 如果有空闲线程可用，立即让它执行任务，如果没有可用的空闲线程， 则创建一个新线程。

- newFixedThreadPool 方法构建一个具有固定大小的线程池。 如果提交的任务数多于空闲的线程数， 那么把得不到服务的任务放置到队列中。当其他任务完成以后再运行它们。

- newSingleThreadExecutor 是一个退化了的大小为 1 的线程池： 由一个线程执行提交的任务，一个接着一个

- 这 3 个方法返回实现了ExecutorService 接口的 ThreadPoolExecutor 类的对象。可用下面的方法之一将一个 Runnable 对象或 Callable 对象提交给 ExecutorService：

  ```java
  // 可以使用这样一个对象来调用isDone、 cancel 或 isCancelled。但是， get 方法在完成的时候只是简单地返回 null
  Future submit(Runnable task)
  //提交一个 Runnable， 并且 Future 的 get 方法在完成的时候返回指定的 result 对象。
  Future submit(Runnable task, T result)
  //提交一个 Callable, 并且返回的 Future 对象将在计算结果准备好的时候得到它    
  Future submit(Callable task)
  ```

- 该池会在方便的时候尽早执行提交的任务。调用 submit 时，会得到一个 Future 对象， 可
  用来查询该任务的状态。

- 当用完一个线程池的时候， 调用 shutdown。该方法启动该池的关闭序列。被关闭的执行器不再接受新的任务。当所有任务都完成以后，线程池中的线程死亡。另一种方法是调用shutdownNow。该池取消尚未开始的所有任务并试图中断正在运行的线程。

- 在使用连接池时应该做的事：

  1. 调用 Executors 类中静态的方法 newCachedThreadPool 或 newFixedThreadPool。
  2. 调用 submit 提交 Runnable 或 Callable 对象。
  3. 如果想要取消一个任务， 或如果提交 Callable 对象， 那就要保存好返回的 Future对象
  4. 当不再提交任何任务时，调用 shutdown。

### 预定执行

- ScheduledExecutorService 接口具有为预定执行（ Scheduled Execution) 或 重 复 执 行 任务而设计的方法。它是一种允许使用线程池机制的 java.util.Timer 的泛化。Executors 类的newScheduledThreadPool 和 newSingleThreadScheduledExecutor 方法将返回实现了 ScheduledExecutorService 接口的对象。可以预定 Runnable 或 Callable 在初始的延迟之后只运行一次。也可以预定一个 Runnable对象周期性地运行。

### 控制任务组

- invokeAll 方法提交所有对象到一个 Callable 对象的集合中，并返回一个 Future 对象的列表，代表所有任务的解决方案。当计算结果可获得时， 可以像下面这样对结果进行处理：

  ```java
  List<Callab1e<T» tasks = . . .;
  List<Future<T» results = executor.invokeAll(tasks):
  for (Future<T> result : results)
  processFurther(result.get());
  ```

- 这个方法的缺点是如果第一个任务恰巧花去了很多时间，则可能不得不进行等待。将结果按可获得的顺序保存起来更有实际意义。可以用 ExecutorCompletionService 来进行排列.

- 用常规的方法获得一个执行器。然后， 构建一个 ExecutorCompletionService， 提交任务给完成服务（ completion service。) 该服务管理 Future 对象的阻塞队列，其中包含已经提交的任务的执行结果（当这些结果成为可用时。) 这样一来，相比前面的计算， 一个更有效的组织形式如下

  ```java
  ExecutorCompletionService<T> service = new ExecutorCompletionServiceo(executor):
  for (Callable<T> task : tasks) 
      service.submit(task);
  for (int i = 0; i < tasks.sizeO；i ++)
  	processFurther(servi ce.take0.get())；
  ```

### Fork-Join 框架

### 可完成 Future








