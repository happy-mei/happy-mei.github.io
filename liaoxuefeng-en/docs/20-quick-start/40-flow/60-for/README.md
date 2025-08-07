<!-- TRANSLATED by md-translate -->
# for loop

Apart from `while` and `do while` loops, the most widely used in Java is the `for` loop.

The `for` loop is very powerful in that it uses a counter to implement the loop. The `for` loop initializes the counter and then, before each loop, detects the loop condition and updates the counter after each loop. The counter variable is usually named `i`.

Let's rewrite the 1 to 100 summation in a `for` loop:

```java
// for
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i=1; i<=100; i++) {
            sum = sum + i;
        }
        System.out.println(sum);
    }
}
```

Before the `for` loop is executed, the initialization statement `int i=1` is executed, which defines the counter variable `i` and assigns it an initial value of `1`, then the loop condition `i<=100` is checked before the loop, and the loop is automatically executed after the loop `i++`, so the `for` loop puts the code for updating the counter in unison, compared to the `while` loop. There is no need to update the variable `i` inside the body of the `for` loop.

Thus, the `for` loop is used:

```java
for (初始条件; 循环检测条件; 循环后更新计数器) {
    // 执行语句
}
```

If we want to sum all the elements of an integer array, we can do this with a `for` loop:

```java
// for
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        int sum = 0;
        for (int i=0; i<ns.length; i++) {
            System.out.println("i = " + i + ", ns[i] = " + ns[i]);
            sum = sum + ns[i];
        }
        System.out.println("sum = " + sum);
    }
}
```

The loop condition of the above code is `i<ns.length`. Since the length of the `ns` array is `5`, when the value of `i` is updated to `5` after looping `5` times, the looping condition is not satisfied and the `for` loop ends.

> [!TIP]思考
>
> 如果把循环条件改为i<=ns.length，会出现什么问题？

Note that the initialization counter for `for` loops is always executed, and that `for` loops may also loop 0 times.

When using a `for` loop, never modify the counter inside the loop body! Modifying the counter inside the loop body often leads to inexplicable logic errors. For the following code:

```java
// for
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=0; i<ns.length; i++) {
            System.out.println(ns[i]);
            i = i + 1;
        }
    }
}
```

Although no error is reported, only half of the array elements are printed because the `i = i + 1` inside the loop causes the counter variable to actually add `2` each time it loops (since the `for` loop also automatically executes `i++`). Therefore, do not modify the value of the counter in the `for` loop. The initialization of the counter, the judgment condition, and the update condition after each loop can be placed uniformly in the `for()` statement at a glance.

If you wish to access only the array elements with even index numbers, you should rewrite the `for` loop as:

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i=i+2) {
    System.out.println(ns[i]);
}
```

This is accomplished by updating the counter with the statement `i=i+2`, thus avoiding the need to modify the variable `i` inside the loop.

When using a `for` loop, the counter variable `i` should be defined in the `for` loop as much as possible:

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
// 无法访问i
int n = i; // compile error!
```

If the variable `i` is defined outside the `for` loop:

```java
int[] ns = { 1, 4, 9, 16, 25 };
int i;
for (i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
// 仍然可以使用i
int n = i;
```

Then, after exiting the `for` loop, the variable `i` can still be accessed, which breaks the principle that variables should be minimized for access.

### Flexible use of for loops

The `for` loop can also be missing the initialization statement, the loop condition, and the update statement for each loop, for example:

```java
// 不设置结束条件:
for (int i=0; ; i++) {
    ...
}
```

```java
// 不设置结束条件和更新语句:
for (int i=0; ;) {
    ...
}
```

```java
// 什么都不设置:
for (;;) {
    ...
}
```

This is not usually recommended, but there are cases where it is possible to omit certain statements from a `for` loop.

### for each loop

The `for` loop is often used to iterate through arrays because the counter allows you to access each element of the array by index:

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
```

However, many times what we actually really want to access is the value of each element of the array.Java also provides another `for each` loop that makes it simpler to iterate through the array:

```java
// for each
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

Compared to a `for` loop, a `for each` loop's variable n is no longer a counter, but corresponds directly to each element of the array. The `for each` loop is also more concise. However, the `for each` loop cannot specify the order of traversal, nor can it get the index of the array.

In addition to arrays, the `for each` loop can iterate over all "iterable" data types, including `List`, `Map`, and so on, which will be described later.

### Exercise 1

Given an array, output each element in reverse order using a `for` loop:

```java
// for
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=?; ???; ???) {
            System.out.println(ns[i]);
        }
    }
}
```

### Exercise 2

Sums each element of an array using a `for each` loop:

```java
// for each
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        int sum = 0;
        for (???) {
            // TODO
        }
        System.out.println(sum); // 55
    }
}
```

### Exercise 3

The circumference π can be calculated using the formula:

```math
\frac{\mathrm\pi}4=1-\frac13+\frac15-\frac17+\frac19-\dots
```

Calculate π using a `for` loop:

```java
// for
public class Main {
    public static void main(String[] args) {
        double pi = 0;
        for (???) {
            // TODO
        }
        System.out.println(pi);
    }
}
```

[Download Exercise](flow-for.zip)

### Summary

The `for` loop allows complex loops through counters;

The `for each` loop can directly iterate through each element of the array;

Best practice: the counter variable is defined inside the `for` loop and the counter is not modified inside the loop body;