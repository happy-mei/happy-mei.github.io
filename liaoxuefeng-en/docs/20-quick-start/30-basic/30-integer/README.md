<!-- TRANSLATED by md-translate -->
# Integer operations

Java's integer arithmetic follows the rule of four and can use any nested parentheses. The rule of four is consistent with elementary math. For example:

```java
// 四则运算
public class Main {
    public static void main(String[] args) {
        int i = (100 + 200) * (99 - 88); // 3300
        int n = 7 * (5 + (i - 9)); // 23072
        System.out.println(i);
        System.out.println(n);
    }
}
```

The numerical representation of integers is not only exact, but integer operations are always exact, even in division, since dividing two integers yields only the integer part of the result:

```java
int x = 12345 / 67; // 184
```

Use `%` for the remainder operation:

```java
int y = 12345 % 67; // 12345÷67的余数是17
```

Special note: Integer division will report an error at runtime when the divisor is 0, but will not report an error when compiled.

### Overflow

Pay special attention to the fact that integers, due to the existence of a range limit, will overflow if the result of the calculation is out of range, and the overflow _will not be an error_, but will give a strange result:

```java
// 运算溢出
public class Main {
    public static void main(String[] args) {
        int x = 2147483640;
        int y = 15;
        int sum = x + y;
        System.out.println(sum); // -2147483641
    }
}
```

To interpret the above result, we convert the integers `2147483640` and `15` into binary to do the addition:

```ascii
0111 1111 1111 1111 1111 1111 1111 1000
+ 0000 0000 0000 0000 0000 0000 0000 1111
-----------------------------------------
  1000 0000 0000 0000 0000 0000 0000 0111
```

Since the highest bit is calculated as `1`, the result of the addition becomes a negative number.

To solve the above problem, you can replace `int` with `long` type, and since `long` can represent a larger range of integers, the result will not overflow:

```java
long x = 2147483640;
long y = 15;
long sum = x + y;
System.out.println(sum); // 2147483655
```

There is also a shorthand operator, `+=`, `-=`, `*=`, `/=`, which are used as follows:

```java
n += 100; // 3409, 相当于 n = n + 100;
n -= 100; // 3309, 相当于 n = n - 100;
```

### Self-increasing/decreasing

Java also provides the `++` operation and the `--` operation, which can add 1 and subtract 1 to an integer:

```java
// 自增/自减运算
public class Main {
    public static void main(String[] args) {
        int n = 3300;
        n++; // 3301, 相当于 n = n + 1;
        n--; // 3300, 相当于 n = n - 1;
        int y = 100 + (++n); // 不要这么写
        System.out.println(y);
    }
}
```

Note that `++` written before and after the calculation results are different, `++n` means first add 1 and then refer to n, `n++` means first refer to n and then add 1. It is not recommended to mix the `++` operation into the regular operation, it is easy to confuse yourself.

### Shift operations

In computers, integers are always represented in binary form. For example, an integer `7` of type `int` is represented in binary using 4 bytes as follows:

```ascii
00000000 0000000 0000000 00000111
```

It is possible to perform shift operations on integers. Shifting an integer `7` by one place gives an integer `14` and shifting it by two places gives an integer `28`:

```java
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n << 1;  // 00000000 00000000 00000000 00001110 = 14
int b = n << 2;  // 00000000 00000000 00000000 00011100 = 28
int c = n << 28; // 01110000 00000000 00000000 00000000 = 1879048192
int d = n << 29; // 11100000 00000000 00000000 00000000 = -536870912
```

On shifting left by 29 bits, the result becomes negative as the highest bit becomes `1`.

Similarly, a right shift of the integer 28 results in the following:

```java
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n >> 1;  // 00000000 00000000 00000000 00000011 = 3
int b = n >> 2;  // 00000000 00000000 00000000 00000001 = 1
int c = n >> 3;  // 00000000 00000000 00000000 00000000 = 0
```

If a negative number is shifted right and the `1` in the highest bit does not move, the result is still a negative number:

```java
int n = -536870912;
int a = n >> 1;  // 11110000 00000000 00000000 00000000 = -268435456
int b = n >> 2;  // 11111000 00000000 00000000 00000000 = -134217728
int c = n >> 28; // 11111111 11111111 11111111 11111110 = -2
int d = n >> 29; // 11111111 11111111 11111111 11111111 = -1
```

There is also an unsigned right-shift operation, using `>>>`, which is characterized by the fact that, regardless of the sign bit, the higher bits are always complemented by `0` after the right-shift, so a `>>>` right-shift of a negative number will turn it into a positive number, due to the fact that the `1` in the highest bit becomes a `0`:

```java
int n = -536870912;
int a = n >>> 1;  // 01110000 00000000 00000000 00000000 = 1879048192
int b = n >>> 2;  // 00111000 00000000 00000000 00000000 = 939524096
int c = n >>> 29; // 00000000 00000000 00000000 00000111 = 7
int d = n >>> 31; // 00000000 00000000 00000000 00000001 = 1
```

When shifting `byte` and `short` types, they are first converted to `int` before being shifted.

A closer look reveals that a left shift is actually a constant ×2 and a right shift is actually a constant ÷2.

### Bitwise arithmetic

Bitwise operations are operations of and, or, not and different or by bit. Let's first look at bitwise operations for a single bit.

The rule with operations is that both numbers must be `1` at the same time for the result to be `1`:

```java
n = 0 & 0; // 0
n = 0 & 1; // 0
n = 1 & 0; // 0
n = 1 & 1; // 1
```

The rule for the or operation is that whenever either is `1`, the result is `1`:

```java
n = 0 | 0; // 0
n = 0 | 1; // 1
n = 1 | 0; // 1
n = 1 | 1; // 1
```

The rule for non-arithmetic is that `0` and `1` are interchanged:

```java
n = ~0; // 1
n = ~1; // 0
```

The rule for the different-or operation is that if two numbers are different, the result is `1`, otherwise `0`:

```java
n = 0 ^ 0; // 0
n = 0 ^ 1; // 1
n = 1 ^ 0; // 1
n = 1 ^ 1; // 0
```

Java does not have a single bit data type. In Java, bitwise operations on two integers are actually aligned by bit and then performed on each bit in turn. Example:

```java
// 位运算
public class Main {
    public static void main(String[] args) {
        int i = 167776589; // 00001010 00000000 00010001 01001101
        int n = 167776512; // 00001010 00000000 00010001 00000000
                         // & -----------------------------------
                           // 00001010 00000000 00010001 00000000
        System.out.println(i & n); // 167776512
    }
}
```

The above bitwise sum operation can actually be viewed as two integer representations of the IP addresses `10.0.17.77` and `10.0.17.0`, and by using the sum operation, it is possible to quickly determine whether or not an IP is within a given network segment.

### Operational priority

In Java's computational expressions, operations are prioritized in descending order:

* `()`
* `! ` `~` `++` `--`
* `*` `/` `%`
* `+` `-`
* `<<` `>>>` `>>>>>`
* `&`
* `|`
* `+=` `-=` `*=` `/=`

It doesn't matter if you can't remember, just add the parentheses to make sure the operation is prioritized correctly.

### Type auto-raising and forced transformation

During an operation, if the two numbers involved are of different types, the result is an integer of the larger type. For example, if `short` and `int` are computed, the result is always `int`, because `short` is automatically transformed to `int` first:

```java
// 类型自动提升与强制转型
public class Main {
    public static void main(String[] args) {
        short s = 1234;
        int i = 123456;
        int x = s + i; // s自动转型为int
        short y = s + i; // 编译错误!
    }
}
```

It is also possible to force a transformation of the result, i.e., to transform a large range of integers into a small range of integers. Forced transformations use `(type)`, e.g., forcing an `int` into a `short`:

```java
int i = 12345;
short s = (short) i; // 12345
```

Be aware that out-of-range forced transformations give incorrect results due to the fact that the two high bytes of `int` are simply thrown away during the transformation, retaining only the two low bytes:

```java
// 强制转型
public class Main {
    public static void main(String[] args) {
        int i1 = 1234567;
        short s1 = (short) i1; // -10617
        System.out.println(s1);
        int i2 = 12345678;
        short s2 = (short) i2; // 24910
        System.out.println(s2);
    }
}
```

Therefore, the result of forced transition is likely to be wrong.

### Exercise

Calculating the sum of the first N natural numbers can be based on the formula:

```math
\frac{(1+N)\times N}2
```

Calculate the sum of the first N natural numbers according to the formula:

```java
// 计算前N个自然数的和
public class Main {
    public static void main(String[] args) {
        int n = 100;
        // TODO: sum = 1 + 2 + ... + n
        int sum = ???;
        System.out.println(sum);
        System.out.println(sum == 5050 ? "测试通过" : "测试失败");
    }
}
```

[Download exercise](basic-integer.zip)

### Summary

The result of integer arithmetic is always exact;

The result of the operation is automatically boosted;

It is possible to force transformations, but forcing transformations out of scope will give the wrong results;

The appropriate range of integers (`int` or `long`) should be chosen, and there is no need to use `byte` and `short` for integer arithmetic in order to save memory.