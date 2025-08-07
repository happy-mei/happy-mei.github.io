<!-- TRANSLATED by md-translate -->
# Exceptions in Java

In the process of running a computer program, there are always various errors.

There are some errors caused by the user, for example, the user is expected to enter an age of type `int`, but the user's input is `abc`:

```java
// 假设用户输入了abc：
String s = "abc";
int n = Integer.parseInt(s); // NumberFormatException!
```

The program wants to read and write the contents of a file, but the user has deleted it:

```java
// 用户删除了该文件：
String t = readFile("C:\\abc.txt"); // FileNotFoundException!
```

There are also mistakes that occur randomly and can never be avoided. For example:

* The network was suddenly disconnected and could not connect to the remote server;
* Memory runs out and the program crashes;
* The user clicks "Print", but there is no printer;
* ......

Therefore, a robust program must handle a wide variety of errors.

An error is when a program calls a function and if it fails, it indicates an error.

How can the caller be informed of a failed call? There are two ways:

Method 1: Agree to return an error code.

For example, processing a file that returns `0` indicates success, and returns some other integer that indicates an agreed-upon error code:

```java
int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}
```

Because of the use of `int` type error codes, it is very cumbersome to try to deal with them. This approach is common in underlying C functions.

Approach 2: Provide an exception handling mechanism at the language level.

Java has a built-in exception handling mechanism that always uses exceptions to indicate errors.

An exception is a `class` and thus carries type information on its own. Exceptions can be thrown anywhere, but only need to be caught at a higher level so that they are separated from the method call:

```java
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}
```

Because Java's exceptions are `class`, it has the following inheritance relationship:

```ascii
┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

From the inheritance relationship: `Throwable` is the root of the exception system, which inherits from `Object`. `Throwable` has two systems: `Error` and `Exception`, `Error` indicates a serious error, which the program can't generally do anything about, for example:

* `OutOfMemoryError`: ran out of memory
* `NoClassDefFoundError`: Could not load a class.
* `StackOverflowError`: stack overflowed

An `Exception`, on the other hand, is a runtime error that can be caught and handled.

Certain exceptions are part of the application logic processing and should be caught and handled. Example:

* `NumberFormatException`: formatting error for numeric type
* `FileNotFoundException`: file not found
* `SocketException`: Failed to read network.

There are also exceptions that are the result of incorrectly written program logic and should be fixed in the program itself. Example:

* `NullPointerException`: a method or field is called on a `null` object.
* `IndexOutOfBoundsException`: array index out of bounds

`Exception` is subdivided into two main categories:

1. `RuntimeException` and its subclasses;
2. non-RuntimeException` (including `IOException`, `ReflectiveOperationException`, etc.)

Java regulations:

* Exceptions that must be caught, including `Exception` and its subclasses, but excluding `RuntimeException` and its subclasses; this type of exception is called a Checked Exception.
* Exceptions that do not need to be caught, including `Error` and its subclasses, and `RuntimeException` and its subclasses.

> [!WARNING]注意
>
> 编译器对RuntimeException及其子类不做强制捕获要求，不是指应用程序本身不应该捕获并处理RuntimeException。是否需要捕获，具体问题具体分析。

### Catch exceptions

Catching an exception uses a `try.... .catch` statement, which puts the code where the exception might occur into a `try {...} `, and then use `catch` to catch the corresponding `Exception` and its subclasses:

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        try {
            // 用指定编码转换String为byte[]:
            return s.getBytes("GBK");
        } catch (UnsupportedEncodingException e) {
            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:
            System.out.println(e); // 打印异常信息
            return s.getBytes(); // 尝试使用默认编码
        }
    }
}
```

If we don't catch `UnsupportedEncodingException`, there will be a compilation failure:

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        return s.getBytes("GBK");
    }
}
```

The compiler reports an error with a message similar to: unreported exception UnsupportedEncodingException; must be caught or declared to be thrown, and pinpoints the statement that needs to be caught as `return s.getBytes(" GBK");`. Meaning that a Checked Exception like `UnsupportedEncodingException` must be caught.

This is because the `String.getBytes(String)` method definition is:

```java
public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
}
```

At the time of method definition, `throws Xxx` is used to indicate the types of exceptions that may be thrown by the method. The caller must force these exceptions to be caught at the time of the call, otherwise the compiler will report an error.

In the `toGBK()` method, an `UnsupportedEncodingException` must be caught because the `String.getBytes(String)` method is called. Instead of catching it, we can also let the `toGBK()` method pass compiler checking by indicating that the `toGBK()` method may throw an `UnsupportedEncodingException` with throws at the method definition:

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        return s.getBytes("GBK");
    }
}
```

The above code still gets a compile error, but this time, instead of calling `return s.getBytes("GBK");`, the compiler prompts for `byte[] bs = toGBK("Chinese");`. This is because the call to `toGBK()` in the `main()` method does not catch the `UnsupportedEncodingException` that it declares may be thrown.

The fix is to catch the exception in the `main()` method and handle it:

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        try {
            byte[] bs = toGBK("中文");
            System.out.println(Arrays.toString(bs));
        } catch (UnsupportedEncodingException e) {
            System.out.println(e);
        }
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

As you can see, any Checked Exception declared by a method that is not caught at the calling level must also be caught at a higher calling level. All uncaught exceptions must also eventually be caught in the `main()` method, and there will be no missed `try`s. This is guaranteed by the compiler. The `main()` method is also the last opportunity to catch `Exception`.

The above is slightly more cumbersome to write if you are testing code. If you don't want to write any `try` code, you can just define the `main()` method as `throws Exception`:

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

Because the `main()` method declares that `Exception` may be thrown, it also declares that all `Exception`s may be thrown, so there is no need to catch them internally. The tradeoff is that the program will exit as soon as the exception occurs.

There are also some kids who like to "digest" exceptions inside `toGBK()`:

```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 什么也不干
    }
    return null;
```

This catch-and-don't-handle approach is very bad; even if you really can't do anything, log the exception first:

```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 先记下来再说:
        e.printStackTrace();
    }
    return null;
```

All exceptions can be printed by calling the `printStackTrace()` method to print the exception stack, which is a simple and useful way to quickly print exceptions.

### Summary

Java uses exceptions to indicate errors, and catches them via `try ... catch` to catch exceptions;

Java exceptions are `class` and inherit from `Throwable`;

An `Error` is a serious error that does not need to be caught, and an `Exception` is a handleable error that should be caught;

A `RuntimeException` does not need to be forced to be caught, a non-`RuntimeException` (Checked Exception) needs to be forced to be caught or declared with `throws`;

It is not recommended to catch an exception but not do anything with it.