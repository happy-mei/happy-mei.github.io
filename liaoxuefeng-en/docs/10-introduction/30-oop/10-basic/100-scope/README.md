<!-- TRANSLATED by md-translate -->
# Scope

In Java, we often see the modifiers `public`, `protected`, and `private`. In Java, these modifiers can be used to limit the access scope.

### public

A `class`, `interface` defined as `public` can be accessed by any other class:

```java
package abc;

public class Hello {
    public void hi() {
    }
}
```

The `Hello` above is `public` and, therefore, can be accessed by classes in other packages:

```java
package xyz;

class Main {
    void foo() {
        // Main可以访问Hello
        Hello h = new Hello();
    }
}
```

A `field`, `method` defined as `public` can be accessed by other classes, provided there is access to `class` first:

```java
package abc;

public class Hello {
    public void hi() {
    }
}
```

The `hi()` method above is `public` and can be called by other classes, provided that the `Hello` class is accessible first:

```java
package xyz;

class Main {
    void foo() {
        Hello h = new Hello();
        h.hi();
    }
}
```

### Private

A `field`, `method` defined as `private` cannot be accessed by other classes:

```java
package abc;

public class Hello {
    // 不能被其他类调用:
    private void hi() {
    }

    public void hello() {
        this.hi();
    }
}
```

In fact, precisely `private` access is restricted to the interior of the `class` and is _irrelevant_ to the order in which the methods are declared. It is recommended to put `private` methods later, because `public` methods define the functionality that the class provides externally, and when reading the code, you should focus on `public` methods first:

```java
package abc;

public class Hello {
    public void hello() {
        this.hi();
    }

    private void hi() {
    }
}
```

Since Java supports nested classes, if a nested class is also defined inside a class, the nested class has access to `private`:

```java
// private
public class Main {
    public static void main(String[] args) {
        Inner i = new Inner();
        i.hi();
    }

    // private方法:
    private static void hello() {
        System.out.println("private hello!");
    }

    // 静态内部类:
    static class Inner {
        public void hi() {
            Main.hello();
        }
    }
}
```

A `class` defined inside a `class` is called a nested class, and Java supports several kinds of nested classes.

### protected

`protected` acts on inheritance relationships. Fields and methods defined as `protected` can be accessed by subclasses, as well as subclasses of subclasses:

```java
package abc;

public class Hello {
    // protected方法:
    protected void hi() {
    }
}
```

The `protected` method above can be accessed by inherited classes:

```java
package xyz;

class Main extends Hello {
    void foo() {
        // 可以访问protected方法:
        hi();
    }
}
```

### package

Finally, package scope means that a class is allowed to access the same `package`'s `class` without the `public`, `private` modifiers, as well as fields and methods without the `public`, `protected`, and `private` modifiers.

```java
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}
```

Access to `class`, `field` and `method` with `package` permissions, as long as they are in the same package:

```java
package abc;

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```

Note that package names must be identical, packages have no parent-child relationship, `com.apache` and `com.apache.abc` are different packages.

### Local variables

Variables defined inside a method are called local variables, and the local variable scope begins at the variable declaration and ends at the corresponding block. Method parameters are also local variables.

```java
package abc;

public class Hello {
    void hi(String name) { // 1
        String s = name.toLowerCase(); // 2
        int len = s.length(); // 3
        if (len < 10) { // 4
            int p = 10 - len; // 5
            for (int i=0; i<10; i++) { // 6
                System.out.println(); // 7
            } // 8
        } // 9
    } // 10
}
```

Let's observe the `hi()` method code above:

* The method parameter name is a local variable whose scope is the entire method, i.e., 1 ~ 10;
* The variable s is scoped from the point of definition to the end of the method, i.e. 2 ~ 10;
* The scope of the variable len is from the definition to the end of the method, i.e. 3 ~ 10;
* The scope of variable p is from the definition to the end of the if block, i.e. 5 ~ 9;
* The scope of variable i is the for loop, i.e. 6 ~ 8.

When using local variables, you should keep the scope of local variables as small as possible and delay declaring local variables as long as possible.

### final

Java also provides a `final` modifier. `final` does not conflict with access rights; it serves many purposes.

Modifying `class` with `final` prevents it from being inherited:

```java
package abc;

// 无法被继承:
public final class Hello {
    private int n = 0;
    protected void hi(int t) {
        long i = t;
    }
}
```

Modifying `method` with `final` prevents it from being overridden by subclasses:

```java
package abc;

public class Hello {
    // 无法被覆写:
    protected final void hi() {
    }
}
```

Modifying `field` with `final` prevents it from being reassigned:

```java
package abc;

public class Hello {
    private final int n = 0;
    protected void hi() {
        this.n = 1; // error!
    }
}
```

Modifying a local variable with `final` prevents it from being reassigned:

```java
package abc;

public class Hello {
    protected void hi(final int t) {
        t = 1; // error!
    }
}
```

### Best practices

If you're not sure if `public` is needed, don't declare it as `public`, i.e. expose as few external fields and methods as possible.

Defining methods with `package` permissions helps with testing because test code can access `package` permission methods of the tested class as long as the test class and the tested class are located on the same `package`.

A `.java` file can contain only one `public` class, but can contain multiple non-`public` classes. If there is a `public` class, the file name must be the same as the name of the `public` class.

### Summary

Java's built-in access permissions include `public`, `protected`, `private`, and `package` permissions;

Variables defined by Java inside a method are local variables, and the scope of a local variable begins with the variable declaration and ends with a block;

The `final` modifier is not an access permission; it can modify `class`, `field`, and `method`;

A `.java` file can contain only one `public` class, but can contain multiple non-`public` classes.