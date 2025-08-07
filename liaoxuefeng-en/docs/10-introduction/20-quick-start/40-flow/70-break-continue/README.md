<!-- TRANSLATED by md-translate -->
# break and continue

Two special statements that can be used in either a `while` loop or a `for` loop are the `break` statement and the `continue` statement.

### Break

During a loop, you can use the `break` statement to jump out of the current loop. Let's look at an example:

```java
// break
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i=1; ; i++) {
            sum = sum + i;
            if (i == 100) {
                break;
            }
        }
        System.out.println(sum);
    }
}
```

When calculating from 1 to 100 using a `for` loop, we do not set a test condition in `for()` for the loop to exit. However, inside the loop, we use `if` to determine that if `i==100`, we exit the loop by `break`.

For this reason, the `break` statement is usually used in conjunction with an `if` statement. In particular, note that the `break` statement always jumps out of its own level of the loop. For example:

```java
// break
public class Main {
    public static void main(String[] args) {
        for (int i=1; i<=10; i++) {
            System.out.println("i = " + i);
            for (int j=1; j<=10; j++) {
                System.out.println("j = " + j);
                if (j >= i) {
                    break;
                }
            }
            // break跳到这里
            System.out.println("breaked");
        }
    }
}
```

The code above is two `for` loops nested together. Because the `break` statement is in the inner `for` loop, it will jump out of the inner `for` loop, but not out of the outer `for` loop.

### continue

`break` jumps out of the current loop, i.e. the whole loop is not executed. `continue`, on the other hand, ends the current loop early and continues directly to the next loop. Let's look at an example:

```java
// continue
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i=1; i<=10; i++) {
            System.out.println("begin i = " + i);
            if (i % 2 == 0) {
                continue; // continue语句会结束本次循环
            }
            sum = sum + i;
            System.out.println("end i = " + i);
        }
        System.out.println(sum); // 25
    }
}
```

Notice the effect of the `continue` statement. When `i` is odd, the entire loop is executed in its entirety, so `begin i=1` and `end i=1` are printed. When `i` is even, the `continue` statement ends the loop early, so `begin i=2` is printed but not `end i=2`.

In multi-level nested loops, the `continue` statement also ends the current loop it is in.

### Summary

The `break` statement jumps out of the current loop;

The `break` statement is often used in conjunction with `if` to end the entire loop early when a condition is met;

The `break` statement always jumps out of the nearest layer of the loop;

The `continue` statement ends this loop early;

The `continue` statement is usually used in conjunction with `if` to end this loop early when a condition is met.