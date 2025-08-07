<!-- TRANSLATED by md-translate -->
# Java program basic structure

We start by dissecting a complete Java program and what its basic structure is:

```java
/**
 * 可以用来自动创建文档的注释
 */
public class Hello {
    public static void main(String[] args) {
        // 向屏幕输出文本:
        System.out.println("Hello, world!");
        /* 多行注释开始
        注释内容
        注释结束 */
    }
} // class定义结束
```

Because Java is an object-oriented language, the basic unit of a program is a `class`, and `class` is the keyword; the name of the `class` defined here is `Hello`:

```java
public class Hello { // 类名是Hello
    // ...
} // class定义结束
```

Class name requirements:

* Class names must begin with a letter followed by a combination of letters, numbers and underscores
* Customarily begin with a capital letter

Be careful to follow naming conventions and good class naming:

* Hello
* NoteBook
* VRPlayer

Bad class naming:

* hello
* Good123
* Note_Book
* _World

Notice that `public` is an access modifier indicating that the `class` is public.

Without `public`, it will compile correctly, but the class will not be executable from the command line.

Within `class`, several methods can be defined:

```java
public class Hello {
    public static void main(String[] args) { // 方法名是main
        // 方法代码...
    } // 方法定义结束
}
```

A method defines a set of execution statements, and the code inside the method will be executed in sequential order.

Here the method name is `main` and the return value is `void`, indicating that there is no return value.

We notice that `public` can modify methods in addition to `class`. And the keyword `static` is another modifier which denotes a static method, and we will explain the types of methods later, for now, all we need to know is that the methods specified by the Java entry program must be static methods, the method name must be `main`, and the arguments in parentheses must be String arrays.

There are also naming rules for method names, named like `class`, but with lowercase initial letters:

Good way to name it:

* main
* goodMorning
* playVR

Bad way to name it:

* Main
* good123
* good_morning
* _playVR

Inside the method, the statement is the actual code that is executed.Each line of Java must be terminated with a semicolon:

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!"); // 语句
    }
}
```

In a Java program, comments are a kind of text for people to read, not part of the program, so the compiler automatically ignores them.

Java has 3 types of comments, the first is a single line comment that begins with a double slash and ends with a double slash until the end of the line:

```java
// 这是注释...
```

Multi-line comments, on the other hand, begin with a `/*` asterisk and end with `*/` and can have multiple lines:

```java
/*
这是注释
blablabla...
这也是注释
*/
```

There is also a special multi-line comment that starts with `/**` and ends with `*/`, and if there are multiple lines, each line usually starts with an asterisk:

```java
/**
 * 可以用来自动创建文档的注释
 * 
 * @auther liaoxuefeng
 */
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

This special multi-line comment needs to be written at the definition of classes and methods and can be used to automatically create documentation.

Java programs do not have a clear requirement for the format, a few more spaces or carriage returns do not affect the correctness of the program, but we have to develop good programming habits, pay attention to comply with the Java community agreed coding format.

What are the requirements of that agreed coding format? In fact, we have introduced the Eclipse IDE provides a shortcut `Ctrl + Shift + F ` (macOS is `⌘ + ⇧ + F `) to help us quickly format the code, Eclipse is in accordance with the agreed coding format of the code formatting , so just need to look at the formatted code after what looks like on the line. Specific code formatting requirements can be in the Eclipse settings ` Java ` - ` Code Style ` view.