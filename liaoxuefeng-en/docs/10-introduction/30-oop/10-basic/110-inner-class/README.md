<!-- TRANSLATED by md-translate -->
# Internal classes

In Java programs, usually, we organize different classes under different packages, and for the classes under a package, they are at the same level and have no parent-child relationship:

```ascii
java.lang
├── Math
├── Runnable
├── String
└── ...
```

There is also a class that is defined inside another class, so it is called the internal class (Nested Class).Java's internal classes are divided into several types, usually not used much, but it is necessary to understand how they are used.

### Inner Class

If a class is defined inside another class, that class is an Inner Class:

```java
class Outer {
    class Inner {
        // 定义了一个Inner Class
    }
}
```

The above definition of `Outer` is an ordinary class, and `Inner` is an Inner Class, it has a big difference with the ordinary class, that is, Inner Class instances can not exist alone, must be dependent on an instance of Outer Class. Sample code is as follows:

```java
// inner class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested"); // 实例化一个Outer
        Outer.Inner inner = outer.new Inner(); // 实例化一个Inner
        inner.hello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    class Inner {
        void hello() {
            System.out.println("Hello, " + Outer.this.name);
        }
    }
}
```

Observing the above code, to instantiate an `Inner`, we must first create an instance of `Outer` and then, call `new` on the `Outer` instance to create the `Inner` instance:

```java
Outer.Inner inner = outer.new Inner();
```

This is because the Inner Class, in addition to having a `this` pointing to itself, implicitly holds an Outer Class instance, which can be accessed with `Outer.this`. So, instantiating an Inner Class cannot be detached from the Outer instance.

Inner Class has an extra "privilege" compared to normal Class, except that it can refer to Outer instances, that is, it can modify the `private` field of Outer Class, because the scope of Inner Class is inside Outer Class, so it can access the `private` field and methods of Outer Class. Because Inner Class is scoped inside Outer Class, it can access `private` fields and methods of Outer Class.

Looking at the `.class` file compiled by the Java compiler you can see that the `Outer` class is compiled as `Outer.class` and the `Inner` class is compiled as `Outer$Inner.class`.

### Anonymous Class

There is another way to define Inner Class, it does not need to explicitly define this Class in Outer Class, but inside the method, through the anonymous class (Anonymous Class) to define. The example code is as follows:

```java
// Anonymous Class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested");
        outer.asyncHello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    void asyncHello() {
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello, " + Outer.this.name);
            }
        };
        new Thread(r).start();
    }
}
```

Observing the `asyncHello()` method, we instantiate a `Runnable` inside the method. `Runnable` itself is an interface, and interfaces can't be instantiated, so here you are actually defining an anonymous class that implements the `Runnable` interface and instantiating that anonymous class via `new`, which then transforms to `Runnable`. You have to instantiate the anonymous class when you define it, and defining an anonymous class is written as follows:

```java
Runnable r = new Runnable() {
    // 实现必要的抽象方法...
};
```

Anonymous Classes, like Inner Classes, have access to the `private` fields and methods of Outer Classes. The reason why we define an anonymous class is because we usually don't care about the class name here, and we can write a lot less code than if we define the Inner Class directly.

Looking at the `.class` file compiled by the Java compiler you can see that the `Outer` class is compiled as `Outer.class` and the anonymous class is compiled as `Outer$1.class`. If there is more than one anonymous class, the Java compiler names each anonymous class `Outer$1`, `Outer$2`, `Outer$3` in turn ......

In addition to interfaces, anonymous classes are perfectly capable of inheriting from normal classes. Observe the following code:

```java
// Anonymous Class
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, String> map1 = new HashMap<>();
        HashMap<String, String> map2 = new HashMap<>() {}; // 匿名类!
        HashMap<String, String> map3 = new HashMap<>() {
            {
                put("A", "1");
                put("B", "2");
            }
        };
        System.out.println(map3.get("A"));
    }
}
```

`map1` is a normal `HashMap` instance, but `map2` is an anonymous class instance, except that the anonymous class inherits from `HashMap`. `map3` is also an instance of an anonymous class that inherits from `HashMap` and adds a `static` block to initialize the data. Observing the compilation output reveals two anonymous class files, `Main$1.class` and `Main$2.class`.

### Static Nested Class

The last type of inner class is similar to Inner Class but uses the `static` modifier and is called Static Nested Class:

```java
// Static Nested Class
public class Main {
    public static void main(String[] args) {
        Outer.StaticNested sn = new Outer.StaticNested();
        sn.hello();
    }
}

class Outer {
    private static String NAME = "OUTER";

    private String name;

    Outer(String name) {
        this.name = name;
    }

    static class StaticNested {
        void hello() {
            System.out.println("Hello, " + Outer.NAME);
        }
    }
}
```

An inner class modified with `static` is very different from an Inner Class in that it is no longer dependent on an instance of `Outer`, but is a completely separate class, and therefore cannot reference `Outer.this`, but it does have access to `private` static fields and static methods of `Outer`. If you move `StaticNested` outside of `Outer`, you lose access to `private`.

### Summary

Java's internal classes can be categorized into Inner Class, Anonymous Class and Static Nested Class;

Inner Class and Anonymous Class are essentially the same, both must be dependent on an instance of Outer Class, i.e., implicitly hold an instance of `Outer.this` and have `private` access to Outer Class;

Static Nested Class is a standalone class, but has the `private` access of Outer Class.