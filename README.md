# Java核心技术（卷一）

## Java 的基本程序设计结构

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

  

- final 类和方法不可继承

### lambda 表达式

#### lambda 表达式的语法

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

#### 函 数 式 接 口

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



#### 内部类

- 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据。
- 内部类可以对同一个包中的其他类隐藏起来。
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名 （anonymous) 内部类比较
  便捷。

