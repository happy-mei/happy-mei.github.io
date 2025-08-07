<!-- TRANSLATED by md-translate -->
# while loop

A loop statement is a statement that allows the computer to do a loop calculation based on a condition, continuing the loop when the condition is met and exiting the loop when the condition is not met.

For example, calculate the sum from 1 to 100:

```plain
1 + 2 + 3 + 4 + … + 100 = ?
```

In addition to using the series formula, it is entirely possible to let the computer to do 100 times the cycle of accumulation. Because the computer is characterized by a very fast speed of calculation, we let the computer cycle 100 million times also use less than 1 second, so many calculations of the task, the human go to sort of can not be counted, but the computer counts, using the cycle of this simple and brutal method can quickly get the results.

We'll start by looking at the `while` conditional loop provided by Java. Its basic usage is:

```java
while (条件表达式) {
    循环语句
}
// 继续执行后续代码
```

The `while` loop begins each loop by determining whether the condition holds. If the result is `true`, the statement inside the loop is executed, and if the result is `false`, the loop jumps to the end of the `while` loop and continues on.

We use a while loop to accumulate 1 to 100, which can be written like this:

```java
// while
public class Main {
    public static void main(String[] args) {
        int sum = 0; // 累加的和，初始化为0
        int n = 1;
        while (n <= 100) { // 循环条件是n <= 100
            sum = sum + n; // 把n累加到sum中
            n ++; // n自身加1
        }
        System.out.println(sum); // 5050
    }
}
```

Notice that the `while` loop determines the loop condition before looping, so it is possible to loop without doing anything at all once.

Special attention should be paid to boundary conditions for looping conditional judgments, as well as for the handling of self-incrementing variables. Think about why the following code does not get the correct result:

```java
// while
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        int n = 0;
        while (n <= 100) {
            n ++;
            sum = sum + n;
        }
        System.out.println(sum);
    }
}
```

If the loop condition is always satisfied, then the loop becomes a dead loop. A dead loop will result in 100% CPU usage and the user will feel that the computer is running slowly, so avoid writing dead loop code.

If the logic of the loop condition is written in a faulty way, it can also cause unexpected results:

```java
// while
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        int n = 1;
        while (n > 0) {
            sum = sum + n;
            n ++;
        }
        System.out.println(n); // -2147483648
        System.out.println(sum);
    }
}
```

On the surface, the above `while` loop is a dead loop, but Java's `int` type has a maximum value, and when it reaches the maximum value, adding 1 will make it negative, and as a result, the `while` loop exits unexpectedly.

### Exercise

Use `while` to compute the sum from `m` to `n`:

```java
// while
public class Main {
    public static void main(String[] args) {
    	int sum = 0;
    	int m = 20;
    	int n = 100;
    	// 使用while计算M+...+N:
    	while (false) {
    	}
    	System.out.println(sum);
    }
}
```

[Download exercise](flow-while.zip)

### Summary

The `while` loop first determines whether the loop condition is satisfied before executing the loop statement;

The `while` loop may not execute once;

Write loops with loop conditions in mind and avoid dead loops.