<!-- TRANSLATED by md-translate -->
# Throw an exception

### The propagation of anomalies ###

When a method throws an exception, if the current method doesn't catch the exception, the exception is thrown to the higher level calling method until it encounters some `try ... catch` is caught:

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        process2();
    }

    static void process2() {
        Integer.parseInt(null); // 会抛出NumberFormatException
    }
}
```

With `printStackTrace()` you can print out the call stack of a method, similarly:

```java
java.lang.NumberFormatException: null
    at java.base/java.lang.Integer.parseInt(Integer.java:614)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.process2(Main.java:16)
    at Main.process1(Main.java:12)
    at Main.main(Main.java:5)
```

The `printStackTrace()` is very useful for debugging errors, and the above message says: `NumberFormatException` was thrown in the `java.lang.Integer.parseInt` method, and the calling hierarchy is in order from the bottom up:

1. `main()` calls `process1()`;
2. `process1()` calls `process2()`;
3. `process2()` calls `Integer.parseInt(String)`;
4. `Integer.parseInt(String)` calling `Integer.parseInt(String, int)`.

A look at the `Integer.java` source code shows that the code for the method that throws the exception is as follows:

```java
public static int parseInt(String s, int radix) throws NumberFormatException {
    if (s == null) {
        throw new NumberFormatException("null");
    }
    ...
}
```

Moreover, the line numbers of the source code are given for each level of calls, which can be directly located.

### Throw an exception

When an error occurs, for example, the user enters an illegal character, we can throw an exception.

How to throw an exception? Referring to the `Integer.parseInt()` method, throwing an exception is done in two steps:

1. Create an instance of `Exception`;
2. throw it with a `throw` statement.

Here is an example:

```java
void process2(String s) {
    if (s==null) {
        NullPointerException e = new NullPointerException();
        throw e;
    }
}
```

In fact, the vast majority of code that throws exceptions is merged and written on one line:

```java
void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

If a method catches an exception and then throws a new exception in a `catch` clause, it is equivalent to "converting" the type of exception thrown:

```java
void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

When `process2()` throws `NullPointerException`, it is caught by `process1()`, which then throws `IllegalArgumentException()`.

If `IllegalArgumentException` is caught in `main()`, let's take a look at the printed exception stack:

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException();
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```

The printed exception stack is similar:

```plain
java.lang.IllegalArgumentException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
```

This means that the new exception has lost the original exception information and we can no longer see the original exception `NullPointerException`.

To be able to trace the full exception stack, pass in the original `Exception` instance when constructing the exception, and the new `Exception` will hold the original `Exception` information. Improvements to the above code are as follows:

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```

Running the above code prints out a similar exception stack:

```plain
java.lang.IllegalArgumentException: java.lang.NullPointerException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
Caused by: java.lang.NullPointerException
    at Main.process2(Main.java:20)
    at Main.process1(Main.java:13)
```

Notice the `Caused by: Xxx`, indicating that the caught `IllegalArgumentException` is not the root cause of the problem, the root cause is the `NullPointerException`, which is thrown in the `Main.process2()` method.

The `Throwable.getCause()` method can be used to get the original exception in code. If it returns `null`, it is already a root exception.

With complete information about the exception stack, we can quickly locate and fix problems with the code.

> [!NOTICE]最佳实践
>
> 捕获到异常并再次抛出时，一定要留住原始异常，否则很难定位第一案发现场！

If we throw an exception in a `try` or `catch` statement block, does the `finally` statement execute? Example:

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
        }
    }
}
```

The result of the execution of the above code is as follows:

```plain
catched
finally
Exception in thread "main" java.lang.RuntimeException: java.lang.NumberFormatException: For input string: "abc"
    at Main.main(Main.java:8)
Caused by: java.lang.NumberFormatException: For input string: "abc"
    at ...
```

The first line prints `catched`, indicating that the `catch` statement block was entered. The second line prints `finally`, indicating that the `finally` block was executed.

Therefore, throwing an exception in `catch` will not affect the execution of `finally`.The JVM will execute `finally` first and then throw the exception.

### Anomaly masking ###

If an exception is thrown during the execution of a `finally` statement, can the exception for the `catch` statement continue to be thrown? Example:

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
            throw new IllegalArgumentException();
        }
    }
}
```

The above code is executed and the following exception message is found:

```plain
catched
finally
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
```

This means that when `finally` throws an exception, the exception that was going to be thrown in `catch` "disappears", because only one exception can be thrown. Exceptions that are not thrown are called "suppressed exceptions".

In rare cases, we need to be informed of all exceptions. How to save all the exception information? The way to do it is to first save the original exception with the `origin` variable, then call `Throwable.addSuppressed()` to add the original exception, and finally throw it in `finally`:

```java
// exception
public class Main {
    public static void main(String[] args) throws Exception {
        Exception origin = null;
        try {
            System.out.println(Integer.parseInt("abc"));
        } catch (Exception e) {
            origin = e;
            throw e;
        } finally {
            Exception e = new IllegalArgumentException();
            if (origin != null) {
                e.addSuppressed(origin);
            }
            throw e;
        }
    }
}
```

When both `catch` and `finally` throw exceptions, the exception thrown by `finally` still contains it, even though the `catch` exception is masked:

```plain
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
Suppressed: java.lang.NumberFormatException: For input string: "abc"
    at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
    at java.base/java.lang.Integer.parseInt(Integer.java:652)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.main(Main.java:6)
```

All `Suppressed Exception`s can be obtained via `Throwable.getSuppressed()`.

In the vast majority of cases, don't throw exceptions in `finally`. Therefore, we usually don't need to care about `Suppressed Exception`.

### Post an exception when asking a question ###

The detailed stack information printed by the exception is the key to find out the problem, many beginners in the question only post the code, not the exception, the equivalent of only report the case without giving clues, Holmes can not do anything.

![Who taught you to ask questions without posting an exception stack](pa.png)

Some other kids only post part of the anomaly information, the most critical `Caused by: xxx` is omitted, this is the incorrect way to ask the question, need to be changed.

### Exercise

Throws `IllegalArgumentException` if the passed argument is negative.

[Download exercise](exception-throw.zip)

### Summary

Calling `printStackTrace()` prints the exception propagation stack, which is useful for debugging;

When an exception is caught and a new exception is thrown again, the original exception information should be held;

Normally do not throw exceptions in `finally`. If an exception is thrown in `finally`, the original exception should be added to the original exception. The caller can get all added `Suppressed Exception`s via `Throwable.getSuppressed()`.