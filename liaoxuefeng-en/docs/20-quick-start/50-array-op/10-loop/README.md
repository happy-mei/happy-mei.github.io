<!-- TRANSLATED by md-translate -->
# Traverse the array

We were introduced to arrays as a data type in Java Program Fundamentals. With arrays, we also need to manipulate it. And one of the most common operations on arrays is traversal.

An array can be traversed with a `for` loop. Since each element of an array can be accessed by index, traversing an array can be accomplished using the standard `for` loop:

```java
// 遍历数组
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=0; i<ns.length; i++) {
            int n = ns[i];
            System.out.println(n);
        }
    }
}
```

To implement a `for` loop traversal, the initial condition is `i=0`, because the index always starts at `0`, and the condition to continue the loop is `i<ns.length`, because when `i=ns.length`, `i` is out of the indexed range (the indexed range is `0` ~ `ns.length-1`), and after each loop, `i++`.

The second way is to use a `for each` loop that directly iterates over each element of the array:

```java
// 遍历数组
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

Note: In the `for (int n : ns)` loop, the variable `n` gets the elements of the `ns` array directly, not the index.

Obviously a `for each` loop is more concise. However, the `for each` loop can't get the index of the array, so which `for` loop to use depends on our needs.

### Print the contents of an array

Printing the array variable directly gives you the reference address of the array in the JVM:

```java
int[] ns = { 1, 1, 2, 3, 5, 8 };
System.out.println(ns); // 类似 [I@7852e922
```

This doesn't make much sense, since we want the contents of the elements of the array to be printed. So use a `for each` loop to print it:

```java
int[] ns = { 1, 1, 2, 3, 5, 8 };
for (int n : ns) {
    System.out.print(n + ", ");
}
```

Printing using `for each` loops is also cumbersome. Luckily, the Java standard library provides `Arrays.toString()` to quickly print the contents of an array:

```java
// 遍历数组
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 1, 2, 3, 5, 8 };
        System.out.println(Arrays.toString(ns));
    }
}
```

### Exercise

Please iterate through the array in reverse order and print each element:

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        // 倒序打印数组元素:
        for (???) {
            System.out.println(???);
        }
    }
}
```

[Download exercise](array-loop.zip)

### Summary

Iterating through an array can be done using a `for` loop, which accesses the array index, and a `for each` loop, which iterates directly over each array element, but does not get the index;

Use `Arrays.toString()` to quickly get the contents of an array.