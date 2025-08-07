<!-- TRANSLATED by md-translate -->
# The first Java program

Let's write our first Java program.

Open a text editor and enter the following code:

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

In a Java program, you can always find a similar:

```java
public class Hello {
    ...
}
```

The definition of a class is called a class, here the class name is `Hello` and is case sensitive, `class` is used to define a class, `public` indicates that the class is public, `public`, `class` are Java keywords and must be lower case, `Hello` is the name of the class and by convention the initial letter `H` is to be capitalized. And in the middle of the braces `{}` is the definition of the class.

Notice that in the class definition, we define a method named `main`:

```java
public static void main(String[] args) {
        ...
    }
```

Methods are executable blocks of code, a method has method parameters enclosed in `()` in addition to the method name `main`, here the `main` method has one parameter, the type of the parameter is `String[]`, the parameter name is `args`, `public`, `static` are used to qualify the method, here it means that it is a public static method, ` void` is the return type of the method, and what's in between the braces `{}` is the code of the method.

The code for the method ends each line with `;`, and here there is only one line of code, which is:

```java
System.out.println("Hello, world!");
```

It is used to print a string to the screen.

Java specifies that `public static void main(String[] args)`, defined by a class, is the fixed entry method for a Java program, and therefore, Java programs always start execution from the `main` method.

Notice that indentation of Java source code is not required, but with indentation, the formatting looks good and it is easy to see the beginning and end of the code block, indentation is usually 4 spaces or a tab.

Finally, when we save the code as a file, the filename must be `Hello.java`, and the filename should also be case sensitive, as it should be exactly the same as the class name we defined, `Hello`.

### How to run a Java program

Java source code is essentially a text file, we need to first compile `Hello.java` into a bytecode file `Hello.class` using `javac` and then, execute this bytecode file using `java` command:

```ascii
┌──────────────────┐
│    Hello.java    │◀── source code
└──────────────────┘
          │ compile
          ▼
┌──────────────────┐
│   Hello.class    │◀── byte code
└──────────────────┘
          │ execute
          ▼
┌──────────────────┐
│    Run on JVM    │
└──────────────────┘
```

Thus, the executable `javac` is the compiler and the executable `java` is the virtual machine.

The first step is to execute the command `javac Hello.java` in the directory where `Hello.java` is saved:

```plain
$ javac Hello.java
```

If the source code is correct, there will be no output from the above command and a `Hello.class` file will be generated in the current directory:

```plain
$ ls
Hello.class	Hello.java
```

In the second step, execute `Hello.class`, using the command `java Hello` (note that it is not `java Hello.class`):

```plain
$ java Hello
Hello, world!
```

Note: The parameter `Hello` passed to the VM is the name of the class we defined, and the VM automatically looks up the corresponding class file and executes it.

If you execute `java Hello` you get an error:

```plain
$ java Hello
Error: Could not find or load main class Hello
Caused by: java.lang.ClassNotFoundException: Hello
```

A `ClassNotFoundException` message appears, indicating that there is no `Hello.class` file in the current directory, please switch to the `Hello.class` directory and execute `java Hello`.

As some of you may know, it is possible to run `java Hello.java` directly:

```plain
$ java Hello.java 
Hello, world!
```

This is a new feature from Java 11, which allows you to run a single-file source code directly!

Note that in real projects, a single Java source code that does not depend on third-party libraries is very rare, so in the vast majority of cases, we can not run a Java source code file directly, the reason is that it needs to depend on other libraries.

### Summary

Only one class of type `public` can be defined for a Java source, and the class name and file name should be identical;

Using `javac` you can compile `.java` source code into `.class` bytecode;

Use `java` to run a compiled Java program with the class name as the argument.