<!-- TRANSLATED by md-translate -->
# Package

In the previous code, we named the classes and interfaces with simple names such as `Person`, `Student`, `Hello`, and so on.

In reality, what if Ming writes a `Person` class and Red also writes a `Person` class, and now, White wants to use both Ming's `Person` and Red's `Person`?

If Jun writes an `Arrays` class, and it happens that the JDK also comes with an `Arrays` class, how do you resolve the class name conflict?

In Java, we use `package` to resolve name conflicts.

Java defines a namespace called a package: `package`. A class always belongs to a package, and the class name (e.g., `Person`) is just a shorthand; the real full class name is `package. ClassName`.

Example:

Xiaoming's `Person` class is stored under the package `ming`, so the full class name is `ming.Person`;

Red's `Person` class is stored under the package `hong`, so the full class name is `hong.Person`;

Junior's `Arrays` class is stored under package `mr.jun`, so the full class name is `mr.jun.Arrays`;

The JDK's `Arrays` class is stored under the package `java.util`, so the full class name is `java.util.Arrays`.

When defining a `class`, we need to declare on the first line which package the `class` belongs to.

The `Person.java` file of Xiaoming:

```java
package ming; // 申明包名ming

public class Person {
}
```

Junior's `Arrays.java` file:

```java
package mr.jun; // 申明包名mr.jun

public class Arrays {
}
```

When the Java Virtual Machine executes, the JVM only looks at the full class name, so as long as the package name is different, the class is different.

Packages can be multi-layered structures, separated by `. ` separated by `. For example: `java.util`.

```alert type=warning title=特别注意
包没有父子关系。java.util和java.util.zip是不同的包，两者没有任何继承关系。
```

A `class` that does not define a package name, which uses the default package, is very prone to name clashes, so the practice of leaving the package name unspecified is not recommended.

We also need to organize the Java files above according to the package structure. Assuming `package_sample` as the root directory and `src` as the source directory, the structure of all the files is:

```ascii
package_sample
└─ src
    ├─ hong
    │  └─ Person.java
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```

That is, the directory hierarchy corresponding to all Java files should be the same as the package hierarchy.

The compiled `.class` file also needs to be stored in a package structure. If you use an IDE and put the compiled `.class` file in the `bin` directory, the structure of the compiled file is:

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

### Package scoping

Classes located in the same package can access package-scoped fields and methods. Fields and methods that are not modified by `public`, `protected`, or `private` are package scopes. For example, the `Person` class is defined under the `hello` package:

```java
package hello;

public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```

The `Main` class is also defined under the `hello` package:

```java
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.hello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

### import

In a `class`, we always reference other `classes`. For example, Xiao Ming's `ming.Person` class, if he wants to reference Xiao Jun's `mr.jun.Arrays` class, he has three ways to write it:

The first, write the full class name directly, for example:

```java
// Person.java
package ming;

public class Person {
    public void run() {
        // 写完整类名: mr.jun.Arrays
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

Obviously, it's more painful to write the full class name each time.

Therefore, the second way to write it is to use the `import` statement, import Junior's `Arrays`, and then write the simple class name:

```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        // 写简单类名: Arrays
        Arrays arrays = new Arrays();
    }
}
```

When writing `import`, you can use `*` to indicate that all `class`s under this package are imported (but not the `class`s of subpackages):

```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

We generally don't recommend this writeup because it's hard to see which package the `Arrays` class belongs to after importing multiple packages.

There is also an `import static` syntax which imports static fields and static methods of a class:

```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```

`import static` is rarely used.

The `.class` file that the Java compiler ultimately compiles only uses the _full class name_, so when the compiler encounters a `class` name in the code:

* If it is a full class name, look up the `class` directly from the full class name;
* If it is a simple class name, look it up in the following order:
    - Find out if the `class` exists in the current `package`;
    - Find out if the `import` package contains this `class`;
    - Find out if the `java.lang` package contains this `class`.

If the class name cannot be determined by following the rules above, the compilation reports an error.

Let's look at an example:

```java
// Main.java
package test;

import java.text.Format;

public class Main {
    public static void main(String[] args) {
        java.util.List list; // ok，使用完整类名 -> java.util.List
        Format format = null; // ok，使用import的类 -> java.text.Format
        String s = "hi"; // ok，使用java.lang包的String -> java.lang.String
        System.out.println(s); // ok，使用java.lang包的System -> java.lang.System
        MessageFormat mf = null; // 编译错误：无法找到MessageFormat: MessageFormat cannot be resolved to a type
    }
}
```

Therefore, when writing a class, the compiler automatically does two import actions for us:

* Automatically `import` other `classes` of the current `package` by default;
* Automatically `import java.lang.*` by default.

```alert type=warning title=注意
自动导入的是java.lang包，但类似java.lang.reflect这些包仍需要手动导入。
```

If there are two `class`s with the same name, e.g., `mr.jun.Arrays` and `java.util.Arrays`, then only one of them can be `imported` and the other must be written with the full class name.

### Best practices

To avoid name conflicts, we need to determine unique package names. The recommended practice is to use inverted domain names to ensure uniqueness. Example:

* org.apache.commons.log
* org.apache.commons.log
* com.liaoxuefeng.sample

Subpackages can then name themselves according to their function.

Be careful not to rename classes with classes from the `java.lang` package, i.e., don't use these names for your own classes:

* String
* System
* Runtime
* ...

Be careful not to rename common JDK classes either:

* java.util.List
* java.text.
* java.math.BigInteger
* ...

### Compile and run

Suppose we create the following directory structure:

```ascii
work
├── bin
└── src
    └── com
        └── itranswarp
            ├── sample
            │   └── Main.java
            └── world
                └── Person.java
```

Among them, the `bin` directory is used to store the compiled `class` files, and the `src` directory stores the Java source code according to the package structure, how can we compile these Java source code at once?

First, make sure that the current directory is the `work` directory, the parent directory where `src` and `bin` are stored:

```plain
$ ls
bin src
```

Then, compile all Java files in the `src` directory:

```plain
$ javac -d ./bin src/**/*.java
```

The `-d` command line specifies that the output `class` files are stored in the `bin` directory, followed by the argument `src/**/*.java` which indicates all `.java` files in the `src` directory, including subdirectories of arbitrary depth.

Note: Windows does not support `**` which searches all subdirectories, so compiling under Windows must list all `.java` files in order:

```plain
C:\work> javac -d bin src\com\itranswarp\sample\Main.java src\com\itranswarp\world\Persion.java
```

Using Windows PowerShell you can use `Get-ChildItem` to list all `.java` files in a specified directory:

```plain
PS C:\work> (Get-ChildItem -Path .\src -Recurse -Filter *.java).FullName
C:\work\src\com\itranswarp\sample\Main.java
C:\work\src\com\itranswarp\world\Person.java
```

Therefore, the compile command can be written as:

```plain
PS C:\work> javac -d .\bin (Get-ChildItem -Path .\src -Recurse -Filter *.java).FullName
```

If it compiles correctly, the `javac` command produces no output. The following `class` file can be seen in the `bin` directory:

```ascii
bin
└── com
    └── itranswarp
        ├── sample
        │   └── Main.class
        └── world
            └── Person.class
```

Now we can run the `class` file directly. Determine the classpath based on the location of the current directory, e.g., if the current directory is still `work`, the classpath is `bin` or `. /bin`:

```plain
$ java -cp bin com.itranswarp.sample.Main 
Hello, world!
```

### Exercise

Please create the project according to the following package structure:

```ascii
oop-package
└── src
    └── com
        └── itranswarp
            ├── sample
            │   └── Main.java
            └── world
                └── Person.java
```

[Download Exercise](oop-package.zip)

### Summary

Java has a built-in `package` mechanism to avoid `class` naming conflicts;

The core classes of the JDK use the `java.lang` package, which is automatically imported by the compiler;

Other common JDK classes are defined in `java.util.*`, `java.math.*`, `java.text.*`, ......;

Inverted domain names are recommended for package names, e.g. `org.apache`.