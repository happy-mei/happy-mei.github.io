<!-- TRANSLATED by md-translate -->
# BigInteger

In Java, the maximum range of integers provided natively by the CPU is 64-bit `long` integers. Using `long` integers can be computed directly by CPU instructions, which is very fast.

What if the range of integers we use exceeds the `long` type? At this point, the only way to simulate a large integer is to use software. `java.math.BigInteger` is used to represent an integer of any size. BigInteger` internally uses an `int[]` array to simulate a very large integer:

```java
BigInteger bi = new BigInteger("1234567890");
System.out.println(bi.pow(5)); // 2867971860299718107233761438093672048294900000
```

When doing operations on `BigInteger`, only instance methods can be used, e.g., addition:

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```

Compared to `long` type integer arithmetic, `BigInteger` does not have a range restriction, but has the disadvantage of being slower.

It is also possible to convert `BigInteger` to `long` type:

```java
BigInteger i = new BigInteger("123456789000");
System.out.println(i.longValue()); // 123456789000
System.out.println(i.multiply(i).longValueExact()); // java.lang.ArithmeticException: BigInteger out of long range
```

When using the `longValueExact()` method, an `ArithmeticException` is thrown if the `long` type is out of range.

`BigInteger`, like `Integer` and `Long`, is an immutable class and also inherits from the `Number` class. This is because `Number` defines several methods for converting to basic types:

* Conversion to `byte`: `byteValue()`.
* Convert to `short`: `shortValue()`.
* Converts to `int`: `intValue()`.
* Converts to `long`: `longValue()`.
* Convert to `float`: `floatValue()`.
* Convert to `double`: `doubleValue()`.

Thus, by the above method, `BigInteger` can be converted to a basic type. If `BigInteger` represents a range that exceeds the range of the basic type, the high level information will be lost when converting, i.e., the result may not be accurate. If you need to accurately convert to a basic type, you can use methods such as `intValueExact()`, `longValueExact()`, etc., and an `ArithmeticException` will be thrown directly if the range is exceeded during the conversion.

If the value of `BigInteger` exceeds even the maximum range of `float` (3.4x10<sup>38</sup>), what is the returned float?

```java
// BigInteger to float
import java.math.BigInteger;

public class Main {
    public static void main(String[] args) {
        BigInteger n = new BigInteger("999999").pow(99);
        float f = n.floatValue();
        System.out.println(f); // Infinity
    }
}
```

### Summary

`BigInteger` is used to represent an integer of arbitrary size;

`BigInteger` is the invariant class and inherits from `Number`;

Converting `BigInteger` to a basic type can be done using methods such as `longValueExact()` to ensure accurate results.