<!-- TRANSLATED by md-translate -->
# Inputs and outputs

### Output

In the previous code, we always use `System.out.println()` to output something to the screen.

`println` is an abbreviation for print line, which means output with line breaks. Therefore, if you don't want to break lines after output, you can use `print()`:

```java
// 输出
public class Main {
    public static void main(String[] args) {
        System.out.print("A,");
        System.out.print("B,");
        System.out.print("C.");
        System.out.println();
        System.out.println("END");
    }
}
```

Note the effect of the execution of the above code.

### Formatting output

Java also provides the ability to format output. Why format output? Because computer representations of data are not always suitable for human reading:

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        double d = 12900000;
        System.out.println(d); // 1.29E7
    }
}
```

If we want to display the data in the format we expect, we need to use the formatted output function. Formatted output uses `System.out.printf()`, and by using the placeholder `%? `, `printf()` can format the arguments that follow into the specified format:

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        double d = 3.1415926;
        System.out.printf("%.2f\n", d); // 显示两位小数3.14
        System.out.printf("%.4f\n", d); // 显示4位小数3.1416
    }
}
```

Java's formatting features provide a variety of placeholders to "format" various data types into a specified string:

| placeholders | description |
|-------|------|
| %d | Formatted output integer | %x | Formatted output hexadecimal integer | %x
| %x | Format output hexadecimal integer |
| %f | Formatted to output a floating point number | %e | Formatted to output a floating point number
| %e | Formatted to output scientific notation floats | %s | Formatted to output hexadecimal integers | %f | Formatted to output floats
| %s | Format string | %f | Format float in scientific notation | %e | Format float in scientific notation | %e

Note that since `%` represents a placeholder, two consecutive `%%`s represent a `%` character itself.

The placeholders themselves can also have more detailed formatting parameters. The following example formats an integer into hexadecimal with 0's complementing the 8 bits:

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        int n = 12345000;
        System.out.printf("n=%d, hex=%08x", n, n); // 注意，两个%占位符必须传入两个数
    }
}
```

For detailed formatting parameters, please refer to the JDK documentation [java.util.Formatter](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Formatter.html#syntax )

### Input

Compared to output, Java's input is much more complex.

Let's start with an example of reading a string and an integer from the console:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```

First, we import `java.util.Scanner` through the `import` statement, `import` is a statement to import a class, must be placed at the beginning of the Java source code, later we will explain how to use `import` in detail in the `package` of Java.

Then, create the `Scanner` object and pass in `System.in`. `System.out` represents the standard output stream, while `System.in` represents the standard input stream. Reading user input directly using `System.in` is possible but requires more complex code, whereas passing `Scanner` simplifies the subsequent code.

With the `Scanner` object, to read a string entered by the user, use `scanner.nextLine()`, and to read an integer entered by the user, use `scanner.nextInt()`. `Scanner` automatically converts data types, so there is no need to convert them manually.

To test the input, the user input must be read from the command line, so the process of compilation and execution needs to be followed:

```plain
$ javac Main.java
```

If this program compiles with a warning, you can ignore it for now and explain it in detail later when you learn IO. After successful compilation, execute it:

```plain
$ java Main
Input your name: Bob ◀── 输入 Bob
Input your age: 12   ◀── 输入 12
Hi, Bob, you are 12  ◀── 输出
```

After entering a string and integer respectively as prompted, we get the formatted output.

### Exercise

Please help Xiaoming's classmates design a program that inputs the last test score (int) and the current test score (int), and then outputs the percentage improvement in the score, retaining two decimal places (e.g., 21.75%).

[Download exercise](flow-input-output.zip)

### Summary

The output provided by Java includes `System.out.println()` / `print()` / `printf()`, where `printf()` formats the output;

Java provides Scanner object to facilitate input, read the corresponding type can be used: `scanner.nextLine()` / `nextInt()` / `nextDouble()` / ...