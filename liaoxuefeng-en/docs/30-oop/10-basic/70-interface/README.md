<!-- TRANSLATED by md-translate -->
# Interfaces

In abstract classes, abstract methods essentially define the interface specification: that is, they specify the interface of the higher-level class, thus ensuring that all subclasses have the same implementation of the interface, and in this way, polymorphism can be powerful.

If an abstract class has no fields, all methods are all abstract:

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

It is then possible to rewrite that abstract class as an interface: `interface`.

In Java, an interface can be declared using `interface`:

```java
interface Person {
    void run();
    String getName();
}
```

A so-called `interface` is a purely abstract interface that is even more abstract than an abstract class, because it can't even have fields. Since all methods defined by an interface are `public abstract` by default, these two modifiers don't need to be written (the effect is the same whether you write them or not).

The `implements` keyword is required when a concrete `class` goes to implement an `interface`. An example:

```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

We know that in Java, a class can only inherit from another class, not from more than one class. However, a class can implement multiple `interfaces`, for example:

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

### Terminology

Note the distinction between the terms:

Java's interface refers specifically to the definition of `interface`, which denotes an interface type and a set of method signatures, while programming interface refers generally to interface specifications such as method signatures, data formats, network protocols, and so on.

The comparison between abstract classes and interfaces is as follows:

| | abstract class | interface |
|--|:---------------|:----------|
| inheritance | can only extends a class | can implements multiple interfaces |
| fields | can define instance fields | cannot define instance fields |
| Abstract Methods | can define abstract methods | can define abstract methods |
| non-abstract methods | can define non-abstract methods | can define default methods |

### Interface inheritance

An `interface` can inherit from another `interface`. An `interface` inherits from an `interface` using `extends`, which is equivalent to extending the methods of the interface. Example:

```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

At this point, the `Person` interface inherits from the `Hello` interface, so the `Person` interface now actually has three abstract method signatures, one of which comes from the inherited `Hello` interface.

### Succession

The inheritance relationship between `interface` and `abstract class` can be rationally designed to fully reuse the code. Generally speaking, public logic is suitable for `abstract class`, concrete logic is put into subclasses, and the interface level represents the degree of abstraction. You can refer to Java's collection class definition of a set of interfaces, abstract classes, and concrete subclass inheritance relationships:

```ascii
┌───────────────┐
│   Iterable    │
└───────────────┘
        ▲                ┌───────────────────┐
        │                │      Object       │
┌───────────────┐        └───────────────────┘
│  Collection   │                  ▲
└───────────────┘                  │
        ▲     ▲          ┌───────────────────┐
        │     └──────────│AbstractCollection │
┌───────────────┐        └───────────────────┘
│     List      │                  ▲
└───────────────┘                  │
              ▲          ┌───────────────────┐
              └──────────│   AbstractList    │
                         └───────────────────┘
                                ▲     ▲
                                │     │
                                │     │
                     ┌────────────┐ ┌────────────┐
                     │ ArrayList  │ │ LinkedList │
                     └────────────┘ └────────────┘
```

When in use, the instantiated object can only ever be a specific subclass, but it is always referenced through an interface, because interfaces are more abstract than abstract classes:

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

### default method

In interfaces, `default` methods can be defined. For example, change the `run()` method of the `Person` interface to a `default` method:

```java
// interface
public class Main {
    public static void main(String[] args) {
        Person p = new Student("Xiao Ming");
        p.run();
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

Implementing classes may not have to override the `default` method. The purpose of the `default` method is that when we need to add a new method to the interface, it will involve modifying all the subclasses. If the `default` method is added, then the subclasses don't have to be modified in their entirety, and only need to override the new method wherever it needs to be overridden.

The `default` method is different from the ordinary method of an abstract class. Because `interface` has no fields, the `default` method has no access to fields, whereas the normal methods of an abstract class have access to instance fields.

### Exercise

Use the interface to calculate taxes for a small partner who has a paycheck and manuscript income.

[Download exercise](oop-interface.zip)

### Summary

Java's interface (interface) defines a purely abstract specification, a class can implement multiple interfaces;

Interfaces are also data types and are suitable for upward and downward transformations;

All methods of an interface are abstract methods, and interfaces cannot define instance fields;

Interfaces can define `default` methods (JDK>=1.8).