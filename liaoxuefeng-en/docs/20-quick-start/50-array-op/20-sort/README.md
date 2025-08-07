<!-- TRANSLATED by md-translate -->
# Array sorting

Sorting an array is a very basic requirement in a program. Commonly used sorting algorithms are bubble sort, insertion sort and quick sort.

Let's look at how to sort an integer array from smallest to largest using the bubble sort algorithm:

```java
// 冒泡排序
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        // 排序前:
        System.out.println(Arrays.toString(ns));
        for (int i = 0; i < ns.length - 1; i++) {
            for (int j = 0; j < ns.length - i - 1; j++) {
                if (ns[j] > ns[j+1]) {
                    // 交换ns[j]和ns[j+1]:
                    int tmp = ns[j];
                    ns[j] = ns[j+1];
                    ns[j+1] = tmp;
                }
            }
        }
        // 排序后:
        System.out.println(Arrays.toString(ns));
    }
}
```

The characteristic of bubble sort is that after each cycle, the largest number is exchanged at the end, so that the next cycle "shaves off" the last number, and each cycle is one place ahead of the end of the previous cycle.

Also, notice that exchanging the values of two variables must be done with the help of a temporary variable. It is wrong to write it like this:

```java
int x = 1;
int y = 2;

x = y; // x现在是2
y = x; // y现在还是2
```

The correct way to write it is:

```java
int x = 1;
int y = 2;

int t = x; // 把x的值保存在临时变量t中, t现在是1
x = y; // x现在是2
y = t; // y现在是t的值1
```

In fact, Java's standard library has built-in sorting functionality, we just need to call `Arrays.sort()` provided by the JDK to sort:

```java
// 排序
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        Arrays.sort(ns);
        System.out.println(Arrays.toString(ns));
    }
}
```

It is important to note that sorting an array actually modifies the array itself. For example, the array before sorting is:

```java
int[] ns = { 9, 3, 6, 5 };
```

In memory, this integer array is represented as follows:

```ascii
┌───┬───┬───┬───┐
ns───▶│ 9 │ 3 │ 6 │ 5 │
      └───┴───┴───┴───┘
```

When we call `Arrays.sort(ns);`, this integer array becomes in memory:

```ascii
┌───┬───┬───┬───┐
ns───▶│ 3 │ 5 │ 6 │ 9 │
      └───┴───┴───┴───┘
```

That is, the contents of the array pointed to by the variable `ns` have been changed.

If you sort an array of strings. for example:

```java
String[] ns = { "banana", "apple", "pear" };
```

Before sorting, this array is represented in memory as follows:

```ascii
┌──────────────────────────────────┐
               ┌───┼──────────────────────┐           │
               │   │                      ▼           ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                 ▲
           └─────────────────┘
```

After calling `Arrays.sort(ns);` sort, this array is represented in memory as follows:

```ascii
┌──────────────────────────────────┐
               ┌───┼──────────┐                       │
               │   │          ▼                       ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                              ▲
           └──────────────────────────────┘
```

None of the original 3 strings changed in memory, but each element of the `ns` array pointed to a change.

### Exercise

Think about how to implement a descending sort of an array:

```java
// 降序排序
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        // 排序前:
        System.out.println(Arrays.toString(ns));
        // TODO:
        // 排序后:
        System.out.println(Arrays.toString(ns));
        if (Arrays.toString(ns).equals("[96, 89, 73, 65, 50, 36, 28, 18, 12, 8]")) {
            System.out.println("测试成功");
        } else {
            System.out.println("测试失败");
        }
    }
}
```

[Download exercise](array-sort.zip)

### Summary

Commonly used sorting algorithms are bubble sort, insertion sort and quick sort;

Bubble sort uses a two-level `for` loop to implement the sort;

Swapping the values of two variables requires the help of a temporary variable;

Sorting can be done directly using `Arrays.sort()` provided by the Java standard library;

Sorting an array directly modifies the array itself.