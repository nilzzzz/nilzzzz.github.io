---
layout: post
title: Java的基本数据类型四类八种
tag: Java
---

### Java常见的数据类型

一.八种基本的数据类型

**1、整型**

byte 、short 、int 、long

**2、浮点型**

float 、 double

**3、字符型**

char

**4、布尔型**

boolean

二.3种引用类型

类class ，就是自己定义的数据结构，还有一些Java类库中的：包括**String**，date等

接口interface

数组array

### 八种基本数据类型

![image-20200509155205046](https://gitee.com/XiaoShenKeHeBen/Static/raw/master/image/202005/09/155207-259324.png)

```
public class PrimitiveTypeTest {  
    public static void main(String[] args) {  
        // byte  
        System.out.println("基本类型：byte 二进制位数：" + Byte.SIZE);  
        System.out.println("包装类：java.lang.Byte");  
        System.out.println("最小值：Byte.MIN_VALUE=" + Byte.MIN_VALUE);  
        System.out.println("最大值：Byte.MAX_VALUE=" + Byte.MAX_VALUE);  
        System.out.println();  
  
        // short  
        System.out.println("基本类型：short 二进制位数：" + Short.SIZE);  
        System.out.println("包装类：java.lang.Short");  
        System.out.println("最小值：Short.MIN_VALUE=" + Short.MIN_VALUE);  
        System.out.println("最大值：Short.MAX_VALUE=" + Short.MAX_VALUE);  
        System.out.println();  
  
        // int  
        System.out.println("基本类型：int 二进制位数：" + Integer.SIZE);  
        System.out.println("包装类：java.lang.Integer");  
        System.out.println("最小值：Integer.MIN_VALUE=" + Integer.MIN_VALUE);  
        System.out.println("最大值：Integer.MAX_VALUE=" + Integer.MAX_VALUE);  
        System.out.println();  
  
        // long  
        System.out.println("基本类型：long 二进制位数：" + Long.SIZE);  
        System.out.println("包装类：java.lang.Long");  
        System.out.println("最小值：Long.MIN_VALUE=" + Long.MIN_VALUE);  
        System.out.println("最大值：Long.MAX_VALUE=" + Long.MAX_VALUE);  
        System.out.println();  
  
        // float  
        System.out.println("基本类型：float 二进制位数：" + Float.SIZE);  
        System.out.println("包装类：java.lang.Float");  
        System.out.println("最小值：Float.MIN_VALUE=" + Float.MIN_VALUE);  
        System.out.println("最大值：Float.MAX_VALUE=" + Float.MAX_VALUE);  
        System.out.println();  
  
        // double  
        System.out.println("基本类型：double 二进制位数：" + Double.SIZE);  
        System.out.println("包装类：java.lang.Double");  
        System.out.println("最小值：Double.MIN_VALUE=" + Double.MIN_VALUE);  
        System.out.println("最大值：Double.MAX_VALUE=" + Double.MAX_VALUE);  
        System.out.println();  
  
        // char  
        System.out.println("基本类型：char 二进制位数：" + Character.SIZE);  
        System.out.println("包装类：java.lang.Character");  
        // 以数值形式而不是字符形式将Character.MIN_VALUE输出到控制台  
        System.out.println("最小值：Character.MIN_VALUE="  
                + (int) Character.MIN_VALUE);  
        // 以数值形式而不是字符形式将Character.MAX_VALUE输出到控制台  
        System.out.println("最大值：Character.MAX_VALUE="  
                + (int) Character.MAX_VALUE);  
    }  
}
```

输出结果：

```
基本类型：byte 二进制位数：8
包装类：java.lang.Byte
最小值：Byte.MIN_VALUE=-128
最大值：Byte.MAX_VALUE=127

基本类型：short 二进制位数：16
包装类：java.lang.Short
最小值：Short.MIN_VALUE=-32768
最大值：Short.MAX_VALUE=32767

基本类型：int 二进制位数：32
包装类：java.lang.Integer
最小值：Integer.MIN_VALUE=-2147483648
最大值：Integer.MAX_VALUE=2147483647

基本类型：long 二进制位数：64
包装类：java.lang.Long
最小值：Long.MIN_VALUE=-9223372036854775808
最大值：Long.MAX_VALUE=9223372036854775807

基本类型：float 二进制位数：32
包装类：java.lang.Float
最小值：Float.MIN_VALUE=1.4E-45
最大值：Float.MAX_VALUE=3.4028235E38

基本类型：double 二进制位数：64
包装类：java.lang.Double
最小值：Double.MIN_VALUE=4.9E-324
最大值：Double.MAX_VALUE=1.7976931348623157E308

基本类型：char 二进制位数：16
包装类：java.lang.Character
最小值：Character.MIN_VALUE=0
最大值：Character.MAX_VALUE=65535
```

Float和Double的最小值和最大值都是以科学记数法的形式输出的，结尾的"E+数字"表示E之前的数字要乘以10的多少次方。比如3.14E3就是3.14 × 103 =3140，3.14E-3 就是 3.14 x 10-3 =0.00314。

实际上，JAVA中还存在另外一种基本类型 void，它也有对应的包装类 java.lang.Void，不过我们无法直接对它们进行操作。

可以看到byte和short的取值范围比较小，而long的取值范围太大，占用的空间多，基本上int可以满足我们的日常的计算了，而且int也是使用的最多的整型类型了。

在通常情况下，如果[Java](http://lib.csdn.net/base/javaee)中出现了一个整数数字比如35，那么这个数字就是int型的；

如果我们希望它是byte型的，可以在数据后加上[大写](http://wenwen.soso.com/z/Search.e?sp=S大写&ch=w.search.yjjlink&cid=w.search.yjjlink)的B：**35B**，表示它是byte型的；同样的**35S**表示short型，**35L**表示long型的；

表示**int我们可以什么都不用加**，但是如果要表示long型的，就一定要在数据后面加**L**；

#### 浮点型 float double 

float和double是表示浮点型的[数据类型](http://wenwen.soso.com/z/Search.e?sp=S数据类型&ch=w.search.yjjlink&cid=w.search.yjjlink)，他们之间的区别在于他们的精确度不同。

单精度：float 3.402823e+38 ~ 1.401298e-45（e+38表示是乘以10的38次方，同样，e-45表示乘以10的负45次方）

双精度：double 1.797693e+308~ 4.9000000e-324

double型比float型存储范围更大，[精度](http://wenwen.soso.com/z/Search.e?sp=S精度&ch=w.search.yjjlink&cid=w.search.yjjlink)更高；

所以通常的***\*浮点型的数据在不声明的情况下都是double型的\****；如果要表示一个数据是float型的，可以在数据后面加上“**F**”。

浮点型的数据是不能完全精确的，所以有的时候在计算的时候可能会在小数点最后几位出现浮动，这是正常的。

**注意事项：**

①String不是基本数据类型，是引用数据类型，它是java提供的一个类

②**Java基本类型存储在栈中**，**因此它们的存取速度要快于存储在堆中的对应包装类的实例对象**。从Java5.0（1.5）开始，JAVA虚拟机（Java Virtual Machine）可以完成基本类型和它们对应包装类之间的自动转换。因此我们在赋值、参数传递以及数学运算的时候像使用基本类型一样使用它们的包装类，但这并不意味着你可以通过基本类型调用它们的包装类才具有的方法。另外，所有基本类型的包装类都使用了final修饰，因此我们无法继承它们扩展新的类，也无法重写它们的任何方法。

**拓展知识点：**Java是面向对象语言，其概念为一切皆为对象，但基本数据类型算是个例外哦，基本数据类型大多是面向机器底层的类型，它是 **“值”** 而不是一个对象，它存放于**“栈”**中而不是存放于**“堆”**中，但Java一切皆为对象的概念不是说说而已，它为每一个基本数据类型都做了相应的**包装类**，我们日常使用中大多情况下都会使用着这些包装类：


boolean Boolean
char Character
byte Byte
short Short
int Integer
long Long
float Float
double Double
String（字符串）
包装类就是一个对象，它存放于**“堆”**中。

③基本类型的优势：数据存储相对简单，运算效率比较高

​    包装类的优势：有的容易，比如集合的元素必须是对象类型，满足了java一切皆是对象的思想

参考链接：

https://www.runoob.com/java/java-basic-datatypes.html

https://blog.csdn.net/buhuikanjian/article/details/52901104

Java 基本数据类型 - 四类八种 - 韦庆明的文章 - 知乎 https://zhuanlan.zhihu.com/p/25439066

