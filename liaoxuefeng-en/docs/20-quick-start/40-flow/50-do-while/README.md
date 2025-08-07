<!-- TRANSLATED by md-translate -->
# do while loop

In Java, a `while` loop is a loop that first determines the loop condition and then executes the loop. The other type of `do while` loop is to execute the loop first, then judge the condition, continue the loop when the condition is satisfied, and exit when the condition is not satisfied. Its usage is:

```java
do {
    执行循环语句
} while (条件表达式);
```

As you can see, the `do while` loop will loop at least once.

Let's rewrite the summation of 1 to 100 in a `do while` loop:

```java
// do-while
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        int n = 1;
        do {
            sum = sum + n;
            n ++;
        } while (n <= 100);
        System.out.println(sum);
    }
}
```

When using `do while` loops, the same care should be taken with the loop conditions.

### Exercise

Use a `do while` loop to calculate the sum from `m` to `n`.

```java
// do while
public class Main {
    public static void main(String[] args) {
    	int sum = 0;
        int m = 20;
    	int n = 100;
    	// 使用do while计算M+...+N:
    	do {
    	} while (false);
    	System.out.println(sum);
    }
}
```

[Download exercise](flow-do-while.zip)

### Summary

`do while` executes the loop first and then judges the condition;

The `do while` loop will execute at least once.