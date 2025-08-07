<!-- TRANSLATED by md-translate -->
# NullPointerException

Of all the `RuntimeException` exceptions, the one Java programmers are most familiar with is probably the `NullPointerException`.

`NullPointerException` i.e. Null Pointer Exception, commonly known as NPE. if an object is `null`, calling its methods or accessing its fields will generate a `NullPointerException`, which is usually thrown by the JVM, for example:

```java
// NullPointerException
public class Main {
    public static void main(String[] args) {
        String s = null;
        System.out.println(s.toLowerCase());
    }
}
```

The concept of pointers actually originated in C. There are no pointers in Java. The variables we define are actually references, and a Null Pointer is more accurately called a Null Reference, although there is little difference between the two.

### Handling NullPointerException

What should we do if we encounter a `NullPointerException`? First of all, it must be clear that `NullPointerException` is a kind of code logic error, encounter `NullPointerException`, follow the principle of early exposure, early repair, strictly prohibit the use of `catch` to hide this kind of coding error:

```java
// 错误示例: 捕获NullPointerException
try {
    transferMoney(from, to, amount);
} catch (NullPointerException e) {
}
```

Good coding habits can greatly reduce the generation of `NullPointerException`, for example:

Member variables are initialized at definition time:

```java
public class Person {
    private String name = "";
}
```

Using the empty string `""` instead of the default `null` avoids a lot of `NullPointerException`, and it is much safer to write business logic with the empty string `""` to indicate that it is not filled in than `null`.

Returns the empty string `""`, the empty array instead of `null`:

```java
public String[] readLinesFromFile(String file) {
    if (getFileSize(file) == 0) {
        // 返回空数组而不是null:
        return new String[0];
    }
    ...
}
```

This makes it unnecessary for the caller to check if the result is `null`.

If the caller must judge based on `null`, such as returning `null` to indicate that the file does not exist, then consider returning `Optional<T>`:

```java
public Optional<String> readFromFile(String file) {
    if (!fileExist(file)) {
        return Optional.empty();
    }
    ...
}
```

This way the caller must determine if there is a result by `Optional.isPresent()`.

### Locating NullPointerException

If a `NullPointerException` is generated, e.g., a call to `a.b.c.x()` generates a `NullPointerException`, the cause may be:

* `a` is `null`;
* `a.b` is `null`;
* `a.b.c` is `null`;

Determining exactly which object is `null` can only print such a log until then:

```java
System.out.println(a);
System.out.println(a.b);
System.out.println(a.b.c);
```

Starting with Java 14, if a `NullPointerException` is thrown, the JVM can give us detailed information about who the `null` object actually is. Let's look at the example:

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        System.out.println(p.address.city.toLowerCase());
    }
}

class Person {
    String[] name = new String[2];
    Address address = new Address();
}

class Address {
    String city;
    String street;
    String zipcode;
}
```

It is possible to see in the `NullPointerException` details something like `... because "<local1>.address.city" is null`, meaning that the `city` field is `null`, so we can quickly pinpoint the problem.

This enhanced `NullPointerException` details is a new feature in Java 14, but is turned off by default, we can enable it by adding a `-XX:+ShowCodeDetailsInExceptionMessages` parameter to the JVM:

```plain
java -XX:+ShowCodeDetailsInExceptionMessages Main.java
```

### Summary

`NullPointerException` is a common logic error in Java code and should be exposed and fixed early;

Enhanced exception messages for Java 14 can be enabled to view detailed error messages for `NullPointerException`.