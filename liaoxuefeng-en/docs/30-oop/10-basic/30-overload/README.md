<!-- TRANSLATED by md-translate -->
# Method overloading

In a class, we can define more than one method. If there is a set of methods, which are all similar in function and differ only in their parameters, then the set of method names can be made into _same_methods. For example, in the `Hello` class, define multiple `hello()` methods:

```java
class Hello {
    public void hello() {
        System.out.println("Hello, world!");
    }

    public void hello(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public void hello(String name, int age) {
        if (age < 18) {
            System.out.println("Hi, " + name + "!");
        } else {
            System.out.println("Hello, " + name + "!");
        }
    }
}
```

Such methods with the same name, but with different arguments for each, are called method overloads (`Overload`).

Note: The return value types of method overloads are usually the same.

The purpose of method overloading is that methods with similar functionality use the same name and are easier to remember and, therefore, simpler to call.

As an example, the `String` class provides several overloaded methods, `indexOf()`, to find substrings:

* `int indexOf(int ch)`: lookup based on the Unicode code of the character;
* `int indexOf(String str)`: lookup based on string;
* `int indexOf(int ch, int fromIndex)`: lookup based on character, but specify start position;
* `int indexOf(String str, int fromIndex)`: lookup by string, but specify start position.

Try it:

```java
// String.indexOf()
public class Main {
    public static void main(String[] args) {
        String s = "Test string";
        int n1 = s.indexOf('t');
        int n2 = s.indexOf("st");
        int n3 = s.indexOf("st", 4);
        System.out.println(n1);
        System.out.println(n2);
        System.out.println(n3);
    }
}
```

### Exercise

Add overloaded method `setName(String, String)` to `Person`:

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        Person hong = new Person();
        ming.setName("Xiao Ming");
        // TODO: 给Person增加重载方法setName(String, String):
        hong.setName("Xiao", "Hong");
        System.out.println(ming.getName());
        System.out.println(hong.getName());
    }
}

class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

[Download Exercise](oop-overload.zip)

### Summary

Method overloading is when multiple methods have the same method name, but each has different parameters;

Overloaded methods should accomplish similar functionality, cf. `indexOf()` for `String`;

Overloaded methods should have the same return value type.