<!-- TRANSLATED by md-translate -->
# Customized exceptions

Common exceptions defined by the Java Standard Library include:

```ascii
Exception
├─ RuntimeException
│  ├─ NullPointerException
│  ├─ IndexOutOfBoundsException
│  ├─ SecurityException
│  └─ IllegalArgumentException
│     └─ NumberFormatException
├─ IOException
│  ├─ UnsupportedCharsetException
│  ├─ FileNotFoundException
│  └─ SocketException
├─ ParseException
├─ GeneralSecurityException
├─ SQLException
└─ TimeoutException
```

When we need to throw exceptions in our code, we try to use the exception types already defined by the JDK. For example, `IllegalArgumentException` should be thrown for illegal argument checking:

```java
static void process1(int age) {
    if (age <= 0) {
        throw new IllegalArgumentException();
    }
}
```

In a large project, it is possible to customize new exception types, but it is important to maintain a sensible exception inheritance system.

A common practice is to customize a `BaseException` as the `root exception` and then, derive exceptions for various business types.

`BaseException` needs to be derived from a suitable `Exception`, and it is usually recommended to derive it from `RuntimeException`:

```java
public class BaseException extends RuntimeException {
}
```

Exceptions for other business types can then be derived from `BaseException`:

```java
public class UserNotFoundException extends BaseException {
}

public class LoginFailedException extends BaseException {
}

...
```

A custom `BaseException` should provide multiple constructor methods:

```java
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
```

All of the above constructors actually copy `RuntimeException` as is. This way, when an exception is thrown, the appropriate constructor method can be chosen. The IDE makes it possible to quickly generate constructor methods for subclasses based on the parent class.

### Exercise

Please derive custom exceptions from `BaseException`.

[Download exercise](exception-custom.zip)

### Summary

When throwing exceptions, try to reuse the exception types already defined by the JDK;

When customizing the exception system, it is recommended to derive the "root exception" from `RuntimeException` and then derive the business exception;

When customizing exceptions, multiple constructor methods should be provided.