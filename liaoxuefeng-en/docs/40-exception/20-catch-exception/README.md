<!-- TRANSLATED by md-translate -->
# Catch exceptions

In Java, any statement that may throw an exception can be caught with `try ... catch`. Place statements that may throw exceptions in `try { ... }` and then use `catch` to catch the corresponding `Exception` and its subclasses.

### Multiple catch statements

Multiple `catch` statements can be used, with each `catch` catching the corresponding `Exception` and its subclasses.The JVM matches the `catch` statements from top to bottom after an exception is caught, and when it matches a particular `catch`, it executes the `catch` block and _doesn't _continue_ to match.

Simply put: only one of multiple `catch` statements can be executed. Example:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println(e);
    } catch (NumberFormatException e) {
        System.out.println(e);
    }
}
```

When multiple `catch`s exist, the order of the `catch`s is important: the subclass must be written first. Example:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("IO error");
    } catch (UnsupportedEncodingException e) { // 永远捕获不到
        System.out.println("Bad encoding");
    }
}
```

For the code above, the `UnsupportedEncodingException` exception is never caught because it is a subclass of `IOException`. When the `UnsupportedEncodingException` exception is thrown, it is caught by `catch (IOException e) { ... }` is caught and executed.

Therefore, the correct way to write it is to put the subclass in front:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

### finally statement

Regardless of whether an exception occurs or not, how do we write statements if we wish to execute something, such as a cleanup job?

You can write the execution statement a number of times: the normal execution is put into `try`, and each `catch` is written again. Example:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
        System.out.println("END");
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
        System.out.println("END");
    } catch (IOException e) {
        System.out.println("IO error");
        System.out.println("END");
    }
}
```

The above code executes the statement `System.out.println("END");` whether or not an exception occurs.

So how do you eliminate this repetitive code?Java's `try ... catch` mechanism also provides `finally` statements, `finally` blocks that are guaranteed to execute with or without errors. The above code can be rewritten as follows:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    } finally {
        System.out.println("END");
    }
}
```

Note that `finally` has several features:

1. The `finally` statement is not required and may or may not be written;
2. `finally` is always executed last.

If no exception occurs, execute the `try { ... }` block and then execute `finally`. If an exception occurs, you interrupt execution of the `try { ... }` block, then skip to the matching `catch` block, and finally execute `finally`.

As you can see, `finally` is used to ensure that some code must be executed.

In some cases, it is possible to have no `catch` and just use the `try ... finally` structure. Example:

```java
void process(String file) throws IOException {
    try {
        ...
    } finally {
        System.out.println("END");
    }
}
```

Because the method declares exceptions that may be thrown, you can leave out `catch`.

### Catch multiple exceptions

If some exceptions have the same handling logic, but the exceptions themselves are not inherited, then multiple `catch` clauses have to be written:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("Bad input");
    } catch (NumberFormatException e) {
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

Since the code that handles `IOException` and `NumberFormatException` is the same, we can merge the two together with `|`:

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) {
        // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

### Exercise

Use `try ... catch` to catch exceptions and handle them.

[Download exercise](exception-catch.zip)

### Summary

When catching exceptions, the order in which multiple `catch` statements are matched is very important, and the subclass must come first;

The `finally` statement guarantees that it will be executed with or without an exception; it is optional;

A `catch` statement can also match multiple exceptions that are not inherited relationships.