**note：这个文档很久之前写的，可以参考下，建议还是看官网**

# 简介

Scala是一种多范式的编程语言，其设计的初衷是要集成面向对象编程和函数式编程的各种特性。Scala运行于Java平台（Java虚拟机），并兼容现有的Java程序。

# 入门

## 安装

### scala安装

window10：需要安装jdk，scala是依赖jdk的，scala直接官网下载安装即可，不需要配置环境变量

### 集成插件

![image-20220321135252322](scala.assets\image-20220321135252322.png)

### 项目集成scala

![image-20220321135452750](scala.assets\image-20220321135452750.png)

### 自动添加描述

#### class

```scala
// 创建时间有的也没有 仅仅就是class的描述信息，看个人公司情况吧，描述信息最好创建时间有一个空行，我忘记了

/**
* 描述信息
*
* @create: ${YEAR}-${MONTH}-${DAY} ${HOUR}:${MINUTE}
*/
```

![image-20220321180131613](scala.assets\image-20220321180131613.png)

#### method

```scala
// 一会要写入模板的内容
**
 * 描述信息
 * $param$
 * @return $return$
 */


// groovyScript在网上找的，一般会java scala可以简单的读一下，就是for循环然后字符串拼接

groovyScript("def result='';def stop=false; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); if (params.size()==1 && (params[0]==null || params[0]=='null' || params[0]=='')) { stop=true; }; if(!stop) { for(i=0; i < params.size(); i++) {result +=((i==0) ? '\\r\\n' : '') + ((i < params.size() - 1) ? ' * @param ' + params[i] + '\\r\\n' : ' * @param ' + params[i] + '')}; }; return result;", methodParameters())

// 下面这个脚本当参数为空的时候也会生成@param不太好，还要手动搞
groovyScript("def result='';def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+='* @param ' + params[i] + ((i < params.size() - 1) ? '\\n ' : '')};return result", methodParameters())
```

![image-20220321190751925](scala.assets\image-20220321190751925.png)

![image-20220321190832088](scala.assets\image-20220321190832088.png)

![image-20220321191221768](scala.assets\image-20220321191221768.png)



## 注释

```scala
// 单行注释

/* 多行注释
多行注释 */

// 文档注释，一般用于方法
/**
* @deprecated xxx 
* @example testing coding 
* @param args 
*/
```

## 输入与输出

### 输入

```java
 public static void  input(){
     Scanner scanner = new Scanner(System.in);
     System.out.println("请输入名字");
     String next = scanner.next();
     System.out.println(next);
 }
```

```scala
// scala 对象下面都会讲解 知道有这个方法就好了
object ScalaObjectTest extends App {

  println("请输入名字")
  val str = StdIn.readLine()
  println(str)

}
```

### 输出

```scala
// 一般采用第三种
println("name=" + name + " age=" + age + " url=" + url) 
printf("name=%s, age=%d, url=%s \n", name, age, url) 
println(s"name=$name, age=$age, url=$url")
```

## hello world

```xml
<properties>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
    <scala.binary.version>2.12</scala.binary.version>
    <delta.version>0.7.0</delta.version>
    <scala.test.version>3.2.11</scala.test.version>
    <spark.version>3.0.3</spark.version>
</properties>

<dependencies>
    <!-- 这个是操作数据湖的如果不用的话可以注释 -->
    <dependency>
        <groupId>io.delta</groupId>
        <artifactId>delta-core_${scala.binary.version}</artifactId>
        <version>${delta.version}</version>
    </dependency>
	<!-- 这个是scala专用的 test 相当于java的junit -->
    <dependency>
        <groupId>org.scalatest</groupId>
        <artifactId>scalatest_${scala.binary.version}</artifactId>
        <version>${scala.test.version}</version>
    </dependency>
	<!-- 这个是spark -->
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_${scala.binary.version}</artifactId>
        <version>${spark.version}</version>
    </dependency>

</dependencies>
```



```scala
/*
* hello world用的scala test编写，pom里面集成了delta和spark可以不添加的
* scala test 不要使用java的junit，scala专属的很好用比junit好太多了
* 定义一个基类，基类里面可以定义自己常用的变量，代码，我常用spark所以定义的spark
*/

// scala 任意表达式都有返回值，具体返回值取决于最后一行

abstract class UnitSpec extends AnyFunSuite {
  val spark = SparkSession
    .builder()
    .appName("天官赐福")
    .master("local[*]")
    .getOrCreate()
  import spark.implicits._

}

class ScalaTest extends UnitSpec {
  
  test("hello world") {
    println("hello scala")
  }
  
}

```



# scala基础

## 变量

- scala中全是对象
- 变量声明 var | val 变量名 [: 变量类型] = 变量值
- 变量类型可以省略，编译器会自动推导
- var修饰的变量可以改变，val修饰的变量不可以改变(引用类型的值是可以更改的)
- 声明Long类型的时候后面需要添加 **L 或者 l** 
- 声明float类型的时候后面需要添加 **f 或者 F** 
- 科学计数法形式:如：5.12e2 = 5.12乘以10的2次方 5.12E-2 = 5.12除以10的2次方

## 数据类型

![image-20220322141620044](scala.assets\image-20220322141620044.png)

| 数据类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| Byte     | 8位有符号补码整数。数值区间为 -128 到 127                    |
| Short    | 16位有符号补码整数。数值区间为 -32768 到 32767               |
| Int      | 32位有符号补码整数。数值区间为 -2147483648 到 2147483647     |
| Long     | 64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807 |
| Float    | 32 位, IEEE 754标准的单精度浮点数                            |
| Double   | 64 位 IEEE 754标准的双精度浮点数                             |
| Char     | 16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF             |
| String   | 字符序列                                                     |
| Boolean  | true或false                                                  |
| Unit     | 表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。 |
| Null     | Null类只有一个实例对象，null，类似于Java中的null引用。null可以赋值给任意 **引用类型**(AnyRef)，但是不能赋值给**值类型(AnyVal: **比如 **Int, Float, Char,** **Boolean, Long, Double, Byte, Short)** |
| Nothing  | Nothing类型在Scala的类层级的最低端；它是任何其他类型的子类型，可以作为没有正常返回值的方法的返回类型，非常直观的告诉你这个方法不会正常返回，而且由于Nothing是其他任意类型的子类，他还能跟要求返回值的方法兼容。 |
| Any      | Any是所有其他类的超类                                        |
| AnyRef   | 求返回值的方法兼容。AnyRef类是Scala里所有引用类(reference class)的基类 |

## 类型转换

scala值类型转换其实是隐式转换，下面会详细介绍。

自动类型转换的逆过程，将容量大的数据类型转换为容量小的数据类型。使用时要 加上强制转函数，**但可能造成精度降低或溢出**,格外要注意。 

```scala
// java : 
int num = (int)2.5 
// scala : 对象
var num: Int = 2.7.toInt 
// 基本类型转为string类型 + "" 即可
val str: String = 2 + ""
```

## 运算符

### 算数运算符

| 运算符 |  描述  | 实例                                       |
| :----: | :----: | ------------------------------------------ |
|   +    |   加   | 10 + 20 = 30                               |
|   -    |   减   | 10 - 20 = -10                              |
|   *    |   乘   | 10 * 20 = 200                              |
|   /    |   除   | 10 / 20 = 0.5                              |
|   //   | 取整除 | 返回除法的整数部分（商） 9 // 2 输出结果 4 |
|   %    | 取余数 | 返回除法的余数 9 % 2 = 1                   |
|   **   |   幂   | 又称次方、乘方，2 ** 3 = 8                 |

### 逻辑运算符

| 运算符 | 描述                                                       |
| ------ | ---------------------------------------------------------- |
| &&     | A && B AB都为true则为true                                  |
| \|\|   | A && B AB只要一个为true则为true                            |
| !      | 如果 x 为 True，返回 False<br />如果 x 为 False，返回 True |

### 赋值运算符

| 运算符 | 描述                       | 实例                                  |
| ------ | -------------------------- | ------------------------------------- |
| =      | 简单的赋值运算符           | c = a + b 将 a + b 的运算结果赋值为 c |
| +=     | 加法赋值运算符             | c += a 等效于 c = c + a               |
| -=     | 减法赋值运算符             | c -= a 等效于 c = c - a               |
| *=     | 乘法赋值运算符             | c *= a 等效于 c = c * a               |
| /=     | 除法赋值运算符             | c /= a 等效于 c = c / a               |
| //=    | 取整除赋值运算符           | c //= a 等效于 c = c // a             |
| %=     | 取 **模** (余数)赋值运算符 | c %= a 等效于 c = c % a               |
| **=    | 幂赋值运算符               | c **= a 等效于 c = c ** a             |

### 比较运算符

| 运算符 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| ==     | 检查两个操作数的值是否 **相等**，如果是，则条件成立，返回 True |
| !=     | 检查两个操作数的值是否 **不相等**，如果是，则条件成立，返回 True |
| >      | 检查左操作数的值是否 **大于** 右操作数的值，如果是，则条件成立，返回 True |
| <      | 检查左操作数的值是否 **小于** 右操作数的值，如果是，则条件成立，返回 True |
| >=     | 检查左操作数的值是否 **大于或等于** 右操作数的值，如果是，则条件成立，返回 True |
| <=     | 检查左操作数的值是否 **小于或等于** 右操作数的值，如果是，则条件成立，返回 True |

### 运算符优先级

| 运算符                   | 描述                   |
| ------------------------ | ---------------------- |
| **                       | 幂 (最高优先级)        |
| * / % //                 | 乘、除、取余数、取整除 |
| + -                      | 加法、减法             |
| <= < > >=                | 比较运算符             |
| == !=                    | 等于运算符             |
| = %= /= //= -= += *= **= | 赋值运算符             |

### 位运算符

| 运算符 | 描述         |
| ------ | ------------ |
| <<     | 左移动运算符 |
| \>>    | 右移动运算符 |
| \>>>   | 无符号右移   |

## 循环

### if-else

```scala
// 基本语法 {}内的逻辑只有一行的话可以去掉 java也是如此

if (条件表达式1) { 
    执行代码块1 
}else if (条件表达式2) {
    执行代码块2 
}else { 
    执行代码块n 
}

// scala 每一个表达式都可以进行赋值操作 

test("test if") {
    val num: Int = 10
    val res = if(num < 0) {
      println(s"num: $num 小于 0")
      "<0"
    } else if(num == 0) {
      println(s"num: $num 等于 0")
      "=0"
    }else {
      println(s"num: $num 大于 0")
      ">0"
    }
    println(s"res : $res")
}
```

### for

- {} 和 () 对于for来说都可以，**(指的是 for(){} 可以 写为 for{}{} )**
- for 推导式有一个不成文的约定：**当for推导式仅包含单一表达式时使用()**，**当其包含多个表达式时使用{}**
- 当使用 **{}** 来换行写表达式时，分号就不用写了

```scala
test("for") {

    // 闭区间 1 2 3
    for(i <- 1 to 3) {
      println(i)
    }

    // 左闭右开 1 2
    println("*"*50)
    for(i <- 1 until 3) {
      println(i)
    }

    // 添加循环守卫,相当于continue 1 3 
    /*
    * i: 1 | j: 1
    * i: 1 | j: 2
    * i: 2 | j: 1
    * i: 2 | j: 2
    */
    println("*"*50)
    for(i <- 1 to 3 if i != 2) {
      println(i)
    }

    // 嵌套循环 Vector(1, 2, 3)
    println("*"*50)
    for(i <- 1 to 2; j <- 1 to 2) {
      println(s" i: $i | j: $j")
    }

    // 将遍历的结果放入集合中
    println("*"*50)
    val res = for(i <- 1 to 3) yield i
    println(res)

    // 控制步长 2
    println("*"*50)
    for (i <- Range(1,3,2)){
      println(i)
    }
    println("*"*50)
    for(i <- 1 to 3 if i % 2 == 0) {
      println(i)
    }
    
    // scala中没有break，感觉没必要利用Breaks 了解就可以了
    println("*" * 50)
    val numList = List(1, 2, 3);
    val loop = new Breaks;
    loop.breakable {
      for (num <- numList) {
        println(s"num: $num")
        if (num == 2) loop.break
      }
    }
  
  }
// break打印结果
**************************************************
num: 1
num: 2
```

### while

```scala
# 基本语法
while(condition)
{
   statement(s);
}

test("while") {
    var num = 5
    while (num > 3) {
        println(s"num: $num")
        num -= 1
    }
}

// 打印值 
num: 5
num: 4
```

### do while

```scala
// 基本语法
do {
   statement(s);
} while( condition );

test("do while") {
    var num = 5
    do {
      println(s"num: $num")
      num -= 1
    } while(num > 3)
}
// 打印值
num: 5
num: 4
```

# 方法和函数

## 方法

- scala 中方法即函数，函数即方法，个人理解都是代码快，定义形式稍微有些差别

- scala 函数的形参默认为 val ，不能进行修改
- def sum(args: Int*) : Int {} ---> args是个集合，通过for循环可以访问到各个值
- 默认最后一行为返回的结果， 可以不写 return

```scala
def 函数名 ([参数名: 参数类型], ...)[[: 返回值类型] =] {
    语句... return 返回值 
}

// scala 执行程序的时候定义为object，下面会详细介绍， object中的方法和java静态方法差不多
object ScalaObjectTest extends App {

  def m1(name: String, age: Int) : Int = {
    val weight = 100
    println(s"name: $name 的 age: $age weight: $weight")
    weight
  }

  /**
   * 这里是scala的入口函数, 因为上面我们继承了App 所以不需要在编写main函数了
   *
   * @param args
   * @return void
   */
  /*def main(args: Array[String]): Unit = {
    println(m1("tom", 18))
  } */
  println(m1("tom", 18))
}
```

## 函数

### 函数

- 函数是头等公民，**它可以像任何其他数据类型一样被传递和操作**
- 神奇的下划线，任何一个方法都可以转为函数， 方法名_
- scala中存在惰性函数， java中一般实现惰性函数是用懒汉模式

```scala
/**
  * 定义一方法，形参为一个函数
  *
  * @param fun
  * @return int
  */
def m2(fun: (Int, Int) => Int) : Int = {
    fun(2, 3)
}

// 定义一个函数
val fun1 = (num1: Int, num2: Int) => num1 * num2

// 调用方法 打印结果为6
println(m2(fun1))

/**
  * 定义一方法, 然后用下划线将其转为函数
  *
  * @param fun
  * @return int
  */	
def m3(num1: Int, num2: Int) : Int = num1 + num2
// 用下划线将方法转为函数
val fun_m3 = m3 _
// 打印结果为5
println(m2(fun_m3))
```

### 惰性函数

- 惰性计算（尽可能延迟表达式求值）是许多函数式编程语言的特性。

- lazy 不能修饰 var 类型的变量

- 不但是 在调用函数时，加了 lazy ,会导致函数的执行被推迟，我们在声明一个变 

  量时，如果给声明了 lazy ,那么变量值得分配也会推迟。 比如 lazy val i = 10

```scala
test("lazy") {
    lazy val res = 10 + 20
    println(s"res: $res")
  }
```

## 异常

- scala 也是 try catch finally，抛出异常也是throw ，有一点点小区别

```scala
// case 模式匹配下面会详细讲解
test("exception") {
    // try catch
    try {
      val num = 10 / 0
    } catch {
      case ex: ArithmeticException=> throw new Exception("抛出异常")
      case ex: Exception => println("其他异常")
    } finally {
      println("关闭资源")
    }
  }
```

# 数据结构

- Scala同时支持**不可变集合**和**可变集合**，不可变集合可以安全的并发访问
  - 不可变集合：scala.collection.immutable 
  - 可变集合： scala.collection.mutable 
  
- Scala**默认采用不可变集合**

- Scala的集合有三大类：序列Seq、集Set、映射Map，所有的集合都扩展自**Iterable**特质

- <font color='red'>**:: 该方法被称为cons，意为构造，向队列的头部追加数据，创造新的列表。用法为x::list,其中x为加入到头部的元素，无论x是列表与否，它都只将成为新生成列表的第一个元素，也就是说新生成的列表长度为list的长度＋1(btw, x::list等价于list.::(x))**</font>

- <font color='red'>**:+和+: 两者的区别在于:+方法用于在尾部追加元素，+:方法用于在头部追加元素，和::很类似，但是::可以用于pattern match ，而+:则不行. 关于+:和:+,只要记住冒号永远靠近集合类型就OK了，加号位置决定元素(无论元素还是集合)加在前还是后 都只添加1个元素。**</font>

- <font color='red'>**++ 该方法用于连接两个集合(列表，数组等)，list1++list2**</font>

- <font color='red'>**::: 该方法只能用于连接两个List类型的集合**</font>

- 上面这几个红色标识的特别重要因为在操作集合的时候都有用到，scala中的集合方法其实和java没什么太大区别不做详细介绍了

  

## 可变集合

![image-20220324101727830](scala.assets\image-20220324101727830.png)

## 不可变集合

![image-20220324101754736](scala.assets\image-20220324101754736.png)

## seq

- 下标从0开始
- 在Scala中列表要么为空（Nil表示空列表）要么是一个head元素加上一个tail列表。9 :: List(5, 2) 

```scala
  test("array") {
    // 如果new，相当于调用了数组的apply方法，直接为数组赋值
    val arr1 = new Array[Int](8)
    println(arr1)
    println(arr1.toBuffer)
    val arr2 = Array[Int](8)
    println(arr2)
    println(arr2.toBuffer)

    // 定长数组 0: tom 1: jack
    val arr3 = Array[String]("tom", "jack", "rose")
    println(s"0: ${arr3(0)} 1: ${arr3(1)}")

    // 变长数组
    val arr4 = ArrayBuffer[Int]()
    arr4 += 1
    arr4 += (2, 3, 4, 5)
    arr4 ++= Array(6, 7)
    arr4 ++= ArrayBuffer(8,9)  
    println(arr4)
  }

[I@28eaa59a
ArrayBuffer(0, 0, 0, 0, 0, 0, 0, 0)
[I@41fbdac4
ArrayBuffer(8)
0: tom 1: jack
ArrayBuffer(1, 2, 3, 4, 5, 6, 7, 8, 9)

```

## map & flatMap

```scala
test("map && flatMap") {
    // 下划线代表ele
    val line = List("hello tom hello jack", "hello tom")
    val res1: Seq[Array[String]] = line.map(_.split(" "))
    // ArrayBuffer(hello, tom, hello, jack) ArrayBuffer(hello, tom)
    for (list <- res1) {
      println(list.toBuffer)
    }

    val res2: Seq[String] = line.flatMap(_.split(" "))
    // List(hello, tom, hello, jack, hello, tom)
    println(res2)

    // key: hello | v: List((hello,1), (hello,1), (hello,1))
    // key: tom | v: List((tom,1), (tom,1))
    // key: jack | v: List((jack,1))
    val res3: Map[String, List[(String, Int)]] = line.flatMap(_.split(" ")).map((_, 1)).groupBy(_._1)
    for((k, v) <- res3 ) {
      println(s"key: $k | v: $v")
    }

    // mapValues 其实就是一个对Map的 value的 map操作
    // foldLeft 0 为初始值， 第一个_ 代表上次计算的结果， _._2 第一个_代表集合的元素tuple _2代表取tuple的第二个元素
    // key: hello | v: 3  key: tom | v: 2 key: jack | v: 1
    val res4: Map[String, Int] = line.flatMap(_.split(" ")).map((_, 1)).groupBy(_._1).mapValues(_.foldLeft(0)(_+_._2))
    for((k, v) <- res4) {
      println(s"key: $k | v: $v")
    }
  }
```

## map

```scala
test("map") {
    val map1 = Map("tom" -> 18, "jack" -> 20)
    val map2 = Map(("tom", 18), ("jack", 20))

    // 18 | Some(18) | 18
    println(s"${map1("tom")} | ${map1.get("tom")} | ${map1.getOrElse("tom", "hello")}")
    // None | hello
    println(s"${map1.get("tom1")} | ${map1.getOrElse("tom1", "hello")}")
    // 抛出异常 key not found: tom1
    println(s"${map1("tom1")}")
  }

```

| 方法    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| **++**  | **def ++(xs: Map[(A, B)]): Map[A, B]** 返回一个新的 Map，新的 Map xs 组成 |
| **-**   | **def -(elem1: A, elem2: A, elems: A\*): Map[A, B]** 返回一个新的 Map, 移除 key 为 elem1, elem2 或其他 elems。 |
| **--**  | **def --(xs: GTO[A]): Map[A, B]** 返回一个新的 Map, 移除 xs 对象中对应的 key |
| **get** | **def get(key: A): Option[B]** 返回指定 key 的值             |

## tuple

```scala
test("tuple"){
    val t1: (String, Int) = ("tom", 18)
    val t2: (String, Int, Int) = Tuple3("tom", 18, 100)
    // t1_1:| tom
    println(s"t1_1:| ${t1._1}")
  }
```

## list & set

- list 下标从0开始

```scala
test("list & set") {
    val l1 = List(1, 2, 3)
    val lb1 = ListBuffer(1, 2, 3)
    val s1 = Set(1, 2, 3)
    val hs1 = mutable.HashSet(1, 2, 3)
    // l1 :  List(1, 2, 3) | l1_0: 1 | l1_1: 2
    println(s"l1 :  $l1 | l1_0: ${l1(0)} | l1_1: ${l1(1)}")
    println(s"lb1 :  $lb1")
    // apply 和 () 都是检测set是否包含元素
    // s1 :  Set(1, 2, 3) | s1_0: false | s1_1: true
    println(s"s1 :  $s1 | s1_0: ${s1.apply(0)} | s1_1: ${s1(1)}")
    println(s"hs1 :  $hs1")
  }
```

| list方法（scala中的+: :: 其实都是方法） | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| **+:**                                  | **def +:(elem: A): List[A]**   为列表预添加元素              |
| **::**                                  | **def ::(x: A): List[A]** 在列表开头添加元素                 |
| **:::**                                 | **def :::(prefix: List[A]): List[A]** 在列表开头添加指定列表的元素 |
| **:+**                                  | **def :+(elem: A): List[A]**  复制添加元素后列表。           |
| **apply**                               | 通过列表索引获取元素                                         |
| **contains**                            | 检测列表中是否包含指定的元素                                 |
| **copyToArray**                         | 将列表的元素复制到数组中。                                   |
| **distinct**                            | 去除列表的重复元素，并返回新列表                             |
| **drop**                                | 丢弃前n个元素，并返回新列表                                  |
| **dropRight**                           | 丢弃最后n个元素，并返回新列表                                |
| **filter**                              | 过滤                                                         |
| **foreach**                             | 将函数应用到列表的所有元素                                   |
| **head**                                | 获取列表的第一个元素                                         |
| **indexOf**                             | 从指定位置 from 开始查找元素第一次出现的位置                 |
| **isEmpty**                             | 检测列表是否为空                                             |
| **last**                                | 返回最后一个元素                                             |
| **length**                              | 返回列表长度                                                 |
| **map**                                 | 通过给定的方法将所有元素重新计算                             |
| **mkString**                            | 列表所有元素作为字符串显示                                   |
| **reverse**                             | 列表反转                                                     |
| **sorted**                              | 列表排序                                                     |
| **tail**                                | 返回所有元素，除了第一个                                     |
| **take**                                | 提取列表的前n个元素                                          |

# 面向对象

- 封装
- 继承
- 多态

## class

- [修饰符] class 类名{} scala中不定义修饰符， 修饰符默认就是public

- [访问修饰符] var 属性名[: 类型] = 属性值 , 也可以使用符号**_(下划线)**，让系统分配默认值

- ```scala
  class 类名(形参列表) {// 主构造器 
  	// 类体 
  	def this(形参列表) {
          this()
  	 	// 辅助构造器 
   	}
  
   	def this(形参列表) {
   	 //辅助构造器可以有多个... 
   	} 
   }
  ```

  - 构造器形参只是name: String -->那么这个参数是局部变量， val name: String --> 私有的成员变量， var name: String -->私有的提供了get set的成员变量
    - 给成员变量添加注解可以自动生成get 和 set @BeanProperty
  - 如果主构造器无参数，小括号可省略，构建对象时调用的构造方法的小括号也可以省略
  - 辅助构造器通过不同参数列表进行区分， 在底层就是构造器重载。

## 伴生对象

- Scala语言是完全面向对象(万物皆对象)的语言，所以并没有静态的操作 (即在Scala中没有静态的概念)。但是为了能够和Java语言交互(因为Java中有静 态概念)，就产生了一种特殊的对象来模拟类对象，我们称之为类的伴生 对象。这个类的所有静态内容都可以放置在它的伴生对象中声明和调用。
- Scala中伴生对象采用object关键字声明，伴生对象中声明的全是 **"静态"** 内容，可以通过**伴生对象名称**直接调用。
- 伴生对象对应的类称之为伴生类，伴生对象的名称应该和伴生类名一致。
- 伴生对象中的属性和方法都可以通过**伴生对象名(类名)**直接调用访问 。
- 所谓的伴生对象其实就是类的静态方法和成员的集合。
- 伴生对象的声明应该和伴生类的声明在**同一个源码文件**中。
- 在伴生对象中定义apply方法 ，可以实现： 类名(参数) 方式 来创建对象实例。

## trait

- 从面向对象来看，接口并不属于面向对象的范畴，Scala是**纯面向对象**的语言， 在Scala中，**没有接口**。

- Scala语言中，**采用特质trait（特征）来代替接口的概念**，也就是说，多个类具 

  有相同的特征（特征）时，就可以将这个特质（特征）独立出来，采用关键字 

  **trait**声明。 理解trait 等价于(interface + abstract class)

- ```scala
  trait 特质名 { trait体 }
  # class 类名 extends 特质1 with 特质2 with 特质3 ..
  # class 类名 extends 父类 with 特质1 with 特质2 with 特质3
  ```

## 样例类

在Scala中样例类是一中特殊的类，可用于模式匹配。case class是多例的，后面要跟构造参数，case object是单例的

```scala
case class Demo1(id: String, name: String)
case object Demo2
```

## Option类型

// 在Scala中Option类型样例类用来表示可能存在或也可能不存在的值(Option的子类有Some和None)。Some包装了某个值，None表示没有值

# scala进阶

## 模式匹配

- Scala中的**模式匹**配类似于Java中的switch语法，但是**更加强大**

- 匹配字符串

```scala
test("case string") {
    val arr = Array("tom", "jack")
    val name = arr(Random.nextInt(arr.length))
    name match {
      case "tom" => println("I am tom")
      case "jack" => println("I am jack")
      case _ => println("else")
    }
  }
```

- 匹配类型

```scala
 test("case type") {
    val arr = Array("tom", 1.2, 2)
    val v = arr(Random.nextInt(arr.length))
    v match {
      case x: Int => println(s"Int $x")
      case y: Double if(y > 2) => println(s"Double: $y")
      case _ => println(s"其他 $v")
    }
  }
```

- 匹配数组

```scala
test("case array") {
    // 8
    // val arr = Array(1, 3, 5)
    // 0.....
   val arr = Array(0, 1)

    arr match {
      case Array(1, x, y) => println(x + y)
      case Array(0) => println("only 0")
      case Array(0, _*) => println("0.....")
      case _ => println("其他")
    }
  }
```

- list

```scala
test("case list") {
    // 空
    // val lst: List[Int] = List()
    // something else
    // val lst: List[Int] = null
    // x: 3 y: -1
    val lst = List(3, -1)
    lst match {
      case Nil => println("空")
      case 0 :: Nil => println("only 0")
      case x :: y :: Nil => println(s"x: $x y: $y")
      case 0 :: tail => println("0 ...")
      case _ => println("something else")
    }
  }
```

- tuple

```scala
test("case tup"){
    // 3
    val tup = (2, 3, 7)
    tup match {
      case (1, x, y) => println(s"1, $x , $y")
      case (_, z, 7) => println(z)
      case  _ => println("else")
    }
  }
```

- 样例类

```scala
test("case sample class"){
    val lst = List(Cat("tom", 18), Dog("jinMao"), Animal)
    lst(Random.nextInt(lst.size)) match {
      case Cat(name, age) => println(s"$name $age")
      case Dog(name) => println(s"Dog | $name")
      case Animal => println("Animal")
      case _ => println("else")
    }
  }
```

## 高阶函数

### 偏函数

被包在花括号内没有match的一组case语句是一个偏函数，它是PartialFunction[A, B]的一个实例，A代表参数类型，B代表返回类型，常用作输入模式匹配

```scala
def func1: PartialFunction[String, Int] = {
    case "one" => 1
    case "two" => 2
    case _ => -1
  }

 def func2: PartialFunction[Any, Int] = {
    case i: Int => i + 1 // case语句可以自动转换为偏函数
  }

 test("PartialFunction") {
    // 2
    println(func1("two"))
    // 2 3
    List(1, 2, "ABC").collect(func2).foreach(println(_))
    // 简写 List(2, 3)
    val list3 = List(1, 2, "ABC").collect{ case i: Int => i + 1 }
    println(list3)
  }
```

### 作为参数的函数

- scala 中函数可以作为参数，方法也可以作为参数，每一个方法都可以通过下划线 _ 转为函数
- 和匿名函数其实差不多

```scala
def plus(x: Int): Int = x + 1
  test("func param"){
    // 将方法转为函数
    val funTmp = plus _
    // 直接传进去 方法自动转为了函数 234
    println(Array(1, 2, 3).map(plus).mkString)
    // 234
    println(Array(1, 2, 3).map(funTmp).mkString)
  }
```

### 匿名函数

- 没有将函数赋值给变量的函数就叫做匿名函数
- i => {i * 2}

```scala
test("no name func"){
    val arr = Array(1, 2, 3)
    //越来越简化 2 4 6
    arr.map((i: Int) => {i * 2}).foreach(println(_))
    arr.map(i => {i * 2}).foreach(println(_))
    arr.map(_ * 2).foreach(println(_))
  }
```

### 闭包

- 闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。

```scala
// 在 multiplier 中有两个变量：i 和 factor。其中的一个 i 是函数的形式参数，在 multiplier 函数被调用时，i 被赋予一个新的值。然而，factor不是形式参数，而是自由变量，考虑下面代码：
var factor = 3  
val multiplier = (i:Int) => i * factor  
// 这里我们引入一个自由变量 factor，这个变量定义在函数外面。
// 这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。
```

### 柯力化

- 函数编程中，接受**多个参数的函数**都可以转化为接受**单个参数的函数**，这个转 化过程就叫柯里化 

```scala
// 定义一个方法 俩个参数
  def mul1(x: Int, y: Int): Int = x * y
  // 定义一个方法 1个参数 返回的是一个函数
  def mul2(x: Int): Int => Int = (y: Int) => x * y
  // 定义一个柯力化方法
  def mul3(x: Int)(y: Int) = x * y

  test("ke li hua") {
    println(mul3(2)(3))
  }
```

### 隐式转换

- 隐式转换函数是以implicit关键字声明的带有单个参数的函数。这种函数将会 **自动**应用，将值从一种类型转换为另一种类型 
- 隐式转换函数的函数名可以是任意的，隐式转换与函数名称无关，只与 函数**签名**（函数参数类型和返回值类型）有关
- 隐式函数可以有多个(即：隐式函数列表)，**但是需要保证在当前环境下， 只有一个隐式函数能被识别**

#### 隐式方法

```scala
implicit def f1(d: Double): Int = { d.toInt }
test("Implicit values") {
    val num: Int = 2.3
}
```

![image-20220326154230865](scala.assets\image-20220326154230865.png)

#### 隐式值

- 隐式值也叫**隐式变量**，将某个形参变量标记为implicit，所以编译器会在方法 省略隐式参数的情况下去搜索作用域内的隐式值作为缺省参数

```scala
// implicit val str1: String = "jack"
  def hello(implicit name: String = "rose"): Unit = {
    println(name + " hello")
  }
  test("Implicit value") {
    // jack hello
    // 注释掉隐式参数 打印 rose hello
    hello
    // tom hello
    hello("tom")
  }
```

#### 隐式类

- 在scala2.10后提供了**隐式类**，可以使用implicit声明类，隐式类的非常强大， 同样可以扩展类的功能，比前面使用**隐式转换丰富类库功能**更加的方便，在集合中隐式类会发挥重要的作用
- 其所带的构造参数有且只能有一个
- 隐式类必须被定义在“类”或“伴生对象”或“包对象”里，即<font color='red'>**隐式类不能是顶级的(top-level objects)**</font>。

```scala
implicit class Calculator(x: Int) {
    def add(a: Int) = a + x
  }
  test("implicit class"){
    // 隐式类就是把int类设置为Calculator的构造参数， 然后定义一个方法，
    // int类型的对象就可以调用add方法了
    println(2.add(3))
  }
```

# Akka

- Akka是JAVA虚拟机JVM平台上构建高并发、分布式和容错应用的工具包和运行时， 你可以理解成**Akka是编写并发程序的框架**。
- Akka用scala语言写成，同时提供了scala和java的开发接口。
- Akka处理并发的方法基于Actor模型。
- spark前期版本通讯使用的akka，现在版本使用的netty

## Actor

- Actor 与 Actor 之间只能用消息进行通信，当一个 Actor 给另外一个 Actor发 消息，消息是有顺序的(消息队列)，只需要将消息投寄的相应的邮箱即可。
-  ActorSystem 的职责是负责创建并管理其创建的 Actor， ActorSystem 是单 例的(可以ActorSystem是一个工厂，专门创建Actor)，一个 JVM 进程中有一 个即可，而 Acotr 是可以有多个的。

### 工作原理

- ActorySystem创建Actor。
- ActorRef:可以理解成是Actor的代理或者引用。消息是通过ActorRef来发送, 而不能通过Actor 发送消息，通过哪个ActorRef 发消息，就表示把该消息发 给哪个Actor。
- 消息发送到Dispatcher Message (消息分发器)，它得到消息后，会将消息进 行分发到对应的MailBox。(注: Dispatcher Message 可以理解成是一个线程 池, MailBox 可以理解成是消息队列，可以缓冲多个消息，遵守FIFO)。
- Actor 可以通过 receive方法来获取消息，然后进行处理。
- ![image-20220326201924812](scala.assets\image-20220326201924812.png)

### 发送消息

| 方式 | 描述                                 |
| ---- | ------------------------------------ |
| !    | 发送异步消息，没有返回值。           |
| !?   | 发送同步消息，等待返回值。           |
| !!   | 发送异步消息，返回值是 Future[Any]。 |

### 简单示例

#### 自我发送

```scala
package delta

import akka.actor.{Actor, ActorSystem, Props}

/**
 * 描述信息
 *
 * @create: 2022-03-26 21:46
 */
class ActorTest extends Actor{
  // Receive 其实是一个偏函数 case就是最简洁的偏函数
  override def receive: Receive = {
    case "hello" => println("接受一个 hello 回复一个 hello")
    case "exit" =>
      // 停止ActorRef
      context.stop(self)
      // 将ActorTest退出ActorSystem
      context.system.terminate()
    case _ => println("不能识别发送的信息")
  }
}

object ActorTest{
  // 创建ActorSystem 用来创建Actor
  val actorSystemSystem = ActorSystem("actorSystem")
  // 创建一个Actor， 并返回ActorRef, Props[ActorTest] 反射
  val actorTestRef = actorSystemSystem.actorOf(Props[ActorTest], "ActorTest")

  def main(args: Array[String]): Unit = {
    actorTestRef ! "hello"
    actorTestRef ! "exit"

  }
}

```

#### 互相发送

```scala
package akka

import akka.actor.{Actor, ActorRef, ActorSystem, Props}

/**
 * 描述信息
 *
 * @create: 2022-03-26 21:46
 */
// MyActor1 如果要向 MyActor2 发送消息，需要持有 ActorRef
class MyActor1(myActor2: ActorRef) extends Actor{
  // Receive 其实是一个偏函数 case就是最简洁的偏函数
  override def receive: Receive = {
    case "start" =>
      println("MyActor1 start ...")
      self ! "hi"
    case "hi" =>
      println("MyActor1向myActor2发送： hi")
      myActor2 ! "hi"
    case _ => println("不能识别发送的信息")
  }
}

// MyActor2 为什么不需要持有 MyActor1的 ref 因为sender里面包含了
class MyActor2() extends Actor{
  // Receive 其实是一个偏函数 case就是最简洁的偏函数
  override def receive: Receive = {
    case "hi" =>
      println("MyActor2向myActor1发送： hi")
      // sender里面包含了发送方的ref
      sender() ! "hi"
  }
}

object MyActor extends App{
  // 创建ActorSystem 用来创建Actor
  val actorSystemSystem: ActorSystem = ActorSystem("actorSystem")
  // 创建Actor， 并返回ActorRef
  val myActor2Ref: ActorRef = actorSystemSystem.actorOf(Props[MyActor2], "MyActor2")
  val myActor1Ref: ActorRef = actorSystemSystem.actorOf(Props(new MyActor1(myActor2Ref)), "MyActor1")

  myActor1Ref ! "start"
}

```

## 网络编程

### 网络链路

![image-20220326225204516](scala.assets\image-20220326225204516.png)

### **端口**

- 0号是**保留端口**. 

- Ø 1-1024是**固定端口** 又叫有名端口,即被某些程序固定使用,一般程序员不使用. 22: SSH远程登录协议 23: telnet使用 21: ftp使用 25: smtp服务使用 80: iis使用 7: echo服务 

- Ø 1025-65535是**动态端口** 这些端口，程序员可以使用

### server-client

#### common

```scala
package akka.yellowchicken.common

//使用样例类来构建协议
//客户端发给服务器协议(序列化的对象)
case class ClientMessage(mes: String)

//服务端发给客户端的协议(样例类对象)
case class ServerMessage(mes: String)

```

#### server

```scala
package akka.yellowchicken.server

import akka.actor.{Actor, ActorRef, ActorSystem, Props}
import akka.yellowchicken.common.{ClientMessage, ServerMessage}
import com.typesafe.config.ConfigFactory

class YellowChickenServer extends Actor{
  override def receive:Receive = {
    case "start" => println("start 小黄鸡客服开始工作了....")
      //如果接收到ClientMessage
    case ClientMessage(mes) => {
      //使用match --case 匹配(模糊)
      mes match {
        case "大数据学费" => sender() ! ServerMessage("35000RMB")
        case "学校地址" => sender() ! ServerMessage("北京昌平xx路xx大楼")
        case "学习什么技术" => sender() ! ServerMessage("大数据 前端 python")
        case _ => sender() ! ServerMessage("你说的啥子~")
      }
    }
  }
}

//主程序-入口

object YellowChickenServer extends App {


  val host = "127.0.0.1" //服务端ip地址
  val port = 9999
  //创建config对象,指定协议类型，监听的ip和端口
  val config = ConfigFactory.parseString(
    s"""
       |akka.actor.provider="akka.remote.RemoteActorRefProvider"
       |akka.remote.netty.tcp.hostname=$host
       |akka.remote.netty.tcp.port=$port
        """.stripMargin)

  //创建ActorSystem
  //url (统一资源定位)
  val serverActorSystem = ActorSystem("Server",config)
  //创建YellowChickenServer 的actor和返回actorRef
  val yellowChickenServerRef: ActorRef = serverActorSystem.actorOf(Props[YellowChickenServer],"YellowChickenServer")

  //启动
  yellowChickenServerRef ! "start"


}
```

#### client

```scala
package akka.yellowchicken.client

import akka.actor.{Actor, ActorRef, ActorSelection, ActorSystem, Props}
import akka.yellowchicken.common.{ClientMessage, ServerMessage}
import com.typesafe.config.ConfigFactory

import scala.io.StdIn

class CustomerActor(serverHost: String, serverPort: Int) extends Actor {
  //定义一个YellowChickenServerRef
  var serverActorRef: ActorSelection = _

  //在Actor中有一个方法PreStart方法，他会在actor运行前执行
  //在akka的开发中，通常将初始化的工作，放在preStart方法
  override def preStart(): Unit = {
    println("preStart() 执行")
    serverActorRef = context.actorSelection(s"akka.tcp://Server@${serverHost}:${serverPort}/user/YellowChickenServer")

    println("serverActorRef=" + serverActorRef)
  }

  override def receive: Receive = {
    case "start" => println("start,客户端运行，可以咨询问题")
    case mes: String => {
      //发给小黄鸡客服
      serverActorRef ! ClientMessage(mes) //使用ClientMessage case class apply
    }
    //如果接收到服务器的回复
    case ServerMessage(mes) => {
      println(s"收到小黄鸡客服(Server): $mes")
    }

  }
}


//主程序-入口
object CustomerActor extends App {

  val (clientHost, clientPort, serverHost, serverPort) = ("127.0.0.1", 9990, "127.0.0.1", 9999)
  val config = ConfigFactory.parseString(
    s"""
       |akka.actor.provider="akka.remote.RemoteActorRefProvider"
       |akka.remote.netty.tcp.hostname=$clientHost
       |akka.remote.netty.tcp.port=$clientPort
        """.stripMargin)

  //创建ActorSystem
  val clientActorSystem = ActorSystem("client", config)

  //创建CustomerActor的实例和引用
  val customerActorRef: ActorRef = clientActorSystem.actorOf(Props(new CustomerActor(serverHost, serverPort)), "CustomerActor")

  //启动customerRef/也可以理解启动Actor
  customerActorRef ! "start"

  //客户端可以发送消息给服务器
  while (true) {
    println("请输入要咨询的问题")
    val mes = StdIn.readLine()
    customerActorRef ! mes
  }


}

```

# 设计模式

# 泛型

- 如果我们要求函数的参数可以接受任意类型。可以使用泛型，这个类型可 以代表任意的数据类型。
- List<E> **extends** Collection<E>

## 协变逆变不变

- 协变 class MyList[+T] 比如Son是Father的子类，则MyList[Son] 也是 MyList[Father]的**子类**

- 逆变 class MyList[-T] 比如Son是Father的子类，则MyList[Son] 也是 MyList[Father]的**父类**

- 不变 class MyList[T] 比如Son是Father的子类，则MyList[Son] 与 MyList[Father] **无父子关系**
- ![image-20220329103550931](scala.assets\image-20220329103550931.png)

## 上界

- 在 Java 泛型里表示某个类型是 A 类型的子类型，使用 extends 关键字，这 种形式叫 upper bounds(上限或上界)，语法如下：
  <T extends A>  or  <? extends A>

- 在 scala 里表示某个类型是 A 类型的子类型，也称上界或上限，使用 **<:** 关键字，语法如下：

  [T <: A]  or [_ <: A]

## 下界

- 在 Java 泛型里表示某个类型是 A类型的父类型，使用 super 关键字

  <T super A>  or  <? super A>

- 在 scala 的下界或下限，使用 **>:** 关键字，语法如下： 
  [T >: A]  or [_ >: A]

## 上下文界定

- 定义

```scala
def f[A : B](a: A) = println(a) 
==
def f[A](a: A)(implicit arg:B[A]) = println(a)

// 上下文界定B就相当于隐式参数的类型，隐式参数的类型的泛型又是A
```

- 例子

```scala
// 方式1

class CompareComm4[T: Ordering](obj1: T, obj2: T)(implicit comparetor: Ordering[T]) {
  def geatter = if (comparetor.compare(obj1, obj2) > 0) obj1 else obj2
}

// 方式2, 将隐式参数放到方法内

class CompareComm5[T: Ordering](o1: T, o2: T) {
  def geatter = {
    def f1(implicit cmptor: Ordering[T]) = cmptor.compare(o1, o2)

    if (f1 > 0) o1 else o2
  }
}

// 方式3, 使用implicitly语法糖，最简单 (推荐使用)

class CompareComm6[T: Ordering](o1: T, o2: T) {
  def geatter = {
    // 这句话就是会发生隐式转换，获取到隐式值 personComparetor
    val comparetor = implicitly[Ordering[T]]
    println ("CompareComm6 comparetor" + comparetor.hashCode())
    if (comparetor.compare(o1, o2) > 0) o1 else o2
  }
}

// 一个普通的Person类
class Person(val name: String, val age: Int) {
  override def toString = this.name + "\t" + this.age
}
```

## 视图界定

- <% 的意思是“ view bounds”(视界)，它比<:适用的范围更广，除了所有的子类型，还允许隐式转换类型。 

- ```scala
  // 定义
  def method [A <% B](arglist): R = ...
  ==
  def method [A](arglist)(implicit viewAB: A => B): R = ...
  ==
  implicit def conver(a:A): B = …
  
  // <% 除了方法使用之外，class 声明类型参数时也可使用： 
  class A[T <% Int]
  ```

- ```scala
  // 例子1
  object ViewBoundsDemo {
    def main(args: Array[String]): Unit = { //方式1
      val compareComm1 = new CompareComm(20, 30)  
      println(compareComm1.greater)
      // 同时，也支持前面学习过的上界使用的各种方式,看后面代码
    }
  }
  
  class CompareComm[T <% Comparable[T]](obj1: T, obj2: T) {
    def greater = if (obj1.compareTo(obj2) > 0) obj1 else obj2
  }
  
  // 例子2
  class Person(val name: String, val age: Int) extends Ordered[Person] {
    override def compare(that: Person): Int = this.age - that.age
  
    override def toString: String = this.name + "\t" + this.age
  }
  
  class CompareComm2[T <% Ordered[T]](obj1: T, obj2: T) {
    def getter = if (obj1 > obj2) obj1 else obj2
    def getter2 = if (obj1.compareTo(obj2) > 0) obj1 else obj2
  }
  
  object test extends App {
    val p1 = new Person("tom", 10)
    val p2 = new Person("jack", 20)
    val compareComm2 = new CompareComm2(p1, p2)
    println (compareComm2.getter)
  }
  ```

  
