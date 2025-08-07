<!-- TRANSLATED by md-translate -->
# Java history

Java was first developed by SUN's (which has been acquired by Oracle) [James Gosling](https://en.wikipedia.org/wiki/James_Gosling) (High Commander, known as the father of Java) in the early 1990s as a programming language, initially named Oak, targeted at Embedded applications for small home appliance devices, resulting in little market response. Who expected the rise of the Internet, so that Oak revitalized, so SUN revamped Oak, officially released in 1995 under the name of Java, the reason is that Oak has been registered, so SUN registered the trademark Java. With the rapid development of the Internet, Java gradually became the most important network programming language.

Java lies between compiled languages and interpreted languages. Compiled languages, such as C and C++, have code that is compiled directly into machine code for execution, but different platforms (x86, ARM, etc.) have different instruction sets for their CPUs, so it is necessary to compile the corresponding machine code for each platform. Interpreted languages such as Python and Ruby do not have this problem, and can be loaded directly by the interpreter into the source code and then run, at the cost of running too inefficiently. Java is to compile the code into a "byte code", which is similar to the abstract CPU instructions, and then, for different platforms to write a virtual machine, the virtual machine of different platforms is responsible for loading the byte code and execution, so as to realize the "once written, run everywhere! This realizes the effect of "write once, run everywhere". Of course, this is for Java developers. For the virtual machine, you need to develop for each platform separately. In order to ensure that different platforms, virtual machines developed by different companies can correctly execute Java byte code, SUN has developed a series of Java virtual machine specifications. From a practical point of view, the JVM compatibility is done very well, the lower version of the Java byte code can be completely normal run on the higher version of the JVM.

With the development of Java, SUN gave Java another three different versions:

* Java SE: Standard Edition
* Java EE: Enterprise Edition
* Java ME: Micro Edition

What is the relationship between these three?

```ascii
┌───────────────────────────┐
│Java EE                    │
│    ┌────────────────────┐ │
│    │Java SE             │ │
│    │    ┌─────────────┐ │ │
│    │    │   Java ME   │ │ │
│    │    └─────────────┘ │ │
│    └────────────────────┘ │
└───────────────────────────┘
```

Simply put, Java SE is the standard edition, including the standard JVM and standard libraries, while Java EE is the enterprise edition, which is just based on Java SE plus a large number of APIs and libraries, in order to facilitate the development of Web applications, databases, messaging services, etc., Java EE applications using the virtual machine and Java SE is exactly the same.

Java ME is different from Java SE, it is a "thin version" for embedded devices, Java SE's standard libraries can not be used on Java ME, Java ME's virtual machine is also a "thin version".

There is no doubt that Java SE is the core of the entire Java platform, and Java EE is necessary for further learning about web applications. We are familiar with frameworks such as Spring that are part of the Java EE open source ecosystem. Unfortunately, Java ME never really caught on, instead Android development became one of the standards for mobile platforms, so learning Java ME is not recommended without special needs.

So our recommended roadmap for learning Java is as follows:

1. First of all, you need to learn Java SE, master the Java language itself, Java core development techniques and the use of Java standard libraries;
2. If you continue to learn Java EE, then Spring framework, database development, distributed architecture is what you need to learn;
3. if you want to learn big data development, then Hadoop, Spark, Flink, these big data platforms is what you need to learn, they are based on Java or Scala development;
4. if you want to learn mobile development, then in-depth Android platform, master Android App development.

Regardless of the choice, the core technology of Java SE is fundamental, and this tutorial is designed to make you fully proficient in Java SE and master Java EE!

### Java version

Starting with the release of version 1.0 in 1996, the latest version of Java to date is Java 21:

| Time | Version |
|------|------|
| 1996 | 1.0 |
| 1997 | 1.1 | 1998 | 1.2 | 1.1
1998 | 1.2 | 2000 | 1.3 | 2000 | 1.4
2000 | 1.3 | 2002 | 1.4 | 1.4
2002 | 1.4 | 2004 | 1.5 | 1.5 | 1.6
2004 | 1.5 / 5.0 | 2005 | 1.6 / 5.0 | 2005 | 1.6 / 5.0
2005 | 1.6 / 6.0 | 2011 | 1.7 / 6.0 | 2011 | 1.7 / 6.0
2011 | 1.7 / 7.0 | 2014 | 1.8 / 7.0
| 2014 | 1.8 / 8.0 | 2017/9 | 1.8 / 8.0 | 2017/9 | 1.8 / 8.0
| 2017/9 | 1.9 / 9.0 | 2018/3 | 10
| The Government of the Hong Kong Special Administrative Region
| The Government of the Hong Kong Special Administrative Region
| The Government of the Hong Kong Special Administrative Region
| The Government of the Hong Kong Special Administrative Region (HKSAR) has been working closely with the Government of the HKSAR on the development of a new national education system.
| The Government of the Hong Kong Special Administrative Region (HKSAR) has been working closely with the Government of the HKSAR on the implementation of the recommendations of the Committee.
| The Government of the Hong Kong Special Administrative Region (HKSAR) will continue its efforts to promote the development of the HKSAR in the coming years.
| The Government of the Hong Kong Special Administrative Region (HKSAR) has been working closely with the Government of the HKSAR on the development of a new government.
| The Government of the Hong Kong Special Administrative Region (HKSAR) will continue its efforts to promote the development of the HKSAR in the coming years.
2022/3 | 18 | 2022/9 | 2022/9 | 2022/9 | 2022/9 | 2022/9
2022/9 | 19 | 2023/3 | 2023/3 | 2023/3 | 2023/3 | 2023/3 | 2023/3
2023/3 | 20 | 20 | 20 | 2023/9 | 20 | 20 | 20 | 20 | 20 | 20
2023/9 | 21 | 2024/3 | 22 | 2024/9
2024/3 | 22 | 2024/9 | 2024/9 | 2024/9 | 2024/9 | 2024/9
| The Administration's response to the questionnaire was as follows

The version of Java used in this tutorial is the latest version **Java 23**.

### Explanation of terms

Beginners learn Java, often hear the terms JDK, JRE, what are they in the end?

* JDK: Java Development Kit
* JRE: Java Runtime Environment

Simply put, JRE is the virtual machine that runs Java bytecode. However, if there is only Java source code, to be compiled into Java byte code, you need the JDK, because the JDK in addition to the inclusion of the JRE, but also provides a compiler, debugger and other development tools.

The relationship between the two is as follows:

```ascii
┌─    ┌──────────────────────────────────┐
  │     │     Compiler, debugger, etc.     │
  │     └──────────────────────────────────┘
 JDK ┌─ ┌──────────────────────────────────┐
  │  │  │                                  │
  │ JRE │      JVM + Runtime Library       │
  │  │  │                                  │
  └─ └─ └──────────────────────────────────┘
        ┌───────┐┌───────┐┌───────┐┌───────┐
        │Windows││ Linux ││ macOS ││others │
        └───────┘└───────┘└───────┘└───────┘
```

To learn Java development, of course, you need to install the JDK.

What about JSR, JCP ......?

* JSR specification: Java Specification Request
* JCP Organization: Java Community Process

In order to ensure that the normative nature of the Java language, SUN has engaged in a JSR specification, where you want to add a function to the Java platform, such as access to the database, we must first create a JSR specification to define the interface, so that the various database vendors are in accordance with the specification to write a Java driver, the developer does not have to worry about their own database code written on the MySQL can run! but can't run on PostgreSQL.

So the JSR is a series of specifications that standardize everything from the memory model of the JVM to the web program interface. And the organization responsible for reviewing the JSRs is the JCP.

When a JSR specification is released, in order to allow everyone to have a reference, but also released a "reference implementation", as well as a "compatibility test suite":

* RI: Reference Implementation
* TCK: Technology Compatibility Kit

For example, someone proposed to get a Java-based message server , this proposal is very good ah , but only a proposal is not enough , you have to post the code can really run , which is the RI. if there are other people also want to develop such a message server , how to ensure that these message servers are the same interface, functionality for the developer ? So also have to provide TCK.

Generally speaking, RI is just a "can run" the right code, it does not seek speed, so if you really want to choose a Java messaging server, generally no one uses RI, everyone will choose a competitive commercial or open source products.

Reference: JSR for Java Messaging Service JMS: [https://jcp.org/en/jsr/detail?id=914](https://jcp.org/en/jsr/detail?id=914)

```question type=radio
请问Java之父是：
----
    James Bond
[x] James Gosling
    James Simons
```