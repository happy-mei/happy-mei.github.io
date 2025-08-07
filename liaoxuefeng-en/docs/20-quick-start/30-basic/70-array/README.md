<!-- TRANSLATED by md-translate -->
# Array types

If we have a set of variables of the same type, for example, the grades of 5 students, we can write it like this:

```java
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int n1 = 68;
        int n2 = 79;
        int n3 = 91;
        int n4 = 85;
        int n5 = 62;
    }
}
```

But there is no need to define 5 `int` variables. You can use an array to represent a "set" of `int` types. The code is as follows:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        ns[0] = 68;
        ns[1] = 79;
        ns[2] = 91;
        ns[3] = 85;
        ns[4] = 62;
    }
}
```

To define a variable of type array, use the array type `type[]', e.g., `int[]`. Unlike a single basic type variable, an array variable must be initialized using `new int[5]` to indicate the creation of an array that can hold five `int` elements.

Java's arrays have several features:

* All elements of an array are initialized to their default values, which are `0` for integers, `0.0` for floats, and `false` for booleans;
* The size of an array is immutable once it has been created.

To access an element in an array, you need to use an index. Array indexes start at `0`, for example, an array of 5 elements has an index range of `0` to `4`.

It is possible to modify an element of an array, using an assignment statement, e.g., `ns[1] = 79;`.

You can use `array variable.length` to get the array size:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        System.out.println(ns.length); // 5
    }
}
```

Arrays are reference types, and when using an index to access an array element, the runtime will report an error if the index is out of range:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[5];
        int n = 5;
        System.out.println(ns[n]); // 索引n不能超出范围
    }
}
```

It is also possible to specify the initialized elements directly when defining the array, so that you don't have to write out the array size, but rather the compiler automatically imputes the array size. Example:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[] { 68, 79, 91, 85, 62 };
        System.out.println(ns.length); // 编译器自动推算数组大小为5
    }
}
```

It can be further abbreviated as:

```java
int[] ns = { 68, 79, 91, 85, 62 };
```

Note that arrays are of reference type and the size of the array is immutable. Let's observe the following code:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns;
        ns = new int[] { 68, 79, 91, 85, 62 };
        System.out.println(ns.length); // 5
        ns = new int[] { 1, 2, 3 };
        System.out.println(ns.length); // 3
    }
}
```

Did the array size change? It looks like it changed, but it didn't really change at all.

For the array `ns`, the execution `ns = new int[] { 68, 79, 91, 85, 62 };` points to a 5-element array:

```ascii
ns
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │
└───┴───┴───┴───┴───┴───┴───┘
```

When executing `ns = new int[] { 1, 2, 3 };` it points to a _new_ 3-element array:

```ascii
ns ──────────────────────┐
                              │
                              ▼
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │ 1 │ 2 │ 3 │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

However, the original 5-element array has not changed, it's just that you can't refer to them through the variable `ns`.

### Array of strings

If the array element is not a basic type, but a reference type, what would be the difference in modifying the array element?

Strings are reference types, so we start by defining an array of strings:

```java
String[] names = {
    "ABC", "XYZ", "zoo"
};
```

For the array variable `names` of type `String[]`, it actually contains 3 elements, but each element points to some string object:

```ascii
┌─────────────────────────┐
    names │   ┌─────────────────────┼───────────┐
      │   │   │                     │           │
      ▼   │   │                     ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

Assigning a value to `names[1]`, such as `names[1] = "cat";`, has the following effect:

```ascii
┌─────────────────────────────────────────────────┐
    names │   ┌─────────────────────────────────┐           │
      │   │   │                                 │           │
      ▼   │   │                                 ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │ "cat" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

Notice here that the original string `"XYZ"` pointed to by `names[1]` has not changed, only the reference to `names[1]` has been changed from pointing to `"XYZ"` to pointing to `"cat"`, with the result that the string `"XYZ"` can no longer be accessed through `names[1]`.

With a deeper understanding of "pointing", try to explain the following code:

```java
// 数组
public class Main {
    public static void main(String[] args) {
        String[] names = {"ABC", "XYZ", "zoo"};
        String s = names[1];
        names[1] = "cat";
        System.out.println(s); // s是"XYZ"还是"cat"?
    }
}
```

### Summary

An array is a collection of the same data type, and once created, an array is immutable in size;

Array elements can be accessed by index, but index out of range will report an error;

Array elements can be of value type (e.g., `int`) or reference type (e.g., `String`), but the array itself is of reference type;