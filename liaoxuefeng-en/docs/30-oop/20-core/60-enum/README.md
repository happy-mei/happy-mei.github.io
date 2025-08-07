<!-- TRANSLATED by md-translate -->
# Enumerated classes

In Java, we can define constants by `static final`. For example, if we wish to define 7 constants from Monday to Sunday, we can use 7 different `int` representations:

```java
public class Weekday {
    public static final int SUN = 0;
    public static final int MON = 1;
    public static final int TUE = 2;
    public static final int WED = 3;
    public static final int THU = 4;
    public static final int FRI = 5;
    public static final int SAT = 6;
}
```

When using constants, they can be referenced this way:

```java
if (day == Weekday.SAT || day == Weekday.SUN) {
    // TODO: work at home
}
```

It is also possible to define constants as string types, for example, defining constants for 3 colors:

```java
public class Color {
    public static final String RED = "r";
    public static final String GREEN = "g";
    public static final String BLUE = "b";
}
```

When using constants, they can be referenced this way:

```java
String color = ...
if (Color.RED.equals(color)) {
    // TODO:
}
```

One serious problem with using either `int` constants or `String` constants to represent a set of enumerated values is that the compiler can't check the reasonableness of each value. For example:

```java
if (weekday == 6 || weekday == 7) {
    if (tasks == Weekday.MON) {
        // TODO:
    }
}
```

The above code compiles and runs without errors, but there are two problems:

* Notice that `Weekday` defines constants in the range `0` to `6` and does not include `7`, and the compiler cannot check for `int` values that are not in the enumeration;
* Defined constants can still be compared to other variables, but their use is not to enumerate weekday values.

### enum

In order for the compiler to automatically check that a value is within the set of enumerations, and, since enumerations for different purposes require different types to be labeled and cannot be mixed, we can use `enum` to define enumeration classes:

```java
// enum
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day == Weekday.SAT || day == Weekday.SUN) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

Noting that defining enumeration classes is accomplished with the keyword `enum`, we simply list the constant names of the enumerations in order.

Defining enumerations with `enum` has the following advantages over `int`-defined constants:

First, the `enum` constant itself carries type information, i.e., the `Weekday.SUN` type is `Weekday`, and the compiler automatically checks for type errors. For example, the following statement is unlikely to compile:

```java
int day = 1;
if (day == Weekday.SUN) { // Compile error: bad operand types for binary operator '=='
}
```

Second, it's impossible to reference to a non-enumerated value because it won't pass compilation.

Finally, enumerations of different types cannot be compared or assigned to each other because the types do not match. For example, a variable of type `Weekday` enumeration cannot be assigned a value of type `Color` enumeration:

```java
Weekday x = Weekday.SUN; // ok!
Weekday y = Color.RED; // Compile error: incompatible types
```

This allows the compiler to automatically check for all possible potential errors at compile time.

### Comparison of enum

The enum class defined using `enum` is a reference type. As we mentioned earlier, to compare reference types, use the `equals()` method, and if you use the `==` comparison, it compares whether two variables of the reference type are the same object or not. Therefore, always use the `equals()` method for reference type comparisons, with the exception of `enum` types.

This is because each constant of type `enum` has only one unique instance in the JVM, so it can be compared directly with `==`:

```java
if (day == Weekday.FRI) { // ok!
}
if (day.equals(Weekday.SUN)) { // ok, but more code!
}
```

### enum type

What is the difference between an enum class defined via `enum` and any other `class`?

The answer is that there is no difference. The type defined by `enum` is `class`, except that it has the following characteristics:

* The type `enum` defined always inherits from `java.lang.Enum` and cannot be inherited;
* Only instances of `enum` can be defined, and instances of `enum` cannot be created by the `new` operator;
* Each instance defined is a unique instance of the reference type;
* The `enum` type can be used in `switch` statements.

For example, we define the `Color` enum class:

```java
public enum Color {
    RED, GREEN, BLUE;
}
```

The compiler compiles a `class` that looks roughly like this:

```java
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```

So there is no difference between the compiled `enum` class and a normal `class`. But we can't define `enum` by ourselves in the same way that we define a regular `class`; we have to use the `enum` keyword, which is required by the Java syntax.

Because `enum` is a `class`, the value of each enumeration is a `class` instance, and as such, these instances have a number of methods:

#### name()

Returns the name of a constant, for example:

```java
String s = Weekday.SUN.name(); // "SUN"
```

#### ordinal()

Returns the order of defined constants, counting from 0, for example:

```java
int n = Weekday.MON.ordinal(); // 1
```

Changing the order in which enumeration constants are defined causes the `ordinal()` return value to change. Example:

```java
public enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

cap (a poem)

```java
public enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

The `ordinal` of `enum` is the difference. If you write a statement like `if(x.ordinal()==1)` in your code, make sure that the enumeration order of `enum` cannot change. New constants must be added last.

Some kids may wonder if it's not very convenient to use `ordinal()` for `Weekday` enum constants if they are to be converted with `int`? For example, write it like this:

```java
String task = Weekday.MON.ordinal() + "/ppt";
saveToFile(task);
```

However, if the order of the enumerations is accidentally changed, the compiler cannot check for such logical errors. To write robust code, don't rely on `ordinal()` return values. Since `enum` is itself a `class`, we can define `private` constructor methods and, add fields to each enumeration constant:

```java
// enum
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day.dayValue == 6 || day.dayValue == 0) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);

    public final int dayValue;

    private Weekday(int dayValue) {
        this.dayValue = dayValue;
    }
}
```

This eliminates the need to worry about order changes, and when adding new enumeration constants, you also need to specify an `int` value.

```alert type=warning title=注意
枚举类的字段也可以是非final类型，即可以在运行期修改，但是不推荐这样做！
```

By default, calling `toString()` on enum constants returns the same string as `name()`. However, `toString()` can be overridden, whereas `name()` cannot. We can add the `toString()` method to `Weekday`:

```java
// enum
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day.dayValue == 6 || day.dayValue == 0) {
            System.out.println("Today is " + day + ". Work at home!");
        } else {
            System.out.println("Today is " + day + ". Work at office!");
        }
    }
}

enum Weekday {
    MON(1, "星期一"), TUE(2, "星期二"), WED(3, "星期三"), THU(4, "星期四"), FRI(5, "星期五"), SAT(6, "星期六"), SUN(0, "星期日");

    public final int dayValue;
    private final String chinese;

    private Weekday(int dayValue, String chinese) {
        this.dayValue = dayValue;
        this.chinese = chinese;
    }

    @Override
    public String toString() {
        return this.chinese;
    }
}
```

The purpose of overriding `toString()` is to make the output more readable.

```alert type=warning title=注意
判断枚举常量的名字，要始终使用name()方法，绝不能调用toString()！
```

### Switch

Finally, enumerated classes can be used in `switch` statements. Because enumerated classes inherently have type information and a limited number of enumeration constants, they are better suited for use in `switch` statements than `int` and `String` types:

```java
// switch
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

Adding the `default` statement allows you to automatically report an error if you miss writing one of the enumeration constants, so that you can catch the error in time.

### Summary

Java uses `enum` to define enumeration types, which are compiled by the compiler as `final class Xxx extends Enum { ... }`;

Get the string defined by the constant via `name()`, being careful not to use `toString()`;

Returns the order of constant definitions (without substance) via `ordinal()`;

Constructors, fields, and methods can be written for `enum`

The `enum` constructor is to be declared `private` and fields are strongly recommended to be declared `final`;

`enum` is suitable for use in `switch` statements.