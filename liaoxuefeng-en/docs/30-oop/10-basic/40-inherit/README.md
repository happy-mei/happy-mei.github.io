<!-- TRANSLATED by md-translate -->
# Inheritance

In the previous sections, we have defined the `Person` class:

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}
```

Now, suppose you need to define a `Student` class with the following fields:

```java
class Student {
    private String name;
    private int age;
    private int score;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
    public int getScore() { … }
    public void setScore(int score) { … }
}
```

A closer look reveals that the `Student` class contains the fields and methods already present in the `Person` class, except for an extra `score` field and the corresponding `getScore()` and `setScore()` methods.

Can you not write duplicate code in `Student`?

This is when inheritance comes in handy.

Inheritance is a very powerful mechanism in object-oriented programming that allows for reusing code in the first place. When we let `Student` inherit from `Person`, `Student` gets all the functionality of `Person`, and we only need to write the additional functionality for `Student`.

Java uses the `extends` keyword to implement inheritance:

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

As you can see, with inheritance, `Student` only needs to write additional functionality and no longer needs to duplicate code.

```alert type=warning title=注意
子类自动获得了父类的所有字段，严禁定义与父类重名的字段！
```

In OOP terminology, we refer to `Person` as super class, parent class, base class and `Student` as subclass, extended class.

### Succession tree ###

Notice that we didn't write `extends` when we defined `Person`. In Java, classes that don't explicitly write `extends`, the compiler automatically adds `extends Object`. So, any class, except `Object`, inherits from some class. The following figure shows the inheritance tree for `Person`, `Student`:

```ascii
┌───────────┐
│  Object   │
└───────────┘
      ▲
      │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```

Java only allows a class to inherit from a class, so a class has one and only one parent. Only `Object` is special in that it has no parent class.

Similarly, if we define a `Teacher` that inherits from `Person`, their inheritance tree relationship is as follows:

```ascii
┌───────────┐
       │  Object   │
       └───────────┘
             ▲
             │
       ┌───────────┐
       │  Person   │
       └───────────┘
          ▲     ▲
          │     │
          │     │
┌───────────┐ ┌───────────┐
│  Student  │ │  Teacher  │
└───────────┘ └───────────┘
```

### protected

One feature of inheritance is that child classes cannot access `private` fields or `private` methods of the parent class. For example, the `Student` class cannot access the `name` and `age` fields of the `Person` class:

```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```

This makes inheritance less useful. In order for subclasses to have access to fields of the parent class, we need to change `private` to `protected`. Fields modified with `protected` can be accessed by subclasses:

```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```

Thus, the `protected` keyword keeps access to fields and methods inside the inheritance tree, and a `protected` field and method can be accessed by its subclasses, as well as by the subclasses of its subclasses, as we'll explain in more detail later.

### super

The `super` keyword indicates a parent class (superclass). A subclass can use `super.fieldName` when referencing a field of the parent class. Example:

```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```

In fact, using `super.name` here, or `this.name`, or `name`, has the same effect. The compiler automatically locates the `name` field of the parent class.

However, at some point, it becomes necessary to use `super`. Let's look at an example:

```java
// super
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 12, 89);
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        this.score = score;
    }
}
```

Running the above code gets you a compile error to the effect that the constructor method of `Person` cannot be called from within the constructor method of `Student`.

This is because in Java, the first line of any `class` constructor method must be a call to the constructor method of the parent class. If the constructor method of the parent class is not explicitly called, the compiler will automatically add a `super();` line for us, so the constructor method of the `Student` class actually looks like this:

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(); // 自动调用父类的构造方法
        this.score = score;
    }
}
```

However, the `Person` class does not have a parameterless constructor method, and therefore, compilation fails.

The solution is to call one of the constructor methods that exist for the `Person` class. Example:

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

This will compile properly!

We therefore conclude that if the parent class does not have a default constructor method, the child class must explicitly call `super()` with arguments in order for the compiler to locate an appropriate constructor method for the parent class.

There is another issue that arises here in passing: namely, subclasses _will not inherit_ any of the parent class's constructor methods. The default constructor methods for subclasses are automatically generated by the compiler, not inherited.

### Block inheritance ###

Normally, any class can inherit from a class as long as that class does not have the `final` modifier.

Starting with Java 15, it is permissible to use `sealed` to modify a class and explicitly write the names of subclasses that can inherit from that class via `permits`.

For example, define a `Shape` class:

```java
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

The above `Shape` class is a `sealed` class that allows only the specified 3 classes to inherit from it. If written:

```java
public final class Rect extends Shape {...}
```

is fine, because the `Rect` appears in the `Shape`'s `permits` list. However, if you define an `Ellipse` you get an error:

```java
public final class Ellipse extends Shape {...}
// Compile error: class is not allowed to extend sealed class: Shape
```

The reason for this is that `Ellipse` does not appear in the `Shape` list of `permits`. This `sealed` class is mainly used in some frameworks to prevent inheritance abuse.

The `sealed` class is currently previewed in Java 15; to enable it, you must use the arguments `--enable-preview` and `--source 15`.

### Transitioning upward

If a reference variable is of type `Student`, then it can point to an instance of type `Student`:

```java
Student s = new Student();
```

If a variable of reference type is `Person`, then it can point to an instance of type `Person`:

```java
Person p = new Person();
```

Now the question arises: if `Student` is inherited from `Person`, can a variable with reference type `Person` point to an instance of type `Student`?

```java
Person p = new Student(); // ???
```

A test reveals that such pointing is allowed!

This is because `Student` inherits from `Person` and therefore has all the functionality of `Person`. A variable of type `Person` that points to an instance of type `Student` has no problem operating on it!

This kind of assignment, which safely changes a subclass type into a parent type, is called upcasting.

Upward transformation is actually the safe changing of a subtype into a more abstract parent type:

```java
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

Notice that the inheritance tree is `Student > Person > Object`, so it is possible to transform the type `Student` to `Person`, or higher level `Object`.

### Transitioning downward

Contrary to upward transformation, if you force a parent type to transform into a child type, it is downcasting. Example:

```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```

If you test the code above, you can find out:

The `Person` type `p1` actually points to the `Student` instance, and the `Person` type variable `p2` actually points to the `Person` instance. When transforming down, transforming `p1` to `Student` will succeed because `p1` actually points to a `Student` instance, and transforming `p2` to `Student` will fail because `p2` is actually of type `Person`, and you can't change the parent class into a subclass because the subclass has more functionality than the parent, and more functionality can't be changed out of thin air.

Therefore, the downward transition is likely to fail. When it fails, the Java Virtual Machine reports a `ClassCastException`.

To avoid downward transformation errors, Java provides the `instanceof` operator, which allows you to first determine whether an instance is of a certain type or not:

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

`instanceof` actually determines whether the instance pointed to by a variable is of the specified type, or a subclass of that type. If a reference variable is `null`, then any `instanceof` judgment is `false`.

Using `instanceof`, you can judge before transforming downwards:

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

Starting with Java 14, after judging `instanceof`, you can transform directly to the specified variable, avoiding the need to force another transformation. For example, for the following code:

```java
Object obj = "hello";
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}
```

It can be rewritten as follows:

```java
// instanceof variable:
public class Main {
    public static void main(String[] args) {
        Object obj = "hello";
        if (obj instanceof String s) {
            // 可以直接使用变量s:
            System.out.println(s.toUpperCase());
        }
    }
}
```

This writeup using `instanceof` is more concise.

### Distinguish between inheritance and combination

When using inheritance, we have to be careful about logical consistency.

Examine the `Book` class below:

```java
class Book {
    protected String name;
    public String getName() {...}
    public void setName(String name) {...}
}
```

This `Book` class also has a `name` field, so can we make `Student` inherit from `Book`?

```java
class Student extends Book {
    protected int score;
}
```

Obviously, logically, this doesn't make sense; `Student` should not inherit from `Book`, but from `Person`.

The reason for this is that `Student` is a type of `Person` and they are is-relationships, whereas `Student` is not `Book`. In fact the relationship between `Student` and `Book` is a has relationship.

Having a has relationship should not use inheritance, but rather a combination, i.e. `Student` can hold a `Book` instance:

```java
class Student extends Person {
    protected Book book;
    protected int score;
}
```

Thus, inheritance is an IS relationship and combination is a HAS relationship.

### Exercise

Define `PrimaryStudent`, inheriting from `Student`, and add a new `grade` field:

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("小明", 12);
        Student s = new Student("小红", 20, 99);
        // TODO: 定义PrimaryStudent，从Student继承，新增grade字段:
        Student ps = new PrimaryStudent("小军", 9, 100, 5);
        System.out.println(ps.getScore());
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age);
        this.score = score;
    }

    public int getScore() { return score; }
}

class PrimaryStudent {
    // TODO
}
```

[Download exercise](oop-inherit.zip)

### Summary

Inheritance is a powerful way to reuse code in object-oriented programming;

Java allows only single inheritance, and the ultimate root class of all classes is `Object`;

`protected` allows subclasses to access fields and methods of the parent class;

Constructor methods of subclasses can be called from the parent class via `super()`;

can be safely up-converted to a more abstract type;

Downward transformations can be forced, preferably with the help of `instanceof` judgment;

The relationship between a subclass and its parent is an is, has relationship cannot be used with inheritance.