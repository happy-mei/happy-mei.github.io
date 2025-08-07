<!-- TRANSLATED by md-translate -->
# Record classes

When using types such as `String`, `Integer`, etc., which are invariant classes, an invariant class has the following characteristics:

1. Use `final` when defining class to make it impossible to derive subclasses;
2. use `final` for each field to ensure that no field can be modified after the instance is created.

Suppose we wish to define a `Point` class with `x` and `y` variables and it is also an invariant class, it can be written like this:

```java
public final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() {
        return this.x;
    }

    public int y() {
        return this.y;
    }
}
```

In order to ensure comparison of invariant classes, it is also necessary to override the `equals()` and `hashCode()` methods correctly so that they can be used properly in collection classes. We'll explain the correct overriding of `equals()` and `hashCode()` in more detail later, and the purpose of demonstrating the writing of the `Point` invariant class here is that all of this code is very simple to write, but tedious.

### record

Starting with Java 14, a new `Record` class was introduced. We define the `Record` class using the keyword `record`. Rewrite the above `Point` class as `Record` class with the following code:

```java
// Record
public class Main {
    public static void main(String[] args) {
        Point p = new Point(123, 456);
        System.out.println(p.x());
        System.out.println(p.y());
        System.out.println(p);
    }
}

record Point(int x, int y) {}
```

Look closely at the definition of `Point`:

```java
record Point(int x, int y) {}
```

Rewriting the above definition as a class is equivalent to the following code:

```java
final class Point extends Record {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() {
        return this.x;
    }

    public int y() {
        return this.y;
    }

    public String toString() {
        return String.format("Point[x=%s, y=%s]", x, y);
    }

    public boolean equals(Object o) {
        ...
    }
    public int hashCode() {
        ...
    }
}
```

In addition to modifying the class with `final` and each field, the compiler automatically creates constructor methods for us, methods with the same name as the field name, and overrides the `toString()`, `equals()`, and `hashCode()` methods.

In other words, using the `record` keyword, an invariant class can be written in one line.

Similar to `enum`, we ourselves cannot derive directly from `Record`, but can only implement inheritance by the compiler via the `record` keyword.

### Constructor

By default, the compiler automatically creates a constructor method in the order of the variables declared by `record` and assigns values to the fields within the method. So the question arises, what should we do if we want to check the parameters?

Assuming that the `x` and `y` of the `Point` class don't allow negative numbers, we'll have to add checking logic to the `Point` constructor:

```java
public record Point(int x, int y) {
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

Notice that the method `public Point {...} ` is called Compact Constructor and its purpose is to allow us to write the checking logic, the compiler ends up generating the constructor method as follows:

```java
public final class Point extends Record {
    public Point(int x, int y) {
        // 这是我们编写的Compact Constructor:
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
        // 这是编译器继续生成的赋值代码:
        this.x = x;
        this.y = y;
    }
    ...
}
```

Static methods can still be added to `Point` as a `record`. One common static method is the `of()` method, which is used to create a `Point`:

```java
public record Point(int x, int y) {
    public static Point of() {
        return new Point(0, 0);
    }
    public static Point of(int x, int y) {
        return new Point(x, y);
    }
}
```

This way we can write cleaner code:

```java
var z = Point.of();
var p = Point.of(123, 456);
```

### Summary

Starting with Java 14, the new `record` keyword is provided, which makes it very easy to define Data Classes:

* Invariant classes are defined using `record`;
* Compact Constructors can be written to validate parameters;
* Static methods can be defined.
