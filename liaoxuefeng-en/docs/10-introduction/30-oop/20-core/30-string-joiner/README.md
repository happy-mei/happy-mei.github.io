<!-- TRANSLATED by md-translate -->
# StringJoiner

To splice strings efficiently, you should use `StringBuilder`.

Many times, we splice strings like this:

```java
// 输出: Hello Bob, Alice, Grace!
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sb = new StringBuilder();
        sb.append("Hello ");
        for (String name : names) {
            sb.append(name).append(", ");
        }
        // 注意去掉最后的", ":
        sb.delete(sb.length() - 2, sb.length());
        sb.append("!");
        System.out.println(sb.toString());
    }
}
```

The need to splice arrays with delimiters like this is common, so the Java standard library also provides a `StringJoiner` to do this:

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```

Slowly! The result with `StringJoiner` is missing the leading `"Hello"` and the ending `"!" `! In this case, you need to specify `Beginning` and `End` for `StringJoiner`:

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ", "Hello ", "!");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```

### String.join()

`String` also provides a static method `join()`, which internally uses `StringJoiner` to concatenate strings, and `String.join()` is more convenient when you don't need to specify the `beginning' and `end'. String.join()` is more convenient when you don't need to specify the `beginning` and `end`:

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

### Exercise

Please construct a `SELECT` statement using `StringJoiner`:

```java
import java.util.StringJoiner;

public class Main {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String select = buildSelectSql(table, fields);
        System.out.println(select);
        System.out.println("SELECT name, position, salary FROM employee".equals(select) ? "测试成功" : "测试失败");
    }

    static String buildSelectSql(String table, String[] fields) {
        // TODO:
        return "";
    }
}
```

[Download exercise](core-stringjoiner.zip)

### Summary

It is more convenient to use `StringJoiner` or `String.join()` when splicing an array of strings with the specified separator;

When splicing strings with `StringJoiner`, it is possible to append an additional "beginning" and "end".