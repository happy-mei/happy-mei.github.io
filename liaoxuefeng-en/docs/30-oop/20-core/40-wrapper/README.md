<!-- TRANSLATED by md-translate -->
# Package type

We already know that there are two types of data types in Java:

* Basic types: `byte`, `short`, `int`, `long`, `boolean`, `float`, `double`, `char`;
* Reference types: all `class` and `interface` types.

Reference types can be assigned `null` for null, but basic types cannot be assigned `null`:

```java
String s = null;
int n = null; // compile error!
```

So how do you consider a basic type to be an object (a reference type)?

For example, to turn the `int` base type into a reference type, we can define an `Integer` class that contains only one instance field, `int`, so that the `Integer` class can be regarded as a Wrapper Class for `int`:

```java
public class Integer {
    private int value;

    public Integer(int value) {
        this.value = value;
    }

    public int intValue() {
        return this.value;
    }
}
```

With the `Integer` class defined, we can convert `int` and `Integer` to each other:

```java
Integer n = null;
Integer n2 = new Integer(99);
int n3 = n2.intValue();
```

In fact, because wrapper types are so useful, the Java core library provides a corresponding wrapper type for each basic type:

| Basic Types | Corresponding Reference Types |
|:--------|:--------------------|
| boolean | java.lang.
| byte | java.lang.
| short | java.lang.
Byte | short | java.lang.Short | int | java.lang.
| long | java.lang.
| float | java.lang.
| double | java.lang.
| char | java.lang.

We can use it directly and don't need to define it ourselves:

```java
// Integer:
public class Main {
    public static void main(String[] args) {
        int i = 100;
        // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
        Integer n1 = new Integer(i);
        // 通过静态方法valueOf(int)创建Integer实例:
        Integer n2 = Integer.valueOf(i);
        // 通过静态方法valueOf(String)创建Integer实例:
        Integer n3 = Integer.valueOf("100");
        System.out.println(n3.intValue());
    }
}
```

### Auto Boxing

Because `int` and `Integer` can be converted to each other:

```java
int i = 100;
Integer n = Integer.valueOf(i);
int x = n.intValue();
```

So, the Java compiler can help us automatically transform between `int` and `Integer`:

```java
Integer n = 100; // 编译器自动使用Integer.valueOf(int)
int x = n; // 编译器自动使用Integer.intValue()
```

This way of writing assignments that directly change `int` to `Integer` is called Auto Boxing, and conversely, the way of writing assignments that change `Integer` to `int` is called Auto Unboxing.

> [!NOTICE]注意
>
> 自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码。

Boxing and unboxing affects the efficiency of code execution because compiled `class` code strictly distinguishes between basic types and reference types. Moreover, automatic unboxing execution may report `NullPointerException`:

```java
// NullPointerException
public class Main {
    public static void main(String[] args) {
        Integer n = null;
        int i = n;
    }
}
```

### No-change class

All wrapper types are invariant classes. As we can see by looking at the source code of `Integer`, its core code is as follows:

```java
public final class Integer {
    private final int value;
}
```

Thus, once an `Integer` object has been created, the object is invariant.

Special attention should be paid to comparing two `Integer` instances: never use `==` to compare, because `Integer` is a reference type and must be compared using `equals()`:

```java
// == or equals?
public class Main {
    public static void main(String[] args) {
        Integer x = 127;
        Integer y = 127;
        Integer m = 99999;
        Integer n = 99999;
        System.out.println("x == y: " + (x==y)); // true
        System.out.println("m == n: " + (m==n)); // false
        System.out.println("x.equals(y): " + x.equals(y)); // true
        System.out.println("m.equals(n): " + m.equals(n)); // true
    }
}
```

If you look at the result carefully, you can see that `==` comparison, the smaller two same `Integer` return `true`, the larger two same `Integer` return `false`, this is because `Integer` is an invariant class, the compiler change `Integer x = 127;` automatically to `Integer x = Integer.valueOf(127);`, and to save memory, `Integer.valueOf()` always returns the same instance for smaller numbers, so the `==` comparison "just happens" to be `true`, but we _mustn't_ because the Java Standard Library's ` Integer` has internal cache optimizations, we must use the `equals()` method to compare two `Integers`.

> [!TIP]最佳实践
>
> 按照语义编程，而不是针对特定的底层实现去“优化”。

Since `Integer.valueOf()` may always return the same `Integer` instance, here are two ways to do it when we create `Integer` ourselves:

* Method 1: `Integer n = new Integer(100);`
* Method 2: `Integer n = Integer.valueOf(100);`

Method 2 is better because method 1 always creates a new instance of `Integer`, and method 2 leaves the internal optimization to the implementer of `Integer`, and even if it is not optimized in the current version, it may be optimized in the next version.

We call static methods that create "new" objects static factory methods. An example of a static factory method is `Integer.valueOf()`, which returns as many cached instances as possible to save memory.

> [!TIP]最佳实践
>
> 创建新对象时，优先选用静态工厂方法而不是new操作符。

If we examine the source code of the `Byte.valueOf()` method, we can see that all the instances of `Byte` returned by the standard library are cached instances, but the caller doesn't care whether the static factory method creates a new instance in any way or returns the cached instance directly.

### Conversion

The `Integer` class itself also provides a large number of methods, for example, the most commonly used static method `parseInt()` parses a string into an integer:

```java
int x1 = Integer.parseInt("100"); // 100
int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析
```

`Integer` can also format an integer into a string of the specified binary:

```java
// Integer:
public class Main {
    public static void main(String[] args) {
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}
```

Note: The output of all of the above methods is `String`, which is represented in computer memory only in binary; no decimal or hexadecimal representation exists. `int n = 100` is always represented in memory in 4-byte binary:

```ascii
┌────────┬────────┬────────┬────────┐
│00000000│00000000│00000000│01100100│
└────────┴────────┴────────┴────────┘
```

We often use `System.out.println(n);` which relies on the core library to automatically format integers into decimal for output and display on the screen, and use `Integer.toHexString(n)` to automatically format integers into hexadecimal via the core library.

Here we note an important principle of program design: the storage and display of data should be separated.

Java's wrapper types also define some useful static variables

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

Finally, all wrapper types for integers and floats inherit from `Number`, so it is very easy to get the various basic types directly from the wrapper types:

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

### Handling unsigned integers

In Java, there is no unsigned integer (Unsigned) basic data type. `byte`, `short`, `int`, and `long` are all signed integers, with the highest bit being the sign bit. C, on the other hand, provides all the data types supported by the CPU, including unsigned integers. Conversion between unsigned and signed integers is done in Java with the help of static methods of wrapper types.

For example, byte is a signed integer with a range of `-128` to `+127`, but if `byte` is considered as an unsigned integer, its range is `0` to `255`. We convert a negative `byte` to `int` by unsigned integer:

```java
// Byte
public class Main {
    public static void main(String[] args) {
        byte x = -1;
        byte y = 127;
        System.out.println(Byte.toUnsignedInt(x)); // 255
        System.out.println(Byte.toUnsignedInt(y)); // 127
    }
}
```

Since the binary representation of `-1` of a `byte` is `11111111`, an `int` converted as an unsigned integer is `255`.

Similarly, it is possible to convert a `short` to an `int` by unsigned and an `int` to a `long` by unsigned.

### Summary

The Java core library provides wrapper types that can wrap basic types as `class`;

Auto-boxing and auto-unboxing are both done at compile time (JDK>=1.5);

Loading and unloading boxes affects execution efficiency and a `NullPointerException` may occur when unloading boxes;

Comparisons of wrapper types must use `equals()`;

The wrapper types for both integers and floats inherit from `Number`;

Packaging types provide a number of practical methods.