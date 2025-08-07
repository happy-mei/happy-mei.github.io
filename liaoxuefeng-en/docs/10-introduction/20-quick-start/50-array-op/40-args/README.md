<!-- TRANSLATED by md-translate -->
# Command line arguments

The entry point to a Java program is the `main` method, and the `main` method can take a command-line argument, which is a `String[]` array.

This command line argument is taken by the JVM as user input and passed to the `main` method:

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```

We can use the received command line arguments to execute different code depending on the arguments. For example, implement a `-version` parameter that prints the program version number:

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            if ("-version".equals(arg)) {
                System.out.println("v 1.0");
                break;
            }
        }
    }
}
```

This program above must be executed at the command line, so let's compile it first:

```plain
$ javac Main.java
```

Then, when executed, pass it a `-version` parameter:

```plain
$ java Main -version
v 1.0
```

This allows the program to respond differently depending on the incoming command line arguments.

### Summary

The type of the command line argument is a `String[]` array;

Command line arguments are received by the JVM as user input and passed to the `main` method;

How to parse command line arguments needs to be implemented by the program itself.