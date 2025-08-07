<!-- TRANSLATED by md-translate -->
# Using JDK Logging

In the process of writing a program, what to do when you find that the result of running the program is not as expected? Of course, it is to use `System.out.println()` to print out certain variables during execution, observe whether the result of each step is consistent with the code logic, and then modify the code in a targeted manner.

What to do when the code is changed? Remove the useless `System.out.println()` statement, of course.

What if I change the code and then change the problem? Add `System.out.println()`.

Repeat this a few times and soon everyone realizes that using `System.out.println()` is very cumbersome.

What to do?

The solution is to use logs.

So what is logging? Logging is Logging, which is meant to replace `System.out.println()`.

Outputting the log instead of using `System.out.println()` has several advantages:

1. You can set the output style to avoid writing `"ERROR: " + var` every time;
2. you can set the output level to disable certain levels of output. For example, only the error log is output;
3. can be redirected to a file so that the log can be viewed at the end of the program run;
4. you can control the logging level by package name to output only logs typed by certain packages;
5. can be ......

In short, there are a lot of benefits.

How does that work with logs?

Because the Java standard library has a built-in logging package `java.util.logging`, we can use it directly. Let's look at a simple example:

```java
// logging
import java.util.logging.Level;
import java.util.logging.Logger;

public class Hello {
    public static void main(String[] args) {
        Logger logger = Logger.getGlobal();
        logger.info("start process...");
        logger.warning("memory is running out...");
        logger.fine("ignored.");
        logger.severe("process will be terminated...");
    }
}
```

Running the above code gives an output similar to the following:

```plain
Mar 02, 2019 6:32:13 PM Hello main
INFO: start process...
Mar 02, 2019 6:32:13 PM Hello main
WARNING: memory is running out...
Mar 02, 2019 6:32:13 PM Hello main
SEVERE: process will be terminated...
```

As you can see by comparison, the biggest benefit of using logging is that it automatically prints the time, calling class, calling method, and a lot of other useful information.

Looking more closely, I found that 4 logs, only 3 are printed, `logger.fine()` is not printed. This is because the output of the log can be set at a level.JDK Logging defines 7 logging levels, from severe to normal:

* SEVERE
* WARNING
* INFO
* CONFIG
* FINE
* FINER
* FINEST

Because the default level is INFO, logs below the INFO level are not printed. The advantage of using logging levels is that by adjusting the level, you can block out a lot of debugging-related logging output.

Using Logging built into the Java Standard Library has the following limitations:

The Logging system reads the configuration file and completes the initialization when the JVM starts, and once it starts running the `main()` method, it is not possible to modify the configuration;

Configuration is less convenient and requires passing the parameter `-Djava.util.logging.config.file=<config-file-name>` at JVM startup.

As a result, the Java standard library built-in Logging is not very widely used. We will introduce a more convenient logging system later.

### Exercise

Use `logger.severe()` to print the exception:

```java
import java.io.UnsupportedEncodingException;
import java.util.logging.Logger;

public class Main {
    public static void main(String[] args) {
        Logger logger = Logger.getLogger(Main.class.getName());
        logger.info("Start process...");
        try {
            "".getBytes("invalidCharsetName");
        } catch (UnsupportedEncodingException e) {
            // TODO: 使用logger.severe()打印异常
        }
        logger.info("Process end.");
    }
}
```

[Download Exercise](logging-jdk.zip)

### Summary

Logging is meant to be a replacement for `System.out.println()` and can define formats, redirect to files, etc;

Logs can be archived for easy tracking of issues;

Log records can be categorized by level, making it easy to turn certain levels on or off;

Logging can be adjusted according to the configuration file without modifying the code;

The Java standard library provides `java.util.logging` to implement logging functionality.