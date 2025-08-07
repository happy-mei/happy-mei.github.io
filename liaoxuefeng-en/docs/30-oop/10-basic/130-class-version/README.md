<!-- TRANSLATED by md-translate -->
# class version

In Java development, many children's shoes are often confused by various versions of the JDK, this section we will explain in detail the Java program compiled class file version of the problem.

By Java 8, Java 11, and Java 17, we usually mean the version of the JDK, which is the version of the JVM, or more specifically, the version of the program `java.exe`:

```plain
$ java -version
java version "17" 2021-09-14 LTS
```

And each version of the JVM, it can be executed by a different version of the class file. For example, Java 11 corresponds to class file version 55, while Java 17 corresponds to class file version 61.

If you compile a Java program with Java 11, the output class file version is 55 by default, and this class can run on both Java 11 and Java 17, because Java 17 supports class file version 61, which means "up to version 61 is supported. ".

If you compile a Java program with Java 17, the output class file version is 61 by default, which will run on Java 17, Java 18, but not on Java 11, which supports class versions up to 55. If you run it with a JVM lower than Java 17, you will get an ` UnsupportedClassVersionError` with a similar error message:

```plain
java.lang.UnsupportedClassVersionError: Xxx has been compiled by a more recent version of the Java Runtime...
```

As soon as you see `UnsupportedClassVersionError` it means that the current version of the class file to be loaded exceeds the capacity of the JVM and a higher version of the JVM must be used in order to run.

Let's say you save a Word file with Word 2013, this file can also be opened on Word 2016. But on the flip side, save a Word file with Word 2016 and it won't open with Word 2013.

But wait, with Word 2016 can also save a file formatted as Word 2013, so that the saved Word file can be opened with a lower version of Word 2013, but only if the save must explicitly specify the file format compatible with Word 2013.

Similarly, corresponding to the JVM class file, we can also compile a Java program with Java 17, specifying that the output class version should be compatible with Java 11 (i.e., class version 55), so that the compiled class file can be run in a Java >= 11 environment.

There are two ways to specify the compilation output, one is to set it on the `javac` command line with the parameter `--release`:

```plain
$ javac --release 11 Main.java
```

The `--release 11` parameter indicates that the source code is Java 11 compatible and the output version of the compiled class is Java 11 compatible, i.e., class version 55.

The second way is to specify the source version with the argument `-source` and the output class version with the argument `-target`:

```plain
$ javac --source 9 --target 11 Main.java
```

If the above command is compiled with the Java 17 JDK, it will treat the source as Java 9 compliant and output the class as Java 11 compliant. Note that the `--release` parameter and the `--source --target` parameter can only be set to one or the other, not both.

However, there are some potential problems with specifying a version that is lower than the current JDK version. For example, we compile `Hello.java` with Java 17, with the parameters `-source 9` and `-target 11`:

```java
public class Hello {
    public static void hello(String name) {
        System.out.println("hello".indent(4));
    }
}
```

Running `Hello` with a JVM lower than Java 11 gets a `LinkageError` because the `Hello.class` file cannot be loaded, and running `Hello` with Java 11 gets a `NoSuchMethodError` because the `String.indent()` method was added from Java 12, and the `String` version of Java 11 doesn't have an `indent()` method at all.

> [!NOTICE]注意
>
> 如果使用--release 11则会在编译时检查该方法是否在Java 11中存在。

Therefore, if the version of the JVM at runtime is Java 11, it is also better to use Java 11 at compile time, rather than compiling with a higher version of the JDK to output a lower version of the class.

If you use `javac` to compile without specifying any version parameter, it is equivalent to compiling with `--release current version`, i.e., both the source and output versions are current.

During the development phase, multiple versions of the JDK can be installed at the same time, and the version of the JDK currently in use can be switched by the `JAVA_HOME` environment variable.

### Source code version

When writing source code, we usually preset a source version. When compiling, if the source version is specified with `-source` or `-release`, the specified source version checking syntax is used.

For example, source versions with lambda expressions must be at least 8 to compile, source versions with the `var` keyword must be at least 10 to compile, source versions with `switch` expressions must be at least 12 to compile, and the `--enable-preview` parameter needs to be enabled for versions 12 and 13.

### Summary

Higher versions of the JDK can compile and output class files compatible with lower versions, but note that lower versions of the JDK may not have classes and methods added by higher versions of the JDK, resulting in runtime errors.

Use whichever JDK version you use at runtime, and try to compile the source code with the same version of the JDK at compile time.