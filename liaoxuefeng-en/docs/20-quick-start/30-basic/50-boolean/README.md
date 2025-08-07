<!-- TRANSLATED by md-translate -->
# Boolean

For the boolean type `boolean`, there are always only two values, `true` and `false`.

Boolean operations are relational operations that include the following categories:

* Comparison operators: `>`, `>=`, `<`, `<=`, `==`, `! =`
* The with operation `&&`
* Or operations `||`
* Non-operations `! `

Here are some examples:

```java
boolean isGreater = 5 > 3; // true
int age = 12;
boolean isZero = age == 0; // false
boolean isNonZero = !isZero; // true
boolean isAdult = age >= 18; // false
boolean isTeenager = age >6 && age <18; // true
```

Relational operators are prioritized in descending order:

* `! `
* `>`, `>=`, `<`, `<=`
* `==`, `! =`
* `&&`
* `||`

### Short-circuit arithmetic ###

An important feature of Boolean operations is short-circuiting. If a Boolean expression determines the result in advance, subsequent calculations are no longer performed and the result is returned directly.

Because `false && x` always results in `false`, regardless of whether `x` is `true` or `false`, the with operation, after determining that the first value is `false`, does not continue the computation, but simply returns `false`.

We examine the following code:

```java
// 短路运算
public class Main {
    public static void main(String[] args) {
        boolean b = 5 < 3;
        boolean result = b && (5 / 0 > 0); // 此处 5 / 0 不会报错
        System.out.println(result);
    }
}
```

Without the short-circuit operation, the expression following `&&` would report an error due to a division of `0`, but in fact the statement does not report an error because the with operation is a short-circuit operator that calculates the result `false` in advance.

If the value of the variable `b` is `true`, the expression becomes `true && (5 / 0 > 0)`. Since short-circuiting is not possible, this expression is bound to report an error due to a divisor of `0`, so you can test it yourself.

Similarly, for the `||` operation, as soon as the first value can be determined to be `true`, subsequent calculations are no longer performed and `true` is returned directly:

```java
boolean result = true || (5 / 0 > 0); // true
```

### Ternary operators

Java also provides a ternary operator `b ? x : y`, which returns the result of the computation of one of the two subsequent expressions, depending on the result of the first Boolean expression. Example:

```java
// 三元运算
public class Main {
    public static void main(String[] args) {
        int n = -100;
        int x = n >= 0 ? n : -n;
        System.out.println(x);
    }
}
```

The above statement means to determine whether `n >= 0` holds, and return `n` if it is `true`, otherwise return `-n`. This is actually an expression to find the absolute value.

Notice that the ternary operation `b ? x : y` will first compute `b`, and if `b` is `true`, then only `x` is computed, otherwise, only `y` is computed. Also, the types of `x` and `y` must be the same, since the return value is not `boolean`, but one of `x` and `y`.

### Exercise

Determine if the specified age is an elementary school student (6 to 12 years old):

```java
// 布尔运算
public class Main {
    public static void main(String[] args) {
        int age = 7;
        // primary student的定义: 6~12岁
        boolean isPrimaryStudent = ???;
        System.out.println(isPrimaryStudent ? "Yes" : "No");
    }
}
```

[Download exercise](basic-boolean.zip)

### Summary

The operations with and or are short-circuiting operations;

The ternary operation `b ? x : y` must be followed by the same type, and the ternary operation is also a "short-circuit operation", which computes only `x` or `y`.