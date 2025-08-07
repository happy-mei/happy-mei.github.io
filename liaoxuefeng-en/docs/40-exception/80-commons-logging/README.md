<!-- TRANSLATED by md-translate -->
# Use Commons Logging

Unlike the logging provided by the Java standard library, Commons Logging is a third-party logging library, which is a logging module created by Apache.

Commons Logging features the ability to hook up different logging systems and specify which logging system to hook up via a configuration file. By default, Commons Loggin automatically searches for and uses Log4j (Log4j is another popular logging system), and then uses JDK Logging if Log4j is not found.

There are only two classes to deal with using Commons Logging, and it's only a two-step process:

In the first step, get an instance of the `Log` class through `LogFactory`;
In the second step, use the methods of the `Log` instance to type the log.

The sample code is as follows:

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Main {
    public static void main(String[] args) {
        Log log = LogFactory.getLog(Main.class);
        log.info("start...");
        log.warn("end.");
    }
}
```

Running the above code will definitely get a compile error, something like `error: package org.apache.commons.logging does not exist` (the package `org.apache.commons.logging` could not be found). Since Commons Logging is a library provided by a third party, you must [download](https://commons.apache.org/proper/commons-logging/download_logging.cgi) it first. After downloading it, unzip it, find the file `commons-logging-1.2.jar`, and put the Java source code `Main.java` into a directory, such as the `work` directory:

```ascii
work
├─ commons-logging-1.2.jar
└─ Main.java
```

Then compile `Main.java` with `javac`, specifying `classpath` when compiling, otherwise the compiler won't be able to find the `org.apache.commons.logging` package we referenced. The compilation command is as follows:

```plain
javac -cp commons-logging-1.2.jar Main.java
```

If the compilation is successful, then there will be an additional `Main.class` file in the current directory:

```ascii
work
├─ commons-logging-1.2.jar
├─ Main.java
└─ Main.class
```

This `Main.class` can now be executed, using the `java` command, which must also specify the `classpath`, with the following command:

```plain
java -cp .;commons-logging-1.2.jar Main
```

Notice that the incoming `classpath` has two parts: a `. ` and one for `commons-logging-1.2.jar`, split by `;`. The `. ` indicates the current directory; without this `. `, the JVM will not search for `Main.class` in the current directory, and will report an error.

If running under Linux or macOS, note that the separator for `classpath` is not `;`, but `:`:

```plain
java -cp .:commons-logging-1.2.jar Main
```

The results of the run are as follows:

```plain
Mar 02, 2019 7:15:31 PM Main main
INFO: start...
Mar 02, 2019 7:15:31 PM Main main
WARNING: end.
```

Commons Logging defines six logging levels:

* FATAL
* ERROR
* WARNING
* INFO
* DEBUG
* TRACE

The default level is `INFO`.

When using Commons Logging, it is usually straightforward to define a static type variable if `Log` is referenced in a static method:

```java
// 在静态方法中引用Log:
public class Main {
    static final Log log = LogFactory.getLog(Main.class);

    static void foo() {
        log.info("foo");
    }
}
```

Referencing `Log` in an instance method usually defines an instance variable:

```java
// 在实例方法中引用Log:
public class Person {
    protected final Log log = LogFactory.getLog(getClass());

    void foo() {
        log.info("foo");
    }
}
```

Notice that the instance variable log is obtained by `LogFactory.getLog(getClass())`, and while it is also possible to use `LogFactory.getLog(Person.class)`, the former approach has the very great advantage that subclasses can use that `log` instance directly. Example:

```java
// 在子类中使用父类实例化的log:
public class Student extends Person {
    void bar() {
        log.info("bar");
    }
}
```

Due to the dynamic nature of Java classes, the `log` field fetched by the subclass is actually equivalent to `LogFactory.getLog(Student.class)`, but is inherited from the parent class and requires no code changes.

In addition, Commons Logging's logging methods, such as `info()`, provide a very useful overloaded method, `info(String, Throwable)`, in addition to the standard `info(String)`, which makes logging exceptions much easier:

```java
try {
    ...
} catch (Exception e) {
    log.error("got exception!", e);
}
```

### Exercise

Use `log.error(String, Throwable)` to print the exception.

[Download exercise](logging-commons.zip)

### Summary

Commons Logging is the most widely used logging module;

The Commons Logging API is very simple;

Commons Logging can automatically detect and use other logging modules.