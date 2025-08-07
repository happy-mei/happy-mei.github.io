<!-- TRANSLATED by md-translate -->
# Abstract classes

Due to polymorphism, every subclass can override the methods of the parent class, for example:

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    @Override
    public void run() { … }
}

class Teacher extends Person {
    @Override
    public void run() { … }
}
```

Both `Student` and `Teacher`, derived from the `Person` class, can override the `run()` method.

If the `run()` method of the parent class `Person` has no real meaning, is it possible to remove the execution statement of the method?

```java
class Person {
    public void run(); // Compile Error!
}
```

The answer is no, it will result in a compilation error because when defining a method, you must implement the method's statements.

Is it possible to remove the `run()` method of the parent class?

The answer is still no, because by removing the `run()` method of the parent class, the polymorphic nature is lost. For example, `runTwice()` would not compile:

```java
public void runTwice(Person p) {
    p.run(); // Person没有run()方法，会导致编译错误
    p.run();
}
```

A method of a parent class can be declared as an abstract method if the method itself does not need to implement any functionality, but is merely intended to define a method signature with the purpose of allowing a subclass to override it:

```java
class Person {
    public abstract void run();
}
```

Declaring a method as `abstract` indicates that it is an abstract method that does not itself implement any method statements. Because this abstract method is not executable by itself, the `Person` class cannot be instantiated either. The compiler will tell us that the `Person` class cannot be compiled because it contains abstract methods.

The `Person` class itself must also be declared as `abstract` in order for it to compile correctly:

```java
abstract class Person {
    public abstract void run();
}
```

### Abstract class

If a `class` defines a method without concrete execution code, the method is an abstract method, and abstract methods are modified with `abstract`.

Because it is not possible to execute abstract methods, this class must also be declared as an abstract class.

A class modified with `abstract` is an abstract class. We cannot instantiate an abstract class:

```java
Person p = new Person(); // 编译错误
```

What is the use of an abstract class that cannot be instantiated?

Because abstract classes themselves are designed to be inherited only, they can force subclasses to implement the abstract methods they define, or else compile errors will be reported. Thus, abstract methods are effectively the equivalent of defining a "specification".

For example, the `Person` class defines the abstract method `run()`, then the `run()` method must be overridden when implementing the subclass `Student`:

```java
// abstract class
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

### Abstraction-oriented programming

When we define the abstract class `Person`, and the concrete subclasses `Student` and `Teacher`, we can refer to instances of the concrete subclasses through the abstract class `Person` type:

```java
Person s = new Student();
Person t = new Teacher();
```

The beauty of this referential abstract class is that we make method calls to it without caring about the specific subtypes of the `Person` type variable:

```java
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

The same code, if the reference is to a new subclass, we still don't care about the specific type:

```java
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```

This way of referencing high-level types as much as possible and avoiding references to actual subtypes is called abstract-oriented programming.

The essence of abstraction-oriented programming is:

* Upper-level code defines only the specification (e.g., `abstract class Person`);
* Business logic can be implemented without subclassing (normal compilation);
* The specific business logic is implemented by different subclasses and is of no concern to the caller.

### Exercise

Use an abstract class to calculate taxes for a partner who has a paycheck and a draft income.

[Download exercise](oop-abstractclass.zip)

### Summary

Methods defined through `abstract` are abstract methods, which have only a definition, not an implementation. Abstract methods define the interface specifications that subclasses must implement;

A class that defines an abstract method must be defined as an abstract class, and subclasses inheriting from an abstract class must implement the abstract method;

If no abstract methods are implemented, the subclass remains an abstract class;

Abstraction-oriented programming makes the caller only care about the definition of the abstract method and not about the concrete implementation of the subclass.