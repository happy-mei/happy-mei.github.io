<!-- TRANSLATED by md-translate -->
# Install JDK

Because Java programs must run on top of the JVM, the first thing we do is install the JDK.

Search for JDK 23 and make sure you download the latest stable version of the JDK from [Oracle's official website](https://www.oracle.com/java/technologies/downloads/):

```ascii
Java SE Development Kit 23 downloads

Linux macOS Windows
              -------

x64 Compressed Archive Download
x64 Installer Download
x64 MSI Installer Download
```

Choose the right operating system and installer, find the download link `Download` for Java SE 23, download and install it. for Windows choose `x64 MSI Installer` in preference, for Linux and macOS choose the right installer according to your computer's CPU whether it is ARM or x86.

### Setting environment variables

After installing the JDK, you need to set a `JAVA_HOME` environment variable, which points to the JDK installation directory. Under Windows, it is the installation directory, similarly:

```plain
C:\Program Files\Java\jdk-23
```

Under macOS, it's in `~/.bash_profile` or `~/.zprofile`, which it is:

```plain
export JAVA_HOME=`/usr/libexec/java_home -v 23`
```

Then, append the `bin` directory of `JAVA_HOME` to the system environment variable `PATH`. On Windows, it looks like this:

```plain
Path=%JAVA_HOME%\bin;<现有的其他路径>
```

Under macOS, it's in `~/.bash_profile` or `~/.zprofile` and looks like this:

```plain
export PATH=$JAVA_HOME/bin:$PATH
```

The reason for adding the `bin` directory of `JAVA_HOME` to the `PATH` is so that `java` can be run from any folder. Open a PowerShell window and enter the command `java -version`, if everything works you will see the following output:

```ascii
┌─────────────────────────────────────────────────────────┐
│Windows PowerShell                                 - □ x │
├─────────────────────────────────────────────────────────┤
│Windows PowerShell                                       │
│Copyright (C) Microsoft Corporation. All rights reserved.│
│                                                         │
│PS C:\Users\liaoxuefeng> java -version                   │
│java version "23" ...                                    │
│Java(TM) SE Runtime Environment                          │
│Java HotSpot(TM) 64-Bit Server VM                        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

If you see a version number that is not `23`, but `15`, `1.8` or something like that, it means that there is more than one JDK on the system and the default JDK is not JDK 23, so you need to mention JDK 23 in front of `PATH`.

If you get an error output that says: "Unable to recognize the item "java" as the name of a cmdlet, function, script file or runnable program." :

```ascii
┌─────────────────────────────────────────────────────────┐
│Windows PowerShell                                 - □ x │
├─────────────────────────────────────────────────────────┤
│Windows PowerShell                                       │
│Copyright (C) Microsoft Corporation. All rights reserved.│
│                                                         │
│PS C:\Users\liaoxuefeng> java -version                   │
│java : The term 'java' is not recognized as ...          │
│...                                                      │
│    + FullyQualifiedErrorId : CommandNotFoundException   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

This is because the system cannot find the program `java.exe` for the Java Virtual Machine, and you need to check the configuration of `JAVA_HOME` and `PATH`.

You can refer to [How to set or change PATH system variables](https://www.java.com/zh-CN/download/help/path.html).

### JDK

A careful reader can also find many executables in the `bin` directory of `JAVA_HOME`:

* java: this executable program is actually the JVM. To run a Java program is to start the JVM and then let the JVM execute the specified compiled code;
* javac: this is the compiler for Java, it is used to compile Java source code files (ending with `.java` suffix) into Java bytecode files (ending with `.class` suffix);
* jar: used to package a set of `.class` files into a `.jar` file for easy distribution;
* javadoc: used to automatically extract comments from Java source code and generate documentation;
* jdb: Java debugger for the development phase of the runtime debugging .
