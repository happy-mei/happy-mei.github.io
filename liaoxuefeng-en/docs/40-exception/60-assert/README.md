<!-- TRANSLATED by md-translate -->
# Use of assertions

Assertion is a way to debug a program. In Java, assertion is implemented using the `assert` keyword.

Let's look at an example first:

```java
public static void main(String[] args) {
    double x = Math.abs(-123.45);
    assert x >= 0;
    System.out.println(x);
}
```

The statement `assert x >= 0;` is an assertion, and the assertion condition `x >= 0` is expected to be `true`. If the result of the computation is `false`, the assertion fails and `AssertionError` is thrown.

An optional assertion message can also be added when using the `assert` statement:

```java
assert x >= 0 : "x must >= 0";
```

This way, when the assertion fails, `AssertionError` will carry the message `x must >= 0`, making it easier to debug.

Java assertions are characterized by the fact that `AssertionError` is thrown when the assertion fails, causing the program to end and exit. Therefore, assertions should not be used for recoverable program errors and should only be used in the development and testing phases.

Assertions should not be used for recoverable program errors. Example:

```java
void sort(int[] arr) {
    assert arr != null;
}
```

Exceptions should be thrown and caught at higher levels:

```java
void sort(int[] arr) {
    if (arr == null) {
        throw new IllegalArgumentException("array cannot be null");
    }
}
```

When we use `assert` in a program, for example, a simple assertion:

```java
// assert
public class Main {
    public static void main(String[] args) {
        int x = -1;
        assert x > 0;
        System.out.println(x);
    }
}
```

Asserting that `x` must be greater than `0`, when in fact `x` is `-1`, the assertion must have failed. Executing the above code, I found that the program did not throw `AssertionError`, but printed the value of `x` normally.

What's going on here? Why doesn't the `assert` statement work?

This is because the JVM turns off the assertion directive by default, i.e., it automatically ignores the `assert` statement when it encounters it and does not execute it.

To execute the `assert` statement, assertions must be enabled by passing the `-enableassertions` (which can be abbreviated as `-ea`) parameter to the Java Virtual Machine. Therefore, the above program must be run from the command line to be effective:

```plain
$ java -ea Main.java
Exception in thread "main" java.lang.AssertionError
    at Main.main(Main.java:5)
```

Assertions can also be selectively enabled for specific classes, with the command line argument: `-ea:com.itranswarp.sample.Main`, which means that assertions are enabled only for the class `com.itranswarp.sample.Main`.

Or to enable assertions for specific packages, the command line argument is: `-ea:com.itranswarp.sample... ` (note the 3 `. `), which enables assertions for the package `com.itranswarp.sample`.

In practice, assertions are rarely used in development. A better approach is to write unit tests, and we'll explain the use of `JUnit` later.

### Summary

Assertion is a debugging method that throws `AssertionError` if it fails, and should only be enabled during the development and testing phases;

Assertions should not be used for recoverable errors; instead, exceptions should be thrown;

Assertions are rarely used, and a better approach is to write unit tests.