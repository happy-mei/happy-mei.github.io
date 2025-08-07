<!-- TRANSLATED by md-translate -->
# Variables and data types

### Variables

What is a variable?

A variable is the concept of algebra in middle school math, such as a simple equation where x, y are variables:

```math
y=x^2+1
```

In Java, there are two types of variables: variables of basic types and variables of reference types.

We start by discussing variables of basic types.

In Java, variables must be defined before they can be used, and when defining a variable, you can give it an initial value. Example:

```java
int x = 1;
```

The above statement defines a variable of type integer `int` with name `x` and initial value `1`.

Not writing an initial value is equivalent to assigning it a default value. The default value is always `0`.

Come see a complete example of defining a variable and then printing the value of the variable:

```java
// 定义并打印变量
public class Main {
    public static void main(String[] args) {
        int x = 100; // 定义int类型变量x，并赋予初始值100
        System.out.println(x); // 打印该变量的值
    }
}
```

An important feature of variables is that they can be reassigned. For example, for the variable `x`, assign the value `100` and then `200`, and observe the results of the two printouts:

```java
// 重新赋值变量
public class Main {
    public static void main(String[] args) {
        int x = 100; // 定义int类型变量x，并赋予初始值100
        System.out.println(x); // 打印该变量的值，观察是否为100
        x = 200; // 重新赋值为200
        System.out.println(x); // 打印该变量的值，观察是否为200
    }
}
```

Notice that the first time the variable `x` is defined, the variable type `int` needs to be specified, so the statement `int x = 100;` is used. The second time the variable `x` is reassigned, the variable `x` already exists and cannot be redefined, so the variable type `int` cannot be specified, and the statement `x = 200;` must be used.

Variables can not only be reassigned, but they can also be assigned to other variables. Let's look at an example:

```java
// 变量之间的赋值
public class Main {
    public static void main(String[] args) {
        int n = 100; // 定义变量n，同时赋值为100
        System.out.println("n = " + n); // 打印n的值

        n = 200; // 变量n赋值为200
        System.out.println("n = " + n); // 打印n的值

        int x = n; // 变量x赋值为n（n的值为200，因此赋值后x的值也是200）
        System.out.println("x = " + x); // 打印x的值

        x = x + 100; // 变量x赋值为x+100（x的值为200，因此赋值后x的值是200+100=300）
        System.out.println("x = " + x); // 打印x的值
        System.out.println("n = " + n); // 再次打印n的值，n应该是200还是300？
   }
}
```

We analyze the code execution flow line by line:

Execute `int n = 100;`, which defines the variable `n` and assigns the value `100`, so the JVM allocates a `storage unit` in memory for the variable `n`, filled with the value `100`:

```ascii
n
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │100│   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

When executing `n = 200;`, the JVM writes `200` to the storage unit of the variable `n`, so the original value is overwritten and the value of `n` is now `200`:

```ascii
n
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

Execution of `int x = n;` defines a new variable `x` and assigns a value to `x` at the same time, so the JVM needs to _newly_ allocate _a_ storage unit to the variable `x` and write the same value as the variable `n`, with the result that the variable `x`'s value also changes to `200`:

```ascii
n x
      │           │
      ▼           ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │200│   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

When executing `x = x + 100;`, the JVM first calculates the value `x + 100` on the right-hand side of the equation, which results in `300` (since the value of `x` is `200` at this moment), and then, it writes the result `300` to the storage unit of `x`, so that the final value of the variable `x` becomes `300`:

```ascii
n x
      │           │
      ▼           ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │200│   │   │300│   │   │
└───┴───┴───┴───┴───┴───┴───┘
```

As you can see, variables can be assigned repeatedly. Note that the equals sign `=` is an assignment statement, not equal in the mathematical sense, otherwise it would be impossible to explain `x = x + 100`.

### Basic data types

Basic data types are types that the CPU can perform operations on directly.Java defines the following basic data types:

* Integer types: byte, short, int, long
* Floating point types: float, double
* Character type: char
* Boolean type: boolean

What are the differences between these basic data types defined by Java? To understand these differences, we must briefly understand the basic structure of computer memory.

The smallest storage unit of computer memory is a byte, a byte is an 8-bit binary number, i.e., 8 bits.Its binary representation ranges from `00000000` to `11111111`, which translates into decimal 0 to 255, and into hexadecimal `00` to `ff`.

Memory cells are numbered from 0 and are called memory addresses. Each memory cell can be thought of as a room, and the memory address is the door number.

```ascii
0 1 2 3 4 5 6  ...
┌───┬───┬───┬───┬───┬───┬───┐
│   │   │   │   │   │   │   │...
└───┴───┴───┴───┴───┴───┴───┘
```

A byte is 1byte, 1024 bytes is 1K, 1024K is 1M, 1024M is 1G, 1024G is 1T. the number of bytes in a computer with 4T of RAM is:

```plain
4T = 4 x 1024G
   = 4 x 1024 x 1024M
   = 4 x 1024 x 1024 x 1024K
   = 4 x 1024 x 1024 x 1024 x 1024
   = 4398046511104
```

Different data types occupy different numbers of bytes. Let's look at the number of bytes occupied by the Java basic data types:

```ascii
┌───┐
  byte │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
```

A `byte` is exactly one byte, while `long` and `double` take 8 bytes.

### Integer

For integer types, Java defines only signed integers, so the highest bit indicates the sign bit (0 for positive numbers, 1 for negative numbers). The maximum range that various integer types can represent is as follows:

* byte: -128 ~ 127
* short: -32768 ~ 32767
* int: -2147483648 ~ 2147483647
* long: -9223372036854775808 ~ 9223372036854775807

Let's look at the example of defining an integer:

```java
// 定义整型
public class Main {
    public static void main(String[] args) {
        int i = 2147483647;
        int i2 = -2147483648;
        int i3 = 2_000_000_000; // 加下划线更容易识别
        int i4 = 0xff0000; // 十六进制表示的16711680
        int i5 = 0b1000000000; // 二进制表示的512

        long n1 = 9000000000000000000L; // long型的结尾需要加L
        long n2 = 900; // 没有加L，此处900为int，但int类型可以赋值给long
        int i6 = 900L; // 错误：不能把long型赋值给int
    }
}
```

Special note: The representation of the same number in the different alphabets is identical, e.g. `15` = `0xf` = `0b1111`.

### Floating point

Floating-point numbers are decimals because when decimals are expressed in scientific notation, the decimal point can "float", e.g., 1234.5 can be expressed as 12.345x10<sup>2</sup>, or 1.2345x10<sup>3</sup>, and are therefore called floating point numbers.

The following is an example of defining a floating point number:

```java
float f1 = 3.14f;
float f2 = 3.14e38f; // 科学计数法表示的3.14x10^38
float f3 = 1.0; // 错误：不带f结尾的是double类型，不能赋值给float

double d = 1.79e308;
double d2 = -1.79e308;
double d3 = 4.9e-324; // 科学计数法表示的4.9x10^-324
```

For `float` types, the `f` suffix is required.

Floating point numbers can be represented in a very large range, with `float` types up to 3.4x10<sup>38</sup> and `double` types up to 1.79x10<sup>308</sup>.

### Boolean type

The boolean type `boolean` has only two values, `true` and `false`, and booleans are always the result of a relational operation:

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

The Java language does not make provisions for storing boolean types, since theoretically only 1 bit is needed to store a boolean type, but usually the JVM internally represents `boolean` as a 4-byte integer.

### Character types

The character type `char` represents a character.Java's `char` type can represent a Unicode character in addition to standard ASCII:

```java
// 字符类型
public class Main {
    public static void main(String[] args) {
        char a = 'A';
        char zh = '中';
        System.out.println(a);
        System.out.println(zh);
    }
}
```

Note that the `char` type uses single quotes `'` and only one character, to separate it from the double-quoted `"` string type.

### Reference types

Apart from the basic types of variables mentioned above, the rest are reference types. For example, the most commonly used reference type is the `String` string:

```java
String s = "hello";
```

Variables of the reference type are similar to pointers in C. They internally store an "address" that points to the location of an object in memory, which will be discussed in more detail later when we introduce the concept of classes.

### Constants

When defining a variable with the `final` modifier, the variable becomes a constant:

```java
final double PI = 3.14; // PI是一个常量
double r = 5.0;
double area = PI * r * r;
PI = 300; // compile error!
```

Constants cannot be re-assigned after initialization at definition time, and re-assignment will result in a compilation error.

Constants are useful for avoiding Magic numbers with meaningful variable names, e.g., instead of writing `3.14` all over the code, define a constant. If in the future we need to improve the precision of a calculation, we only need to change the definition of the constant, for example, to `3.1416`, instead of replacing `3.14` everywhere.

To distinguish them from variables, by convention, constant names are usually **all capitalized**.

### The var keyword

There are times when the name of a type is too long and is more cumbersome to write. For example:

```java
StringBuilder sb = new StringBuilder();
```

This time, if you want to omit the variable type, you can use the `var` keyword:

```java
var sb = new StringBuilder();
```

The compiler automatically infers that the variable `sb` is of type `StringBuilder` based on the assignment statement. statement to the compiler:

```java
var sb = new StringBuilder();
```

It will actually become automatic:

```java
StringBuilder sb = new StringBuilder();
```

Thus, defining a variable using `var` is simply writing less of the variable type.

### The scope of a variable

In Java, multi-line statements are enclosed in `{ ... }`. Many control statements, such as conditional judgments and loops, have `{ ... }` as their own scope, for example:

```java
if (...) { // if开始
    ...
    while (...) { // while 开始
        ...
        if (...) { // if开始
            ...
        } // if结束
        ...
    } // while结束
    ...
} // if结束
```

As long as these `{ ... }`, the compiler recognizes the beginning and end of the statement block. Variables that are defined within a statement block have a scope that begins at the point of definition and ends at the end of the statement block. Referring to these variables out of scope, the compiler will report an error. An example:

```java
{
    ...
    int i = 0; // 变量i从这里开始定义
    ...
    {
        ...
        int x = 1; // 变量x从这里开始定义
        ...
        {
            ...
            String s = "hello"; // 变量s从这里开始定义
            ...
        } // 变量s作用域到此结束
        ...
        // 注意，这是一个新的变量s，它和上面的变量同名，
        // 但是因为作用域不同，它们是两个不同的变量:
        String s = "hi";
        ...
    } // 变量x和s作用域到此结束
    ...
} // 变量i作用域到此结束
```

When defining variables, follow the principle of scope minimization, try to define variables in the smallest possible scope, and, do not reuse variable names.

### Summary

Java provides two types of variables: basic types and reference types

Basic types include integer, floating point, boolean, and character.

Variables can be reassigned, and the equal sign is an assignment statement, not an equal sign in the mathematical sense.

Constants cannot be reassigned after initialization, and their use facilitates understanding of program intent.