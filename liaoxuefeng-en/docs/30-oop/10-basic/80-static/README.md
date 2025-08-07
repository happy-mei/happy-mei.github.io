<!-- TRANSLATED by md-translate -->
# Static fields and static methods

Fields defined in a `class` we call instance fields. Instance fields are characterized by the fact that each instance has a separate field, and the same field in each instance does not affect each other.

There is another type of field, one that is modified with `static`, called a static field: `static field`.

Instance fields have a separate "space" in each instance, but static fields have a shared "space" that is shared by all instances. As an example:

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}
```

Let's take a look at the code below:

```java
// static field
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);
        hong.number = 99;
        System.out.println(ming.number);
    }
}

class Person {
    public String name;
    public int age;

    public static int number;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

For static fields, the effect is the same no matter which instance's static field is modified: all instances' static fields are modified, due to the fact that static fields do not belong to instances:

```ascii
┌──────────────────┐
ming ──▶│Person instance   │
        ├──────────────────┤
        │name = "Xiao Ming"│
        │age = 12          │
        │number ───────────┼──┐    ┌─────────────┐
        └──────────────────┘  │    │Person class │
                              │    ├─────────────┤
                              ├───▶│number = 99  │
        ┌──────────────────┐  │    └─────────────┘
hong ──▶│Person instance   │  │
        ├──────────────────┤  │
        │name = "Xiao Hong"│  │
        │age = 15          │  │
        │number ───────────┼──┘
        └──────────────────┘
```

Although instances can access static fields, they all point to static fields of `Person class` actually. So, all instances share a static field.

Therefore, it is not recommended to use `Instance Variables. Static Fields` to access static fields, because in Java programs, instance objects do not have static fields. In code, instance objects can access static fields only because the compiler can automatically convert them based on the instance type to `Class name. Static Fields` to access static objects.

It is recommended to access static fields by class name. Static fields can be understood as fields that describe the `class` itself. For the above code, a better way to write it would be:

```java
Person.number = 99;
System.out.println(Person.number);
```

### Static methods

There are static fields, and there are static methods. Methods modified with `static` are called static methods.

An instance method must be called through an instance variable, whereas a static method does not require an instance variable and can be called through the class name. Static methods are similar to functions in other programming languages. For example:

```java
// static method
public class Main {
    public static void main(String[] args) {
        Person.setNumber(99);
        System.out.println(Person.number);
    }
}

class Person {
    public static int number;

    public static void setNumber(int value) {
        number = value;
    }
}
```

Because static methods belong to a `class` and not to an instance, inside a static method, there is no access to the `this` variable or to instance fields; it can only access static fields.

Static methods can also be called through instance variables, but this is just the compiler automatically rewriting the instance to the class name for us.

Typically, accessing static fields and static methods through instance variables will result in a compilation warning.

Static methods are often used in tool classes. For example:

* Arrays.sort()
* Math.random()

Static methods are also often used for helper methods. Notice that the entry point to a Java program, `main()`, is also a static method.

### Static fields for interfaces

Because `interface` is a purely abstract class, it cannot define instance fields. However, `interface` is allowed to have static fields, and the static fields must be of type `final`:

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

In fact, since fields of `interface` can only be of type `public static final`, we can remove all these modifiers and the above code can be abbreviated as:

```java
public interface Person {
    // 编译器会自动加上public static final:
    int MALE = 1;
    int FEMALE = 2;
}
```

The compiler automatically changes the field to the `public static final` type.

### Exercise

Add a static field `count` and static method `getCount()` to the `Person` class to count the number of instances created.

[Download Exercise](oop-static.zip)

### Summary

Static fields are fields that are `shared' by all instances, in fact they are `class` fields;

Calling a static method does not require an instance, and you cannot access `this`, but you can access static fields and other static methods;

Static methods are commonly used in tool classes and helper methods.