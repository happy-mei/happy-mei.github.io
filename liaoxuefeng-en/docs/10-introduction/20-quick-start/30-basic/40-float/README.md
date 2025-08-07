<!-- TRANSLATED by md-translate -->
# Floating point arithmetic

Floating-point operations, compared with integer operations, can only perform addition, subtraction, multiplication, and division of these numerical calculations, and cannot do bitwise and shift operations.

In computers, although floating point numbers have a large range of representations, they have a very important characteristic, which is that they often cannot be represented exactly.

An example:

The floating-point number `0.1` cannot be represented exactly in a computer because `0.1` in decimal is an infinitely-circular decimal when converted to binary, and it is clear that only an approximation of `0.1` can be stored, whether `float` or `double` is used. However, `0.5`, a floating point number, can again be represented exactly.

Because floating-point numbers often cannot be represented exactly, floating-point arithmetic is subject to error:

```java
// 浮点数运算误差
public class Main {
    public static void main(String[] args) {
        double x = 1.0 / 10;
        double y = 1 - 9.0 / 10;
        // 观察x和y是否相等:
        System.out.println(x);
        System.out.println(y);
    }
}
```

Comparing two floating-point numbers for equality often yields incorrect results because of arithmetic errors in floating-point numbers. The correct method of comparison is to determine whether the absolute value of the difference between two floating-point numbers is less than a very small number:

```java
// 比较x和y是否相等，先计算其差的绝对值:
double r = Math.abs(x - y);
// 再判断绝对值是否足够小:
if (r < 0.00001) {
    // 可以认为相等
} else {
    // 不相等
}
```

The representation of floating-point numbers in memory is more complex than that of integers. Java's floating-point numbers fully follow the [IEEE-754](https://standards.ieee.org/ieee/754/6210/) standard, which is the standard representation of floating-point numbers supported by the vast majority of computer platforms.

### Type Lifting

If one of the two numbers involved in the operation is an integer, then the integer can be automatically promoted to a floating-point type:

```java
// 类型提升
public class Main {
    public static void main(String[] args) {
        int n = 5;
        double d = 1.2 + 24.0 / n; // 6.0
        System.out.println(d);
    }
}
```

Special attention needs to be paid to the fact that in a complex quadratic operation, the operation of two integers does not appear to be automatically lifted. Example:

```java
double d = 1.2 + 24 / 5; // 结果不是 6.0 而是 5.2
```

The reason the result is `5.2` is that the compiler calculates the sub-expression `24 / 5` as two integers, and the result is still the integer `4`.

To fix this calculation, change `24 / 5` to `24.0 / 5`. Since `24.0` is a floating point number, the calculation of division automatically raises `5` to a floating point number.

### Overflow

Integer operations report an error when the divisor is `0`, while floating-point operations do not report an error when the divisor is `0`, but return several special values:

* `NaN` means Not a Number
* `Infinity` means infinity.
* `-Infinity` means negative infinity.

Example:

```java
double d1 = 0.0 / 0; // NaN
double d2 = 1.0 / 0; // Infinity
double d3 = -1.0 / 0; // -Infinity
```

These three special values are seldom encountered in actual arithmetic, we just need to understand them.

### Forced transition

It is possible to force the transformation of a floating point number to an integer. When transforming, the fractional part of the floating point number is discarded. If the transformation exceeds the maximum range that the integer can represent, the maximum value of the integer is returned. Example:

```java
int n1 = (int) 12.3; // 12
int n2 = (int) 12.7; // 12
int n3 = (int) -12.7; // -12
int n4 = (int) (12.7 + 0.5); // 13
int n5 = (int) 1.2e20; // 2147483647
```

If you want to round, you can add `0.5` to the floating point number and then force the transformation:

```java
// 四舍五入
public class Main {
    public static void main(String[] args) {
        double d = 2.6;
        int n = (int) (d + 0.5);
        System.out.println(n);
    }
}
```

### Exercise

Based on the root formula for the quadratic equation $ax^2+bx+c=0$:

```math
\frac{\displaystyle-b\pm\sqrt{b^2-4ac}}{\displaystyle2a}
```

Calculate two solutions to a quadratic equation:

```java
// 一元二次方程
public class Main {
    public static void main(String[] args) {
        double a = 1.0;
        double b = 3.0;
        double c = -4.0;
        // 求平方根可用 Math.sqrt():
        // System.out.println(Math.sqrt(2)); ==> 1.414
        // TODO:
        double r1 = 0;
        double r2 = 0;
        System.out.println(r1);
        System.out.println(r2);
        System.out.println(r1 == 1 && r2 == -4 ? "测试通过" : "测试失败");
    }
}
```

[Download exercise](basic-float.zip)

### Summary

Floating-point numbers are often impossible to represent precisely, and the results of floating-point arithmetic can be inaccurate;

Comparing two floating-point numbers usually compares whether the absolute value of their difference is less than a specific value;

Integer and floating-point operations are automatically promoted to floating-point when integer is used;

It is possible to force a floating-point type to an integer, but out of range will always return the maximum value of the integer.