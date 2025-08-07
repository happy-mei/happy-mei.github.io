<!-- TRANSLATED by md-translate -->
# Constructor

When creating an instance, we often need to initialize the fields of this instance at the same time, for example:

```java
Person ming = new Person();
ming.setName("小明");
ming.setAge(12);
```

It takes 3 lines of code to initialize an object instance, and, if you forget to call `setName()` or `setAge()`, the state inside this instance is incorrect.

Is it possible to initialize all internal fields to the appropriate values as soon as the object instance is created?

Totally.

This is where we need constructor methods.

When creating an instance, the instance is actually initialized by a constructor method. We'll start by defining a constructor method that can be initialized by passing in `name` and `age` at once when creating an instance of `Person`:

```java
// 构造方法
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

Since constructor methods are so special, the name of the constructor method is the class name. There is no restriction on the parameters of a constructor method, and inside the method, arbitrary statements can be written. However, in contrast to normal methods, constructor methods have no return value (and no `void`), and to call a constructor method, you must use the `new` operator.

### Default constructor

Does any `class` have a constructor method? Yes.

So why can we call `new Person()` when we didn't write a constructor for the `Person` class earlier?

The reason for this is that if a class doesn't define a constructor method, the compiler automatically generates a default constructor method for us, which has no parameters and no execution statement, something like this:

```java
class Person {
    public Person() {
    }
}
```

In particular, note that if we customize a constructor method, then the compiler _no longer_ automatically creates the default constructor method:

```java
// 构造方法
public class Main {
    public static void main(String[] args) {
        Person p = new Person(); // 编译错误:找不到这个构造方法
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

If you want to be able to use the constructor with parameters but also want to keep the constructor without parameters, then you can only define both constructors:

```java
// 构造方法
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Xiao Ming", 15); // 既可以调用带参数的构造方法
        Person p2 = new Person(); // 也可以调用无参数构造方法
    }
}

class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

When fields are not initialized in the constructor method, fields of reference types default to `null`, fields of numeric types use default values, `int` types default to `0`, and boolean types default to `false`:

```java
class Person {
    private String name; // 默认初始化为null
    private int age; // 默认初始化为0

    public Person() {
    }
}
```

Fields can also be initialized directly:

```java
class Person {
    private String name = "Unamed";
    private int age = 10;
}
```

So here's the problem: both initializing the field and initializing the field in the constructor method:

```java
class Person {
    private String name = "Unamed";
    private int age = 10;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

When we create an object, `new Person("Xiao Ming", 12)` gets an instance of the object, what are the initial values of the fields?

In Java, object instances are initialized in the following order when they are created:

1. Initialize the fields first, e.g., `int age = 10;` means that the field is initialized to `10`, `double salary;` means that the field is initialized to `0` by default, and `String s;` means that the field of reference type is initialized to `null` by default;
2. Execute the code of the constructor method for initialization.

Therefore, the code of the constructor method is run later, so the field values of `new Person("Xiao Ming", 12)` are ultimately determined by the code of the constructor method.

### Multiple constructors

Multiple constructor methods can be defined, and when invoked via the `new` operator, the compiler automatically distinguishes them by the number, position, and type of the constructor's arguments:

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this.name = name;
        this.age = 12;
    }

    public Person() {
    }
}
```

If you call `new Person("Xiao Ming", 20);`, it will automatically match to the constructor `public Person(String, int)`.

If you call `new Person("Xiao Ming");`, it will automatically match to the constructor `public Person(String)`.

If `new Person();` is called, it will automatically match to the constructor `public Person()`.

A constructor method can call other constructor methods; this is done to facilitate code reuse. The syntax for calling other constructor methods is `this(...)`:

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }

    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```

### Exercise

Please add `(String, int)` constructor to `Person` class:

```java
public class Main {
    public static void main(String[] args) {
        // TODO: 给Person增加构造方法:
        Person ming = new Person("小明", 12);
        System.out.println(ming.getName());
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

[Download exercise](oop-constructor.zip)

### Summary

Instances are created with the `new` operator by calling their corresponding constructor methods, which are used to initialize the instance;

When no constructor is defined, the compiler automatically creates a default parameterless constructor;

Multiple constructor methods can be defined, and the compiler automatically determines them based on the parameters;

It is possible to call another constructor method inside a constructor method, which facilitates code reuse.