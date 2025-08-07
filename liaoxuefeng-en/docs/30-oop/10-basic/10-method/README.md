<!-- TRANSLATED by md-translate -->
# Methods

A `class` can contain more than one `field`, for example, we define two `fields` for the `Person` class:

```java
class Person {
    public String name;
    public int age;
}
```

However, exposing `field` directly to the outside with `public` may break encapsulation. For example, the code could be written like this:

```java
Person ming = new Person();
ming.name = "Xiao Ming";
ming.age = -99; // age设置为负数
```

Obviously, direct manipulation of `field` is likely to cause logical confusion. To prevent external code from accessing `field` directly, we can modify `field` with `private` to deny external access:

```java
class Person {
    private String name;
    private int age;
}
```

Try what effect `field` with the `private` modifier has:

```java
// private field
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.name = "Xiao Ming"; // 对字段name赋值
        ming.age = 12; // 对字段age赋值
    }
}

class Person {
    private String name;
    private int age;
}
```

Is it a compilation error? Remove the assignment statement that accesses `field` and it will compile fine.

![compile-error](compile-error.png)

Changing `field` from `public` to `private` prevents external code from accessing these `fields`, so what is the point of defining these `fields`? How can we assign a value to it? How can we read its value?

So we need to use methods (`method`) to allow external code to modify `field` indirectly:

```java
// private field
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("Xiao Ming"); // 设置name
        ming.setAge(12); // 设置age
        System.out.println(ming.getName() + ", " + ming.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 100) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
}
```

Although external code cannot modify the `private` field directly, external code can call the methods `setName()` and `setAge()` to modify the `private` field indirectly. Inside the methods, we then have the opportunity to check that the parameters are correct. For example, `setAge()` checks the incoming parameters, which are out of range and directly reports an error. This way, the external code doesn't have any chance to set `age` to an unreasonable value.

The same checks can be made for the `setName()` method, e.g., disallowing `null` and empty strings from being passed in:

```java
public void setName(String name) {
    if (name == null || name.isBlank()) {
        throw new IllegalArgumentException("invalid name");
    }
    this.name = name.strip(); // 去掉首尾空格
}
```

Similarly, external code cannot read the `private` field directly, but it can indirectly get the value of the `private` field through `getName()` and `getAge()`.

So, by defining methods, a class exposes an interface to external code for some operations, while, at the same time, internally ensuring logical consistency itself.

The syntax for calling a method is `Instance variable. Method name (parameters);`. A method call is a statement, so don't forget to add `;` to the end. For example: `ming.setName("Xiao Ming");`.

### Define the method

As you can see from the code above, the syntax for defining a method is:

```java
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

Method return values are implemented via the `return` statement; `return` can be omitted if there is no return value and the return type is set to `void`.

### Private methods

There are `public` methods, and naturally there are `private` methods. Like `private` fields, `private` methods do not allow external calls, so what is the point of defining `private` methods?

The rationale for defining `private` methods is that it is possible for internal methods to call `private` methods. Example:

```java
// private method
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setBirth(2008);
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;
    private int birth;

    public void setBirth(int birth) {
        this.birth = birth;
    }

    public int getAge() {
        return calcAge(2019); // 调用private方法
    }

    // private方法:
    private int calcAge(int currentYear) {
        return currentYear - this.birth;
    }
}
```

Observing the above code, `calcAge()` is a `private` method which cannot be called by external code, however, the internal method `getAge()` can call it.

In addition, we note that this `Person` class only defines the `birth` field, not the `age` field, and that when getting the `age`, the method `getAge()` returns a value calculated in real time, not a value stored in a field. This suggests that methods can encapsulate the external interface of a class, and the caller does not need to know or care whether the `Person` instance has an `age` field internally or not.

### this variable

Inside the method, an implicit variable `this` can be used, which always points to the current instance. Thus, the fields of the current instance can be accessed through `this.field`.

If there are no naming conflicts, `this` can be omitted. Example:

```java
class Person {
    private String name;

    public String getName() {
        return name; // 相当于this.name
    }
}
```

However, if there are local variables and fields that are renamed, then the local variables have higher priority and `this` must be added:

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name; // 前面的this不可少，少了就变成局部变量name了
    }
}
```

### Method parameters

Methods can contain 0 or any number of parameters. Method parameters are used to receive the values of variables passed to the method. When calling a method, the parameters must be passed one by one in strict accordance with their definitions. Example:

```java
class Person {
    ...
    public void setNameAndAge(String name, int age) {
        ...
    }
}
```

This `setNameAndAge()` method must be called with two parameters, and the first parameter must be a `String` and the second parameter must be an `int`:

```java
Person ming = new Person();
ming.setNameAndAge("Xiao Ming"); // 编译错误：参数个数不对
ming.setNameAndAge(12, "Xiao Ming"); // 编译错误：参数类型不对
```

### Variable parameters

Variable parameters are defined with `Type ... ` is defined, and variable parameters are equivalent to array types:

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

The `setNames()` above defines a variable parameter. When called, it can be written like this:

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

It is perfectly possible to rewrite variable parameters as `String[]` types:

```java
class Group {
    private String[] names;

    public void setNames(String[] names) {
        this.names = names;
    }
}
```

However, the caller needs to construct `String[]` by itself first, which is more cumbersome. Example:

```java
Group g = new Group();
g.setNames(new String[] {"Xiao Ming", "Xiao Hong", "Xiao Jun"}); // 传入1个String[]
```

Another issue is that the caller can pass in `null`:

```java
Group g = new Group();
g.setNames(null);
```

Variable parameters, on the other hand, guarantee that `null` cannot be passed in, since the actual value received when passing in 0 parameters is an empty array instead of `null`.

### Parameter binding

When the caller passes parameters to an instance method, the values passed at the time of the call are bound one by one by the position of the parameters.

So what is parameter binding?

We start by observing the passing of a basic type parameter:

```java
// 基本类型参数绑定
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15; // n的值为15
        p.setAge(n); // 传入n的值
        System.out.println(p.getAge()); // 15
        n = 20; // n的值改为20
        System.out.println(p.getAge()); // 15还是20?
    }
}

class Person {
    private int age;

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Running the code, it is clear from the results that modifying the external local variable `n` does not affect the `age` field of the instance `p`, because the parameter obtained by the `setAge()` method, replicates the value of `n`, and therefore, `p.age` and the local variable `n` do not affect each other.

Conclusion: The passing of a basic type parameter is a copy of the caller's value. Subsequent modifications by each side do not affect each other.

Let's look at another example of passing a reference parameter:

```java
// 引用类型参数绑定
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String[] fullname = new String[] { "Homer", "Simpson" };
        p.setName(fullname); // 传入fullname数组
        System.out.println(p.getName()); // "Homer Simpson"
        fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
        System.out.println(p.getName()); // "Homer Simpson"还是"Bart Simpson"?
    }
}

class Person {
    private String[] name;

    public String getName() {
        return this.name[0] + " " + this.name[1];
    }

    public void setName(String[] name) {
        this.name = name;
    }
}
```

Notice that the argument to `setName()` is now an array. At first, passing the `fullname` array in, and then, modifying the contents of the `fullname` array, it turns out that the field `p.name` of the instance `p` has also been modified!

> [!NOTICE]结论
>
> 引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象嘛）。

With the above conclusion in mind, let's look at another example:

```java
// 引用类型参数绑定
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String bob = "Bob";
        p.setName(bob); // 传入bob变量
        System.out.println(p.getName()); // "Bob"
        bob = "Alice"; // bob改名为Alice
        System.out.println(p.getName()); // "Bob"还是"Alice"?
    }
}

class Person {
    private String name;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Don't doubt the mechanism of quoted parameter binding, try to explain why the above code outputs `"Bob"` twice.

### Exercise

Add `getAge`/`setAge` methods to the `Person` class:

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("小明");
        ming.setAge(12);
        System.out.println(ming.getAge());
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

[Download exercise](oop-method.zip)

### Summary

* Methods allow external code to securely access instance fields;
* methods are a set of executable statements and can execute arbitrary logic;
* methods internally return when they encounter a return, void means that they do not return any value (note that this is different from returning null);
* External code manipulates instances through public methods, and internal code can call private methods;
* Understanding parameter binding for methods.
