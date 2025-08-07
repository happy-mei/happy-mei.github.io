<!-- TRANSLATED by md-translate -->
# Strings and codes

### String

In Java, `String` is a reference type, which is itself a `class`. However, the Java compiler has special treatment for `String`, which means that you can directly use `"..." ` to represent a string:

```java
String s1 = "Hello!";
```

Strings are actually represented internally in `String` by a `char[]` array, so it's fine to write it as follows:

```java
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

Because `String` is so commonly used, Java provides `"..." ` as a string literal representation.

An important feature of Java strings is that they are _immutable_. This immutability is achieved through the internal `private final char[]` field, and the absence of any method of modifying `char[]`.

Let's look at an example:

```java
// String
public class Main {
    public static void main(String[] args) {
        String s = "Hello";
        System.out.println(s);
        s = s.toUpperCase();
        System.out.println(s);
    }
}
```

Based on the output of the above code, try to explain whether the content of the string has changed or not.

### String comparison

When we want to compare two strings to see if they are the same, pay special attention to the fact that we are actually trying to compare the contents of the strings to see if they are the same. The `equals()` method must be used and not `==`.

Let's look at the example below:

```java
// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "hello";
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

On the surface, two strings compared with `==` and `equals()` are both `true`, but in fact that's just the Java compiler in the compilation period, it will automatically treat all the same strings as an object into the constant pool, naturally `s1` and `s2` references are the same.

So it is purely coincidental that this `==` comparison returns `true`. Written differently, the `==` comparison fails:

```java
// String
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

Conclusion: to compare two strings, the `equals()` method must always be used.

To ignore case comparisons, use the `equalsIgnoreCase()` method.

The `String` class also provides a variety of methods to search for substrings and extract substrings. The commonly used methods are:

```java
// 是否包含子串:
"Hello".contains("ll"); // true
```

Notice that the argument to the `contains()` method is `CharSequence` and not `String`, because `CharSequence` is an interface implemented by `String`.

More examples of searching for substrings:

```java
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true
```

Example of extracting a substring:

```java
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
```

Note that the index numbers start at `0`.

### Remove leading and trailing blank characters

Use the `trim()` method to remove whitespace characters from the beginning and end of a string. Whitespace characters include spaces, `\t`, `\r`, `\n`:

```java
"  \tHello\r\n ".trim(); // "Hello"
```

Note: `trim()` does not change the contents of the string, it returns a new string.

Another `strip()` method also removes whitespace characters from the beginning and end of a string. It differs from `trim()` in that the Chinese-like space character \u3000` is also removed:

```java
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```

`String` also provides `isEmpty()` and `isBlank()` to determine whether a string is empty and a blank string:

```java
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```

### Replace the substring

To replace a substring in a string, there are two methods. One is to replace based on characters or strings:

```java
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"
```

The other is by regular expression substitution:

```java
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"
```

The above code uniformly replaces the matched substrings with `","` by regular expressions. We will explain the usage of regular expressions in detail later.

### Splitting strings

To split a string, use the `split()` method and pass in a regular expression as well:

```java
String s = "A,B,C,D";
String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}
```

### Splicing strings

String concatenation uses the static method `join()`, which joins an array of strings with the specified string:

```java
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"
```

### Formatting strings

Strings provide the `formatted()` method and the `format()` static method, which can be passed additional arguments, placeholders replaced, and a new string generated:

```java
// String
public class Main {
    public static void main(String[] args) {
        String s = "Hi %s, your score is %d!";
        System.out.println(s.formatted("Alice", 80));
        System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));
    }
}
```

There are several placeholders, followed by several parameters passed in. The parameter types should be the same as the placeholders. We often use this to format information. Commonly used placeholders are:

* `%s`: displays a string;
* `%d`: displays integers;
* `%x`: displays hexadecimal integers;
* `%f`: displays floating point numbers.

Placeholders can also be formatted, e.g. `%.2f` means to display two decimals. If you're not sure what placeholder to use, then always use `%s`, since `%s` can display any data type. To see the full formatting syntax, see the [JDK documentation](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Formatter.html#syntax).

### Type conversion

To convert any basic or reference type to a string, use the static method `valueOf()`. This is an overloaded method and the compiler will automatically choose the appropriate method based on the arguments:

```java
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```

To convert strings to other types, it depends on the situation. For example, to convert a string to type `int`:

```java
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
```

Converts a string to a `boolean` type:

```java
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false
```

In particular, note that `Integer` has a `getInteger(String)` method that, instead of converting a string to an `int`, converts the system variable corresponding to that string to an `Integer`:

```java
Integer.getInteger("java.version"); // 版本号，11
```

### Converted to char[]

The types `String` and `char[]` can be converted to each other by:

```java
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```

If the `char[]` array is modified, `String` does not change:

```java
// String <-> char[]
public class Main {
    public static void main(String[] args) {
        char[] cs = "Hello".toCharArray();
        String s = new String(cs);
        System.out.println(s);
        cs[0] = 'X';
        System.out.println(s);
    }
}
```

This is because when a new `String` instance is created via `new String(char[])`, it doesn't directly reference the incoming `char[]` array, it makes a copy of it, so modifying the external `char[]` array won't affect the `char[]` array inside the `String` instance, because it's two different arrays.

It is clear from the design of `String`'s invariant that we need to copy rather than directly reference the passed-in object if it is likely to change.

For example, the following code is designed to hold the grades of a group of students in a `Score` class:

```java
// int[]
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] scores = new int[] { 88, 77, 51, 66 };
        Score s = new Score(scores);
        s.printScores();
        scores[2] = 99;
        s.printScores();
    }
}

class Score {
    private int[] scores;
    public Score(int[] scores) {
        this.scores = scores;
    }

    public void printScores() {
        System.out.println(Arrays.toString(scores));
    }
}
```

Observing the two outputs, since `Score` internally directly references the externally passed `int[]` array, this can cause modifications to the `int[]` array by the external code, affecting the fields of the `Score` class. This creates a security risk if the external code is not trustworthy.

Please fix the constructor method of `Score` so that modifications to the array by external code do not affect the `int[]` field of the `Score` instance.

### Character encoding

In early computer systems, to encode characters, the American National Standard Institute (ANSI) developed a set of codes for English letters, numbers, and commonly used symbols, which occupied a single byte and ranged from `0` to `127`, with the highest bit always being `0`, known as the `ASCII` encoding. For example, the character `'A'` is encoded as `0x41`, and the character `'1'` is encoded as `0x31`.

If Chinese characters are to be included in computer coding, it is clear that one byte is not enough. The `GB2312' standard uses two bytes to represent a Chinese character, where the highest bit of the first byte is always `1' in order to distinguish it from the `ASCII' code. For example, the `GB2312' code for the Chinese character ``zhong'' is `0xd6d0'.

Similarly, there are `Shift_JIS` encodings for Japanese and `EUC-KR` encodings for Korean, and these encodings, because of the lack of standardization, are used at the same time, which creates conflicts.

In order to standardize the encoding of all languages around the world, the Globally Harmonized Code Consortium issued the `Unicode' encoding, which incorporates the major languages of the world into the same encoding so that Chinese, Japanese, Korean and other languages do not conflict.

`Unicode` encoding requires two or more bytes to represent, we can compare the encoding of Chinese and English characters in `ASCII`, `GB2312` and `Unicode`:

The `ASCII` and `Unicode` encoding of the English character `'A'`:

```ascii
┌────┐
ASCII:   │ 41 │
         └────┘
         ┌────┬────┐
Unicode: │ 00 │ 41 │
         └────┴────┘
```

The `Unicode` encoding of English characters is simply adding a `00` byte to the front.

The `GB2312` and `Unicode` encoding of the Chinese character ``Middle``:

```ascii
┌────┬────┐
GB2312:  │ d6 │ d0 │
         └────┴────┘
         ┌────┬────┐
Unicode: │ 4e │ 2d │
         └────┴────┘
```

What is the `UTF-8` encoding that we often use? Because the high byte of the `Unicode` code for English characters is always `00`, and the text containing a large number of English characters will waste space, so `UTF-8` encoding appeared, which is a kind of variable-length encoding, used to turn the fixed-length `Unicode` code into a variable-length encoding of 1 to 4 bytes. With `UTF-8` encoding, the `UTF-8` encoding of the English character `'A` becomes `0x41`, which is exactly the same as the `ASCII` code, while the `UTF-8` encoding of the Chinese character `'zhong` is 3 bytes `0xe4b8ad`.

Another advantage of `UTF-8' encoding is its fault tolerance. If some characters are incorrectly transmitted, the subsequent characters are not affected because `UTF-8' encoding relies on the high byte bit to determine how many bytes a character is, and it is often used as a transmission encoding.

In Java, the `char` type is actually the two-byte `Unicode` encoding. If we want to manually convert a string to another encoding, we can do so:

```java
byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换
```

Note: After converting the encoding, it is no longer of type `char`, but an array represented by type `byte`.

To convert a `byte[]` with a known encoding to a `String`, do this:

```java
byte[] b = ...
String s1 = new String(b, "GBK"); // 按GBK转换
String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换
```

Always keep in mind: Java's `String` and `char` are always represented in memory as Unicode encodings.

### Read more

For different versions of the JDK, the `String` class has different optimizations in memory. Specifically, `String` in early JDK versions is always stored as `char[]`, which is defined as follows:

```java
public final class String {
    private final char[] value;
    private final int offset;
    private final int count;
}
```

Newer JDK versions of `String`, on the other hand, store `String` as `byte[]`: one character per `byte` if the `String` contains only ASCII characters, and one character for every two `bytes` otherwise; this is done to conserve memory because a large number of `String`s of short lengths usually contain only ASCII characters:

```java
public final class String {
    private final byte[] value;
    private final byte coder; // 0 = LATIN1, 1 = UTF16
```

For the user, the `String` internal optimizations do not affect any existing code, since its `public` method signature is unchanged.

### Summary

The Java string `String` is an immutable object;

String operations do not change the contents of the original string, but return a new string;

Commonly used string operations: extract substrings, find, replace, case conversion, etc;

Java uses Unicode to represent `String` and `char`;

Converting an encoding is converting `String` to `byte[]`, and requires the encoding to be specified;

When converting to `byte[]`, always give preference to `UTF-8` encoding.