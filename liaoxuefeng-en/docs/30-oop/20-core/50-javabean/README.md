<!-- TRANSLATED by md-translate -->
# JavaBean

In Java, there are many `class` definitions that conform to such a specification:

* Several `private` instance fields;
* Read and write instance fields through `public` methods.

Example:

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }
}
```

If the read and write methods conform to this naming convention below:

```java
// 读方法:
public Type getXyz()
// 写方法:
public void setXyz(Type value)
```

Then this `class` is called a `JavaBean`:

![java-bean](javabean.jpg)

The field above is `xyz`, so the read/write method names start with `get` and `set`, respectively, and are followed by the field name `Xyz` in uppercase, so the two read/write method names are `getXyz()` and `setXyz()`, respectively.

The `boolean` field is special in that its read method is generally named `isXyz()`:

```java
// 读方法:
public boolean isChild()
// 写方法:
public void setChild(boolean value)
```

We usually refer to a set of corresponding read methods (`getter`) and write methods (`setter`) as a property (`property`). For example, the `name` property:

* The corresponding read method is `String getName()`.
* The corresponding write method is `setName(String)`.

Properties with only a `getter` are called read-only properties, for example, defining an age read-only property:

* The corresponding read method is `int getAge()`.
* No corresponding write method `setAge(int)`

Similarly, properties with only `setter` are called write-only properties.

Obviously, read-only attributes are common and write-only attributes are not.

Attributes only require `getter` and `setter` methods to be defined, not necessarily corresponding fields. For example, the `child` read-only attribute is defined as follows:

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }

    public boolean isChild() {
        return age <= 6;
    }
}
```

As you can see, `getter` and `setter` are also methods of data encapsulation.

### The role of JavaBean

JavaBean is mainly used to pass data, that is, a set of data combined into a JavaBean for easy transmission. In addition, JavaBean can be easily analyzed by IDE tools to generate code for reading and writing properties, mainly used in the visual design of graphical interfaces.

With the IDE, `getter` and `setter` can be generated quickly. For example, in Eclipse, start by typing the following code:

```java
public class Person {
    private String name;
    private int age;
}
```

Then, right click, in the pop-up menu, select "Source", "Generate Getters and Setters", in the pop-up dialog box, select the fields you need to generate `getter` and `setter` methods. getter` and `setter` methods in the pop-up dialog box, click OK to have the IDE automatically complete all the method code.

### Enumerating JavaBean properties

To enumerate all the properties of a JavaBean, you can directly use the `Introspector` provided by the Java core library:

```java
import java.beans.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BeanInfo info = Introspector.getBeanInfo(Person.class);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            System.out.println(pd.getName());
            System.out.println("  " + pd.getReadMethod());
            System.out.println("  " + pd.getWriteMethod());
        }
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Running the above code lists all the attributes, and the corresponding read and write methods. Note that the `class` attribute is brought from the `getClass()` method inherited from `Object`.

### Summary

A JavaBean is a `class` that conforms to a naming convention and defines properties via `getter` and `setter`;

Attribute is a generic name, not a Java syntax requirement;

The IDE can be used to quickly generate `getter` and `setter`;

Use `Introspector.getBeanInfo()` to get the list of properties.