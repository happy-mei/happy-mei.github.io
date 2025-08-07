<!-- TRANSLATED by md-translate -->
# Polymorphic

In an inheritance relationship, a subclass that defines a method with the exact same method signature as the parent class is called an Override.

For example, in the `Person` class, we define the `run()` method:

```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

Override this `run()` method in the subclass `Student`:

```java
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

Override differs from Overload in that if the method signature is different, it is Overload and the Overload method is a new method; if the method signature is the same and the return value is the same, it is `Override`.

> [!WARNING]注意
>
> 方法名相同，方法参数相同，但方法返回值不同，也是不同的方法。在Java程序中，出现这种情况，编译器会报错。

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    // 不是Override，因为参数不同:
    public void run(String s) { … }
    // 不是Override，因为返回值不同:
    public int run() { … }
}
```

Adding `@Override` allows the compiler to help check that the correct override has been done. If you wish to override, but accidentally write the wrong method signature, the compiler will report an error.

```java
// override
public class Main {
    public static void main(String[] args) {
    }
}

class Person {
    public void run() {}
}

public class Student extends Person {
    @Override // Compile error!
    public void run(String s) {}
}
```

But `@Override` is not required.

In the previous section, we have learned that the declared type of a reference variable may not match its actual type, for example:

```java
Person p = new Student();
```

Now, let's consider a situation if a subclass overrides a method of the parent class:

```java
// override
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

So, a variable with actual type `Student` and reference type `Person`, calling its `run()` method, calls the `run()` method of `Person` or `Student`?

A quick run of the above code shows that the method actually called is the `run()` method of `Student`. Hence the conclusion can be drawn:

Java's instance method calls are dynamic calls based on the actual type at runtime, not the declared type of the variable.

This very important feature is called polymorphism in object-oriented programming. It is spelled very complicatedly in English: Polymorphic.

### Polymorphism

Polymorphism means that a method call for a certain type is made in such a way that the method it actually executes depends on the actual type of method at the runtime. Example:

```java
Person p = new Student();
p.run(); // 无法确定运行时究竟调用哪个run()方法
```

Some students will say, from the code above, it is clear that the `run()` method of `Student` is definitely called.

But suppose we write such a method:

```java
public void runTwice(Person p) {
    p.run();
    p.run();
}
```

It passes in a parameter of type `Person`, and there is no way of knowing whether the actual type of the passed-in parameter is `Person`, or `Student`, or some other subclass of `Person` such as `Teacher`, and therefore there is no way of knowing for sure whether or not it is the `run()` method defined by the `Person` class that is being invoked.

So, the property of polymorphism is that it is the runtime that dynamically determines the subclass method that is called. Calling a certain method on a certain type, the actual method executed may be an overridden method of a certain subclass. What exactly is the purpose of this indeterministic method invocation?

Let's stick to examples.

Suppose we define an income for which we need to file a tax return, then first define an `Income` class:

```java
class Income {
    protected double income;
    public double getTax() {
        return income * 0.1; // 税率10%
    }
}
```

For salary income, a base can be subtracted, then we can derive `SalaryIncome` from `Income` and override `getTax()`:

```java
class Salary extends Income {
    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
```

If you are entitled to the State Council Special Allowance, then according to the regulations, you can be fully exempted from tax:

```java
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```

Now, we are going to write a financial software for tax filing, for all the income of a person, which can be written like this:

```java
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

Come and try it:

```java
// Polymorphic
public class Main {
    public static void main(String[] args) {
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new StateCouncilSpecialAllowance(15000)
        };
        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {
        double total = 0;
        for (Income income: incomes) {
            total = total + income.getTax();
        }
        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}
```

Observing the `totalTax()` method: utilizing polymorphism, the `totalTax()` method only needs to deal with `Income`, which does not need to know about the existence of `Salary` and `StateCouncilSpecialAllowance` at all, in order to correctly compute the total tax. If we want to add a new type of manuscript income, we just need to derive it from `Income` and override the `getTax()` method correctly. Pass the new type into `totalTax()` without changing any code.

As you can see, polymorphism has a very powerful feature, which is to allow the addition of more types of subclasses to achieve functionality extensions, but do not need to modify the code based on the parent class.

### Overriding Object methods

Because all `classes` ultimately inherit from `Object`, which defines several important methods:

* `toString()`: output instance as `String`;
* `equals()`: determine if two instances are logically equivalent;
* `hashCode()`: computes the hash value of an instance.

We can override these methods of `Object` if necessary. Example:

```java
class Person {
    ...
    // 显示更有意义的字符串:
    @Override
    public String toString() {
        return "Person:name=" + name;
    }

    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }

    // 计算hash:
    @Override
    public int hashCode() {
        return this.name.hashCode();
    }
}
```

### Call super ###

In the overridden method of a subclass, if you want to call the overridden method of the parent class, you can call it via `super`. Example:

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

### final

Inheritance allows a subclass to override a parent class's methods. If a parent class does not allow a child class to override one of its methods, it can mark the method as `final`. Methods modified with `final` cannot be `Overridden`:

```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```

If a class does not want any other class to inherit from it, then the class itself can be marked as `final`. Classes modified with `final` cannot be inherited:

```java
final class Person {
    protected String name;
}

// compile error: 不允许继承自Person
class Student extends Person {
}
```

For instance fields of a class, they can also be modified with `final`. Fields modified with `final` cannot be modified after initialization. Example:

```java
class Person {
    public final String name = "Unamed";
}
```

Reassigning a value to a `final` field will report an error:

```java
Person p = new Person();
p.name = "New Name"; // compile error!
```

You can initialize final fields in the constructor method:

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

This approach is more commonly used because it ensures that once an instance is created, its `final` field cannot be modified.

### Exercise

Doing the tax math for a partner who has a paycheck and manuscript income.

[Download exercise](oop-polymorphic.zip)

### Summary

Subclasses can override the methods of the parent class (Override), overriding changes the behavior of the parent class method in the subclass;

Java's method calls always act on the actual type of the runtime object, a behavior known as polymorphism;

The `final` modifier serves multiple purposes:

* `final`-modified methods prevent overriding;
* A class modified by `final` prevents inheritance;
* `final`-modified fields must be initialized when the object is created and may not be modified thereafter.
