<!-- TRANSLATED by md-translate -->
# Modules

Starting with Java 9, the JDK has introduced modules (Module).

What are modules? It starts with versions prior to Java 9.

As we know, `.class` file is the smallest executable file seen by the JVM, and a large program needs to write a lot of Class and generate a bunch of `.class` files, which is very unmanageable, so the `jar` file is a container for `class` files.

Prior to Java 9, a large Java program would generate its own jar file, while referencing dependent third-party jar files, and the Java standard library that comes with the JVM is actually stored in the form of a jar file called `rt.jar`, which is more than 60M in total.

If you are developing your own program, you need a bunch of third-party jar packages in addition to an `app.jar` of your own to run a Java program, and in general, the command line writes like this:

```plain
java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Main
```

```alert type=warning title=注意
JVM自带的标准库rt.jar不要写到classpath中，写了反而会干扰JVM的正常运行。
```

If you leave out a jar that is required at runtime, it is highly likely that a `ClassNotFoundException` will be thrown at runtime.

So, a jar is just a container for classes, it doesn't care about dependencies between classes.

Modules have been introduced since Java 9, mainly to solve the problem of "dependencies". If `a.jar` must depend on another `b.jar` in order to run, then we should add some description to `a.jar`, so that the program can automatically locate `b.jar` when compiling and running, this kind of class container with "dependency" is a module.

In a sign of Java's commitment to modularity, starting with Java 9, the original Java standard library has been split from a single, huge `rt.jar` into dozens of modules, identified by the `.jmod` extension, which can be found in the `$JAVA_HOME/jmods` directory:

* java.base.jmod
* java.compiler.jmod
* java.datatransfer.jmod
* java.desktop.jmod
* ...

Each of these `.jmod` files is a module, and the module name is the file name. For example, the file corresponding to the module `java.base` is `java.base.jmod`. Dependencies between modules are written to the `module-info.class` file within the module. All modules depend directly or indirectly on `java.base`, only `java.base` module does not depend on any module, it can be regarded as the "root module", as if all classes are inherited directly or indirectly from `Object`.

Packaging a bunch of classes into a jar is just a packaging process, while packaging a bunch of classes into a module requires not only packaging, but also writing dependencies, and can also contain binary code (usually JNI extensions). In addition, modules support multiple versions, that is, in the same module can provide different versions for different JVMs.

### Writing modules

So, how should we write modules? Or take a concrete example. First of all, creating a module is exactly the same as the original creation of a Java project, take the `oop-module` project as an example, it has the following directory structure:

```ascii
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

The `bin` directory holds the compiled class files, the `src` directory holds the source code, which is stored according to the directory structure of the package name, and just under the `src` directory, there is an extra file `module-info.java`, which is the description file of the module. In this module, it looks like this:

```java
module hello.world {
    requires java.base; // 可不写，任何模块都会自动引入java.base
    requires java.xml;
}
```

Where `module` is the keyword, followed by `hello.world` is the name of the module, which follows the package naming convention. The `requires xxx;` in parentheses indicates other module names that this module needs to reference. In addition to `java.base` which can be automatically introduced, here we introduce a `java.xml` module.

The introduced module can be used only when we have declared dependencies using the module. For example, `Main.java` code is as follows:

```java
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
    public static void main(String[] args) {
    	Greeting g = new Greeting();
    	System.out.println(g.hello(XMLConstants.XML_NS_PREFIX));
    }
}
```

If you remove `requires java.xml;` from `module-info.java`, the compilation will report an error. As you can see, the important role of modules is to declare dependencies.

Below, we compile and create the module using the command line tools provided by the JDK.

First, we switch our working directory to `oop-module` and compile all the `.java` files in the current directory and store them in the `bin` directory with the following command:

```plain
$ javac -d bin src/module-info.java src/com/itranswarp/sample/*.java
```

If the compilation is successful, the project structure is now as follows:

```ascii
oop-module
├── bin
│   ├── com
│   │   └── itranswarp
│   │       └── sample
│   │           ├── Greeting.class
│   │           └── Main.class
│   └── module-info.class
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

Notice that `module-info.java` in the `src` directory is compiled to `module-info.class` in the `bin` directory.

Next, we need to package all the class files in the bin directory into a jar first, and while doing so, take care to pass in the `-main-class` parameter so that the jar package can locate the class where the `main` method is on its own:

```plain
$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```

Now we've got the `hello.jar` jar package in the current directory, which is no different from a regular jar package, and you can run it directly with the command `java -jar hello.jar`. But our goal is to create modules, so go ahead and use the `jmod` command that comes with the JDK to convert a jar package into a module:

```plain
$ jmod create --class-path hello.jar hello.jmod
```

So, in the current directory we get the module file `hello.jmod` again, which is the legendary module that is finally packaged!

### Run the module

To run a jar, we use the `java -jar xxx.jar` command. To run a module, we just specify the module name. Try:

```plain
$ java --module-path hello.jmod --module hello.world
```

It turned out to be a mistake:

```plain
Error occurred during initialization of boot layer
java.lang.module.FindException: JMOD format not supported at execution time: hello.jmod
```

The reason is that `.jmod` cannot be put into `-module-path`. Replace it with `.jar` and you'll be fine:

```plain
$ java --module-path hello.jar --module hello.world
Hello, xml!
```

So what's the use of `hello.jmod` that we worked so hard to create? The answer is that we can use it to package the JRE.

### Package the JRE

As mentioned earlier, in order to support modularity, Java 9 first took the lead in splitting its one gigantic `rt.jar` into dozens of `.jmod` modules, the reason being that, when running a Java program, there aren't really that many JDK modules that we use. Modules that you don't need can be deleted.

In the past, to publish a Java application, to run it, you must download a complete JRE, and then run the jar package. The full JRE is a huge chunk, more than 100 M. How to slim down the JRE?

Now that the JRE's own standard library has been split into modules, only the modules used by the program need to be brought along, and the rest can be trimmed away. How to cut the JRE? Instead of removing some of the modules from the JRE installed on your system, you can make a copy of the JRE, but only with the modules you need. To do this, the JDK provides the `jlink` command. The command is as follows:

```plain
$ jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

We specify our own module `hello.jmod` in the `-module-path` parameter, and then, in the `--add-modules` parameter, we specify the three modules we use `java.base`, `java.xml` and `hello.world`, separated by `,`. Finally, the output directory is specified in the `--output` parameter.

Now, in the current directory, we can find the `jre` directory, which is a complete JRE with our own `hello.jmod` module. try running this JRE directly:

```plain
$ jre/bin/java --module hello.world
Hello, xml!
```

To distribute our own Java applications, we just need to make a package of the `jre` directory and send it to the other party, and the other party can directly run the above commands, without having to download and install the JDK or know how to configure our own modules, which greatly facilitates distribution and deployment.

### Access rights

As we said earlier, Java's class access rights are divided into public, protected, private and the default package access rights. After the introduction of the module, the rules of these access rights should be slightly adjusted.

To be precise, these access rights for classes are only valid within a module, module to module, e.g., for module a to have access to a certain class in module b, the necessary condition is that module b explicitly exports packages that can be accessed.

As an example: the module `hello.world` that we wrote uses a class `javax.xml.XMLConstants` from module `java.xml`, and we are able to use this class directly because of a number of exports declared in `module-info.java` of module `java.xml`:

```java
module java.xml {
    exports java.xml;
    exports javax.xml.catalog;
    exports javax.xml.datatype;
    ...
}
```

External code is only allowed access if it declares an exported package. In other words, if external code wants to access the `com.itranswarp.sample.Greeting` class in our `hello.world` module, we must export it:

```java
module hello.world {
    exports com.itranswarp.sample;

    requires java.base;
    requires java.xml;
}
```

Thus, the module further isolates access to the code.

### Exercise

Please download and practice how to package modules and JREs.

[Download Exercise](oop-module.zip)

### Summary

The purpose of modules introduced in Java 9 is to manage dependencies;

JREs can be packaged on demand using modules;

Access to classes is further restricted using modules.