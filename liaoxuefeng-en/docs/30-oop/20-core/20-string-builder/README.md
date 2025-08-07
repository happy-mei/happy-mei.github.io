<!-- TRANSLATED by md-translate -->
# StringBuilder

The Java compiler does a special treatment of `String` that allows us to splice strings directly with `+`.

Examine the following loop code:

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
```

Although it is possible to splice strings directly, in a loop, each loop creates a new string object and then throws away the old string. Thus, the vast majority of strings are temporary objects, which not only wastes memory, but also affects GC efficiency.

For efficient string splicing, the Java Standard Library provides `StringBuilder`, which is a mutable object that can be preallocated with a buffer, so that when new characters are added to `StringBuilder`, no new temporary objects are created:

```java
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();
```

`StringBuilder` can also perform chaining operations:

```java
// 链式操作
public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, ");
        System.out.println(sb.toString());
    }
}
```

If we look at the source code of `StringBuilder`, we can see that the key to chaining operations is that the `append()` method defined returns `this` so that other methods of itself can be called over and over again.

Modeled after `StringBuilder`, we can also design classes that support chaining operations. For example, a counter that can be incremented:

```java
// 链式操作
public class Main {
    public static void main(String[] args) {
        Adder adder = new Adder();
        adder.add(3)
             .add(5)
             .inc()
             .add(10);
        System.out.println(adder.value());
    }
}

class Adder {
    private int sum = 0;

    public Adder add(int n) {
        sum += n;
        return this;
    }

    public Adder inc() {
        sum ++;
        return this;
    }

    public int value() {
        return sum;
    }
}
```

Note: For normal string `+` operations, there is no need for us to rewrite them as `StringBuilder`, because the Java compiler automatically encodes multiple consecutive `+` operations as `StringConcatFactory` operations at compile time. At runtime, `StringConcatFactory` automatically optimizes string concatenation operations to array copy or `StringBuilder` operations.

You may also have heard of `StringBuffer`, a thread-safe version of `StringBuilder` from the early days of Java, which ensures that it is also safe for multiple threads to manipulate `StringBuffer` through synchronization, but synchronization brings about a slowdown in execution.

The `StringBuilder` and `StringBuffer` interfaces are identical, and there is no need to use `StringBuffer` at all now.

### Exercise

Please construct an `INSERT` statement using `StringBuilder`:

```java
public class Main {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String insert = buildInsertSql(table, fields);
        System.out.println(insert);
        String s = "INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)";
        System.out.println(s.equals(insert) ? "测试成功" : "测试失败");
    }

    static String buildInsertSql(String table, String[] fields) {
        // TODO:
        return "";
    }
}
```

[Download Exercise](core-stringbuilder.zip)

### Summary

`StringBuilder` are mutable objects used to splice strings efficiently;

`StringBuilder` can support chaining operations, and the key to implementing chaining operations is to return the instance itself;

`StringBuffer` is a thread-safe version of `StringBuilder`, which is rarely used these days.