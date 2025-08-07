<!-- TRANSLATED by md-translate -->
# Common tool classes

Java's core library provides a large number of ready-made classes for us to use. In this section we introduce a few commonly used tool classes.

### Math

As the name suggests, the `Math` class is used for mathematical calculations, and it provides a large number of static methods to facilitate our implementation of mathematical calculations:

Find the absolute value:

```java
Math.abs(-100); // 100
Math.abs(-7.8); // 7.8
```

Takes the maximum or minimum value:

```java
Math.max(100, 99); // 100
Math.min(1.2, 2.3); // 1.2
```

Calculate the x<sup>y</sup>th power:

```java
Math.pow(2, 10); // 2的10次方=1024
```

Compute $\sqrt x$:

```java
Math.sqrt(2); // 1.414...
```

Calculate the e<sup>x</sup>th power:

```java
Math.exp(2); // 7.389...
```

Compute the logarithm with e as the base:

```java
Math.log(4); // 1.386...
```

Calculate logarithms with base 10:

```java
Math.log10(100); // 2
```

Trigonometric functions:

```java
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

Math also provides several mathematical constants:

```java
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5
```

Generate a random number x in the range `0 <= x < 1`:

```java
Math.random(); // 0.53907... 每次都不一样
```

If we want to generate a random number with interval in `[MIN, MAX)`, we can achieve it with the help of `Math.random()`, which is calculated as follows:

```java
// 区间在[MIN, MAX)的随机数
public class Main {
    public static void main(String[] args) {
        double x = Math.random(); // x的范围是[0,1)
        double min = 10;
        double max = 50;
        double y = x * (max - min) + min; // y的范围是[10,50)
        long n = (long) y; // n的范围是[10,50)的整数
        System.out.println(y);
        System.out.println(n);
    }
}
```

Some of you may have noticed that the Java Standard Library also provides a `StrictMath`, which provides almost exactly the same methods as `Math`. The difference between these two classes is that, because floating-point calculations have errors, the results of calculations on different platforms (e.g., x86 and ARM) may not be consistent (meaning that the errors are different), so `StrictMath` guarantees that the results of calculations on all platforms are identical, while `Math` tries to optimize the speed of calculations for the platforms, so that, for the vast majority of cases, the use of `Math` will be is sufficient in most cases.

### HexFormat

When dealing with `byte[]` arrays, we often need to be converted to hexadecimal strings, it is more trouble to write your own, with the Java standard library provides `HexFormat` can be convenient to help us convert.

To convert a `byte[]` array to a hexadecimal string, use the `formatHex()` method:

```java
import java.util.HexFormat;

public class Main {
    public static void main(String[] args) throws InterruptedException {
        byte[] data = "Hello".getBytes();
        HexFormat hf = HexFormat.of();
        String hexData = hf.formatHex(data); // 48656c6c6f
    }
}
```

If you want to customize the conversion format, use a customized `HexFormat` instance:

```java
// 分隔符为空格，添加前缀0x，大写字母:
HexFormat hf = HexFormat.ofDelimiter(" ").withPrefix("0x").withUpperCase();
hf.formatHex("Hello".getBytes())); // 0x48 0x65 0x6C 0x6C 0x6F
```

Convert from a hexadecimal string to a `byte[]` array, using the `parseHex()` method:

```
byte[] bs = HexFormat.of().parseHex("48656c6c6f");
```

### Random

`Random` is used to create pseudo-random numbers. By pseudo-random numbers, we mean that given an initial seed, the sequence of random numbers generated is exactly the same.

To generate a random number, you can use `nextInt()`, `nextLong()`, `nextFloat()`, `nextDouble()`:

```java
Random r = new Random();
r.nextInt(); // 2071575453,每次都不一样
r.nextInt(10); // 5,生成一个[0,10)之间的int
r.nextLong(); // 8811649292570369305,每次都不一样
r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
```

Some children asked, every time you run the program, the random number generated is different, did not see the _pseudo-random number_ characteristics.

This is because when we create a `Random` instance, we use the current system timestamp as the seed if it is not given, so each time we run it, the seed is different and we get a different sequence of pseudo-random numbers.

If we specify a seed when we create an instance of `Random`, we get a completely deterministic sequence of random numbers:

```java
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        Random r = new Random(12345);
        for (int i = 0; i < 10; i++) {
            System.out.println(r.nextInt(100));
        }
        // 51, 80, 41, 28, 55...
    }
}
```

The `Math.random()` we used earlier actually calls the `Random` class internally, so it's also pseudo-random, we just can't specify the seed.

### SecureRandom

There are pseudo-random numbers, and then there are true random numbers. In fact true true random numbers can only be obtained by quantum mechanical principles, and what we want is an unpredictable and secure random number, `SecureRandom` is used to create secure random numbers:

```java
SecureRandom sr = new SecureRandom();
System.out.println(sr.nextInt(100));
```

`SecureRandom` cannot specify a seed, it uses the RNG (random number generator) algorithm.The JDK's `SecureRandom` actually has a number of different underlying implementations, some of which use a secure random seed coupled with a pseudo-random number algorithm to generate secure random numbers, and some of which use a true random number generator. In practice, you can prioritize getting a high-strength secure random number generator, and if one is not provided, then you can use a normal level secure random number generator:

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;

public class Main {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}
```

`SecureRandom` is secured by generating random numbers from a secure random seed provided by the operating system. This seed is the "entropy" generated by various random events such as CPU thermal noise, bytes read and written to disk, network traffic, and so on.

In cryptography, secure random numbers are very important. If insecure pseudo-random numbers are used, all cryptographic systems will be broken. Therefore, always keep in mind that `SecureRandom` must be used to generate secure random numbers.

```alert type=caution title=注意
需要使用安全随机数的时候，必须使用SecureRandom，绝不能使用Random！
```

### Summary

The common tool classes provided by Java are:

* Math: math calculation
* HexFormat: format hexadecimal numbers
* Random: generate pseudo-random numbers
* SecureRandom: generate secure random numbers
