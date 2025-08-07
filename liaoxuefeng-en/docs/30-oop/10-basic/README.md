<!-- TRANSLATED by md-translate -->
# Object-oriented fundamentals

Object-oriented programming, a programming method that maps the real world to a computer model by means of objects.

In the real world, we define the abstract concept of "human being", and the concrete human being is a specific person such as "Xiaoming", "Xiaohong", "Xiaojun" and so on. "Xiao Ming", "Xiao Hong", "Xiao Jun" and other concrete people. Therefore, "person" can be defined as a class, and concrete people are instances:

| Real World | Computer Modeling | Java code |
|:--------|:----------|:----------------------------|
| People | Classes / class | class Person { } |
| Xiao Ming | Instances / ming | Person ming = new Person() |
| Instances / hong | Person hong = new Person() | | Person { }
| Instances / jun | Person jun = new Person() | | Xiao Hong | Instances / hong | Person hong = new Person() | | Xiao Jun

Similarly, the "book" is an abstraction, so it is the class, while "Java Core Technology", "Java Programming Ideas", "Java Study Notes" are examples:

| Real World | Computer Modeling | Java code |
|:------------|:------------|:-------------------------|
| Book | Classes / class | class Book { } |
| Java Core Technology | Examples / book1 | Book book1 = new Book() |
| Java Programming Ideas | Examples / book2 | Book book2 = new Book() |
| Java Learning Notes | Examples / book3 | Book book3 = new Book() |

### class and instance ###

So, once you understand the concepts of class and instance, you basically understand what object-oriented programming is.

A class is an object template that defines how instances are created, and as such, the class itself is a data type:

![class](class.jpg)

Whereas instance is an object instance, instance is an instance created according to class, multiple instances can be created, each of the same type, but the respective properties may not be the same:

![instances](instances.jpg)

### Define class

In Java, creating a class, for example, naming the class `Person`, is defining a `class`:

```java
class Person {
    public String name;
    public int age;
}
```

A `class` can contain multiple fields (`fields`) which are used to characterize a class. In the `Person` class above, we defined two fields, one of type `String` named `name` and one of type `int` named `age`. Thus, with `class`, data encapsulation is achieved by bringing together a set of data into a single object.

`public` is used to qualify a field, which indicates that the field can be accessed externally.

Let's look at another definition of the `Book` class:

```java
class Book {
    public String name;
    public String author;
    public String isbn;
    public double price;
}
```

Indicate the individual fields of the `Book` class.

### Creating instances

Defining a class only defines the object template, and to create a real object instance based on the object template, you must use the new operator.

The new operator creates an instance, and then we need to define a variable of reference type to point to this instance:

```java
Person ming = new Person();
```

The above code creates an instance of type Person and points to it via the variable `ming`.

Note the distinction that `Person ming` defines the variable `ming` of type `Person`, while `new Person()` creates an instance of `Person`.

With a variable pointing to this instance, we can manipulate the instance through this variable. Accessing instance variables can be done with `variable. Field`, for example:

```java
ming.name = "Xiao Ming"; // 对字段name赋值
ming.age = 12; // 对字段age赋值
System.out.println(ming.name); // 访问字段name

Person hong = new Person();
hong.name = "Xiao Hong";
hong.age = 15;
```

The above two variables point to two different instances and their structure in memory is as follows:

```ascii
┌──────────────────┐
ming ──────▶│Person instance   │
            ├──────────────────┤
            │name = "Xiao Ming"│
            │age = 12          │
            └──────────────────┘
            ┌──────────────────┐
hong ──────▶│Person instance   │
            ├──────────────────┤
            │name = "Xiao Hong"│
            │age = 15          │
            └──────────────────┘
```

The two `instances` have the `name` and `age` fields defined by the `class`, and each has a separate copy of the data that does not interfere with each other.

> [!NOTICE]注意
>
> 一个Java源文件可以包含多个类的定义，但只能定义一个public类，且public类名必须与文件名一致。如果要定义多个public类，必须拆到多个Java源文件中。

### Exercise

Please define a City class that has the following fields.

* name: name, type String
* latitude: latitude, type double
* longitude: longitude, type double

Instantiate several Cities and assign values, then print.

```java
// City
public class Main {
    public static void main(String[] args) {
        City bj = new City();
        bj.name = "Beijing";
        bj.latitude = 39.903;
        bj.longitude = 116.401;
        System.out.println(bj.name);
        System.out.println("location: " + bj.latitude + ", " + bj.longitude);
    }
}

class City {
    ???
}
```

### Summary

In OOP, `class` and `instance` are the relationship between `template` and `instance`;

To define `class` is to define a data type, and the corresponding `instance` is an instance of that data type;

A `field` defined by a `class` will have its own `field` in each `instance` and will not interfere with each other;

Create a new `instance` with the `new` operator and point to it with a variable to refer to the `instance` through the variable;

Access to instance fields is `variable name. Field name`;

Variables that point to `instance` are reference variables.