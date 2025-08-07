<!-- TRANSLATED by md-translate -->
# BigDecimal

Similar to `BigInteger`, `BigDecimal` can represent a floating-point number of any size with exactly the right precision.

```
BigDecimal bd = new BigDecimal("123.4567");
System.out.println(bd.multiply(bd)); // 15241.55677489
```

`BigDecimal` uses `scale()` to indicate the number of decimal places, for example:

```
BigDecimal d1 = new BigDecimal("123.45");
BigDecimal d2 = new BigDecimal("123.4500");
BigDecimal d3 = new BigDecimal("1234500");
System.out.println(d1.scale()); // 2,两位小数
System.out.println(d2.scale()); // 4
System.out.println(d3.scale()); // 0
```

With the `stripTrailingZeros()` method of `BigDecimal`, it is possible to format a `BigDecimal` into an equal, but stripped of the trailing zeros, `BigDecimal`:

```
BigDecimal d1 = new BigDecimal("123.4500");
BigDecimal d2 = d1.stripTrailingZeros();
System.out.println(d1.scale()); // 4
System.out.println(d2.scale()); // 2,因为去掉了00

BigDecimal d3 = new BigDecimal("1234500");
BigDecimal d4 = d3.stripTrailingZeros();
System.out.println(d3.scale()); // 0
System.out.println(d4.scale()); // -2
```

If the `scale()` of a `BigDecimal` returns a negative number, e.g., `-2`, it means that the number is an integer and has 2 zeros at the end.

It is possible to set its `scale` for a `BigDecimal`, and if the precision is lower than the original value, then round or just truncate as specified:

```x-java
import java.math.BigDecimal;
import java.math.RoundingMode;
----
public class Main {
    public static void main(String[] args) {
        BigDecimal d1 = new BigDecimal("123.456789");
        BigDecimal d2 = d1.setScale(4, RoundingMode.HALF_UP); // 四舍五入，123.4568
        BigDecimal d3 = d1.setScale(4, RoundingMode.DOWN); // 直接截断，123.4567
        System.out.println(d2);
        System.out.println(d3);
    }
}
```

Precision is not lost when adding, subtracting, or multiplying `BigDecimal`, but when dividing, there are cases where you can't divide all the way, so you have to specify the precision and how to truncate:

```
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("23.456789");
BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入
BigDecimal d4 = d1.divide(d2); // 报错：ArithmeticException，因为除不尽
```

It is also possible to divide `BigDecimal` and find the remainder at the same time:

```x-java
import java.math.BigDecimal;
----
public class Main {
    public static void main(String[] args) {
        BigDecimal n = new BigDecimal("12.345");
        BigDecimal m = new BigDecimal("0.12");
        BigDecimal[] dr = n.divideAndRemainder(m);
        System.out.println(dr[0]); // 102
        System.out.println(dr[1]); // 0.105
    }
}
```

When the `divideAndRemainder()` method is called, the array returned contains two `BigDecimal`s, the quotient and remainder, where the quotient is always an integer and the remainder is not greater than the divisor. We can use this method to determine if the two `BigDecimal` are integer multiples:

```
BigDecimal n = new BigDecimal("12.75");
BigDecimal m = new BigDecimal("0.15");
BigDecimal[] dr = n.divideAndRemainder(m);
if (dr[1].signum() == 0) {
    // n是m的整数倍
}
```

### Compare BigDecimal

When comparing two `BigDecimal` values to be equal, pay special attention to the fact that using the `equals()` method requires not only that the values of the two `BigDecimal`s are equal, but also that their `scale()`s are equal:

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("123.45600");
System.out.println(d1.equals(d2)); // false,因为scale不同
System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为3
System.out.println(d1.compareTo(d2)); // 0 = 相等, -1 = d1 < d2, 1 = d1 > d2
```

Comparisons must be made using the `compareTo()` method, which returns negative, positive, and `0` for less than, greater than, and equal to, respectively, depending on the size of the two values.

```alert type=warning title=注意
总是使用compareTo()比较两个BigDecimal的值，不要使用equals()！
```

If you look at the source code of `BigDecimal`, you can see that a `BigDecimal` is actually represented by a `BigInteger` and a `scale`, i.e., the `BigInteger` represents a full integer and the `scale` represents the number of decimal places:

```java
public class BigDecimal extends Number implements Comparable<BigDecimal> {
    private final BigInteger intVal;
    private final int scale;
}
```

`BigDecimal` also inherits from `Number` and is also an immutable object.

### Summary

`BigDecimal` is used to represent exact decimals and is often used in financial calculations;

To compare `BigDecimal` values for equality, you must use `compareTo()` and not `equals()`.