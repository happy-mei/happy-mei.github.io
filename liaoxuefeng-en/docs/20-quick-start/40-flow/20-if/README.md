<!-- TRANSLATED by md-translate -->
# if conditional judgment

In a Java program, an `if` statement is needed if you want to decide whether or not to execute a piece of code based on a condition.

The basic syntax of an `if` statement is:

```java
if (条件) {
    // 条件满足时执行
}
```

Depending on the result of the `if` calculation (`true` or `false`), the JVM decides whether or not to execute the `if` statement block (i.e., all statements contained in the curly braces {}).

Let's look at an example:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 70;
        if (n >= 60) {
            System.out.println("及格了");
        }
        System.out.println("END");
    }
}
```

When the condition `n >= 60` is calculated as `true`, the `if` block is executed and `"Passed"` is printed, otherwise the `if` block is skipped. Modify the value of `n` to see the effect of execution.

Notice that the `if` statement contains a block that can contain multiple statements:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 70;
        if (n >= 60) {
            System.out.println("及格了");
            System.out.println("恭喜你");
        }
        System.out.println("END");
    }
}
```

The curly braces {} can be omitted when the `if` statement block has only one line of statements:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 70;
        if (n >= 60)
            System.out.println("及格了");
        System.out.println("END");
    }
}
```

However, omitting the curly braces is not always a good idea. Suppose that at some point it suddenly becomes desirable to add a statement to the `if` statement block:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 50;
        if (n >= 60)
            System.out.println("及格了");
            System.out.println("恭喜你"); // 注意这条语句不是if语句块的一部分
        System.out.println("END");
    }
}
```

Due to the use of indented formatting, it is easy to see both lines of a statement as an execution block of an `if` statement, when in fact only the first line of the statement is an execution block of an `if`. This is more likely to be a problem when using version control systems like git to automate merges, so ignoring the curly braces is not recommended.

### else

The `if` statement can also be written with an `else { ... }`, which will execute the `else` block of statements when the conditional judgment is `false`:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 70;
        if (n >= 60) {
            System.out.println("及格了");
        } else {
            System.out.println("挂科了");
        }
        System.out.println("END");
    }
}
```

Modify the value of `n` in the above code and observe the block of statements executed by the program when the `if` condition is `true` or `false`.

Note that `else` is not required.

It is also possible to use multiple `if ... else if ... ` in series. Example:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 70;
        if (n >= 90) {
            System.out.println("优秀");
        } else if (n >= 60) {
            System.out.println("及格了");
        } else {
            System.out.println("挂科了");
        }
        System.out.println("END");
    }
}
```

The effect of the tandem is actually equivalent:

```java
if (n >= 90) {
    // n >= 90为true:
    System.out.println("优秀");
} else {
    // n >= 90为false:
    if (n >= 60) {
        // n >= 60为true:
        System.out.println("及格了");
    } else {
        // n >= 60为false:
        System.out.println("挂科了");
    }
}
```

When using multiple `if`s in series, pay _special attention_ to the order of judgment. Observe the code below:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 100;
        if (n >= 60) {
            System.out.println("及格了");
        } else if (n >= 90) {
            System.out.println("优秀");
        } else {
            System.out.println("挂科了");
        }
    }
}
```

Execution reveals that the condition `n >= 90` is satisfied when `n = 100`, but the output is not `"excellent"` but `"passed"`. The reason is that when the `if` statement is executed from top to bottom, it first determines that `n >= 60` succeeds, and then the subsequent `else`s are no longer executed, and thus `if (n >= 90)` does not have a chance to be executed.

The correct way to do this is to follow the range of judgments in order from largest to smallest:

```java
// 从大到小依次判断：
if (n >= 90) {
    // ...
} else if (n >= 60) {
    // ...
} else {
    // ...
}
```

Or rewrite it to judge in order from smallest to largest:

```java
// 从小到大依次判断：
if (n < 60) {
    // ...
} else if (n < 90) {
    // ...
} else {
    // ...
}
```

Special attention should also be paid to boundary conditions when using `if`. For example:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        int n = 90;
        if (n > 90) {
            System.out.println("优秀");
        } else if (n >= 60) {
            System.out.println("及格了");
        } else {
            System.out.println("挂科了");
        }
    }
}
```

Assuming that we expect a score of 90 or higher to be "excellent", the above code outputs "pass" because `>` and `>=` have different effects.

As mentioned earlier floating-point numbers are often not represented precisely in computers, and calculations can be inaccurate, so it is not reliable to use the `==` judgment for determining the equality of floating-point numbers:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        double x = 1 - 9.0 / 10;
        if (x == 0.1) {
            System.out.println("x is 0.1");
        } else {
            System.out.println("x is NOT 0.1");
        }
    }
}
```

The correct way to determine this is to use the fact that the difference is less than some critical value:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        double x = 1 - 9.0 / 10;
        if (Math.abs(x - 0.1) < 0.00001) {
            System.out.println("x is 0.1");
        } else {
            System.out.println("x is NOT 0.1");
        }
    }
}
```

### Determining the equality of reference types

In Java, to determine whether variables of value types are equal, you can use the `==` operator. However, to determine whether variables of reference types are equal, `==` indicates "whether the references are equal", or whether they point to the same object. For example, the following two String types, which have the same content but point to different objects, are judged by `==`, resulting in `false`:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1);
        System.out.println(s2);
        if (s1 == s2) {
            System.out.println("s1 == s2");
        } else {
            System.out.println("s1 != s2");
        }
    }
}
```

To determine whether the contents of variables of reference types are equal, you must use the `equals()` method:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1);
        System.out.println(s2);
        if (s1.equals(s2)) {
            System.out.println("s1 equals s2");
        } else {
            System.out.println("s1 not equals s2");
        }
    }
}
```

Note: When executing the statement `s1.equals(s2)`, if the variable `s1` is `null`, a `NullPointerException` is reported:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

To avoid the `NullPointerException` error, you can utilize the short-circuit operator `&&`:

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1 != null && s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

It is also possible to put the object `"hello"`, which must not be `null`, in front: e.g. `if ("hello".equals(s)) { ... }`.

### Exercise

Write a program using `if ... else` to write a program to calculate the body mass index BMI and print the result.

```plain
BMI = 体重(kg) / 身高(m)的平方
```

BMI results:

* Hypertonic: less than 18.5
* Normal: 18.5 ~ 25
* Overweight: 25 ~ 28
* Obese: 28 ~ 32
* Very obese: above 32

[Download exercise](flow-if.zip)

### Summary

`if ... else` can do conditional judgment, `else` is optional;

The omission of the curly brackets `{}` is not recommended;

Multiple `if ... else` in series should pay special attention to the order of judgment;

Be aware of the `if` boundary condition;

Be aware that floating-point number equality judgments cannot be made directly with the `==` operator;

Use `equals()` for reference types to determine that the contents are equal, taking care to avoid `NullPointerException`.