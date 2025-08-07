<!-- TRANSLATED by md-translate -->
# Characters and strings

In Java, characters and strings are two different types.

### Character types

The character type `char` is the basic data type which is an abbreviation for `character`. A `char` holds one Unicode character:

```java
char c1 = 'A';
char c2 = '中';
```

Because Java always uses Unicode to represent characters in memory, an English character and a Chinese character are both represented by a `char` type, and they both take up two bytes. To display the Unicode encoding of a character, simply assign the `char` type directly to the `int` type:

```java
int n1 = 'A'; // 字母“A”的Unicodde编码是65
int n2 = '中'; // 汉字“中”的Unicode编码是20013
```

It is also possible to represent a character directly with the escape character `\u` + Unicode encoding:

```java
// 注意是十六进制:
char c3 = '\u0041'; // 'A'，因为十六进制0041 = 十进制65
char c4 = '\u4e2d'; // '中'，因为十六进制4e2d = 十进制20013
```

### String type

Unlike the `char` type, the string type `String` is a reference type, and we use double quotes `"..." ` to denote a string. A string can store from 0 to any number of characters:

```java
String s = ""; // 空字符串，包含0个字符
String s1 = "A"; // 包含一个字符
String s2 = "ABC"; // 包含3个字符
String s3 = "中文 ABC"; // 包含6个字符，其中有一个空格
```

Since strings use double quotes `"..." ` to indicate the beginning and the end, what if the string itself happens to contain a `"` character? For example, `"abc "xyz"`, the compiler will not be able to determine whether the middle quote is part of the string or the end of the string. In this case, we need to use the escape character `\`:

```java
String s = "abc\"xyz"; // 包含7个字符: a, b, c, ", x, y, z
```

Since `\` is an escape character, two `\\`s represent one `\` character:

```java
String s = "abc\\xyz"; // 包含7个字符: a, b, c, \, x, y, z
```

Common escape characters include:

* `\"` indicates the character `"`.
* `\'` indicates the character `'`.
* `\\\` indicates the character `\\`.
* `\n` indicates a line feed.
* `\r` indicates a carriage return.
* `\t` for Tab
* `\u=####` indicates a Unicode encoded character

Example:

```java
String s = "ABC\n\u4e2d\u6587"; // 包含6个字符: A, B, C, 换行符, 中, 文
```

### String concatenation

Java's compiler takes special care of strings by allowing you to use `+` to connect any string to other data types, which greatly facilitates string handling. For example:

```java
// 字符串连接
public class Main {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "world";
        String s = s1 + " " + s2 + "!";
        System.out.println(s); // Hello world!
    }
}
```

If you concatenate strings and other data types with `+`, it will automatically transform the other data types to strings first before concatenating them:

```java
// 字符串连接
public class Main {
    public static void main(String[] args) {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s); // age is 25
    }
}
```

### Multi-line strings

If we want to represent a multi-line string, it would be very inconvenient to concatenate it with a + sign:

```java
String s = "first line \n"
         + "second line \n"
         + "end";
```

Starting with Java 13, strings can now be represented by `"""""...""" ` to represent multi-line strings (Text Blocks) now. An example:

```java
// 多行字符串
public class Main {
    public static void main(String[] args) {
        String s = """
                   SELECT * FROM
                     users
                   WHERE id > 100
                   ORDER BY name DESC
                   """;
        System.out.println(s);
    }
}
```

The above multi-line string is actually 5 lines with a `\n` after the last `DESC`. If we don't want a `\n` at the end of the string, this is how we need to write it:

```java
String s = """ 
           SELECT * FROM
             users
           WHERE id > 100
           ORDER BY name DESC""";
```

It is also important to note that spaces common to the front of multi-line strings are removed, ie:

```java
String s = """
...........SELECT * FROM
........... users
...........WHERE id > 100
...........ORDER BY name DESC
...........""";
```

Spaces marked with `. ` are labeled with spaces that are removed.

If a multi-line string has irregular typography, then the removed spaces look like this:

```java
String s = """
......... SELECT * FROM
......... users
.........WHERE id > 100
......... ORDER BY name DESC
.........  """;
```

That is, it is always based on the shortest space at the beginning of the line.

### Irrevocable properties ###

An important feature of Java's string, besides being a reference type, is that the string is immutable. Examine the following code:

```java
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        System.out.println(s); // 显示 hello
        s = "world";
        System.out.println(s); // 显示 world
    }
}
```

Observe the result, did the string `s` change? Actually, it is not the string that has changed, but the variable `s` that has changed.

When executing `String s = "hello";`, the JVM virtual machine first creates the string `"hello"` and then, points the string variable `s` to it:

```ascii
s
      │
      ▼
┌───┬───────────┬───┐
│   │  "hello"  │   │
└───┴───────────┴───┘
```

Immediately afterward, when executing `s = "world";`, the JVM virtual machine first creates the string `"world"` and then, points the string variable `s` to it:

```ascii
s ──────────────┐
                      │
                      ▼
┌───┬───────────┬───┬───────────┬───┐
│   │  "hello"  │   │  "world"  │   │
└───┴───────────┴───┴───────────┴───┘
```

The original string `"hello"` is still there, we just can't access it through the variable `s`. Thus, the immutability of a string means that the content of the string is immutable. As for the variable, it can point to the string `"hello"` at one time and to the string `"world"` at another.

After understanding the "pointing" of a reference type, try to interpret the following code output:

```java
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        String t = s;
        s = "world";
        System.out.println(t); // t是"hello"还是"world"?
    }
}
```

### The null value null

A variable of reference type can point to a null value `null`, which indicates non-existence, i.e. the variable does not point to any object. Example:

```java
String s1 = null; // s1是null
String s2 = s1; // s2也是null
String s3 = ""; // s3指向空字符串，不是null
```

Note the distinction between the null value `null` and the empty string `""`, which is a valid string object that is not equal to `null`.

### Exercise

Please treat a set of int values as the Unicode encoding of a character and then put them together into a string:

```java
public class Main {
    public static void main(String[] args) {
        // 请将下面一组int值视为字符的Unicode码，把它们拼成一个字符串：
        int a = 72;
        int b = 105;
        int c = 65281;
        // FIXME:
        String s = a + b + c;
        System.out.println(s);
    }
}
```

[Download Exercise](basic-char-string.zip)

### Summary

Java's character type `char` is a basic type and the string type `String` is a reference type;

Basic variables "hold" a value, while reference variables "point" to an object;

Variables of reference type can be null `null`;

To distinguish between the null value `null` and the empty string `""`.