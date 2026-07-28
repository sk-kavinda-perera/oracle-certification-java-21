 # String

```java
String str = "Java";

// Character: J a v a
// Index:     0 1 2 3
```

* A `String` in Java is a sequence of characters.
* There are two common ways to create a `String` object:

### 1. String Literal

```java
String s1 = "Java";
String s2 = "Programming";
```

### 2. Using the `new` Keyword

```java
String s3 = new String("Java");
String s4 = new String("Programming");
```

* The `String` class is in the `java.lang` package.
* The `java.lang` package is imported automatically.

## String Concatenation

```java
// Example 01
String stringNumber1 = "3";
String stringNumber2 = "4";

System.out.println(stringNumber1 + stringNumber2); // 34
```

```java
// Example 02
int num1 = 5;
int num2 = 5;

System.out.println(num1 + num2);          // 10
System.out.println(stringNumber1 + num1); // 35
```

* When the `+` operator is used with a `String`, it performs concatenation.

```java
// Example 03
System.out.println(4 + 5 + "6"); // 96
System.out.println(4 + "5" + 6); // 456
System.out.println("4" + 5 + 6); // 456
```

* Expressions are evaluated from left to right.
* In `4 + 5 + "6"`, `4 + 5` is calculated first, producing `9`. Then, `9 + "6"` produces `"96"`.

## Equality of Strings

* When a string literal is created, the Java Virtual Machine first checks the **string pool**, which is located in heap memory.
* If the same string already exists in the pool, a reference to the existing object is returned.
* If it does not exist, a new string object is created and placed in the string pool.
* String literals are memory-efficient because identical literals can share the same object.

### String Literals

```java
String s1 = "Java";
String s2 = "Java";    // Refers to the same pooled object as s1
String s3 = "Android";
```

### String Objects

```java
String s4 = new String("Hello");
```

* The literal `"Hello"` is stored in the string pool.
* The `new` keyword creates a separate `String` object in heap memory.
* `s4` refers to the newly created object, not directly to the pooled object.

```java
String s1 = new String("Hello Developers");
String s2 = new String("hello Developer");
String s3 = "Hello Developers";
String s4 = "Hello Developers";
String s5 = "Hello " + "Developers";
```

* `"Hello " + "Developers"` is a compile-time constant, so Java combines it and uses the pooled `"Hello Developers"` object.

### Using the `==` Operator

* The `==` operator checks whether two references point to the same object.

```java
System.out.println(s1 == s2); // false
System.out.println(s1 == s3); // false
System.out.println(s1 == s4); // false
System.out.println(s1 == s5); // false

System.out.println(s2 == s3); // false
System.out.println(s2 == s4); // false
System.out.println(s2 == s5); // false

System.out.println(s3 == s4); // true
System.out.println(s3 == s5); // true
System.out.println(s4 == s5); // true
```

### Identity Hash Code

```java
System.out.println(System.identityHashCode(s1));
```

* `System.identityHashCode()` prints the identity hash code of an object.
* It does not print the actual memory address or reference ID.

### Using the `.equals()` Method

* The `.equals()` method compares the contents of two strings.

```java
System.out.println(s1.equals(s2)); // false
System.out.println(s1.equals(s3)); // true
System.out.println(s1.equals(s4)); // true
System.out.println(s1.equals(s5)); // true

System.out.println(s2.equals(s3)); // false
System.out.println(s2.equals(s4)); // false
System.out.println(s2.equals(s5)); // false

System.out.println(s3.equals(s4)); // true
System.out.println(s3.equals(s5)); // true
System.out.println(s4.equals(s5)); // true
```

* Java is case-sensitive.
* The value of `s2` is `"hello Developer"`, which is different from `"Hello Developers"`.

```text
.equals() → Checks the value or content
==        → Checks the object reference
```

## Strings with Wrapper Classes

```java
Integer age = new Integer(20); // Deprecated; use autoboxing instead

Integer age1 = 20;
int age2 = 20;

System.out.println(age1 == age2);      // true
System.out.println(age1.equals(age2)); // true
```

* Java does not have a `===` operator.
* In `age1 == age2`, Java automatically unboxes `age1` from `Integer` to `int`.
* It then compares the primitive values `20 == 20`, which returns `true`.
* In `age1.equals(age2)`, Java automatically boxes `age2` as an `Integer` before comparing the values.

## Arrays

```java
String[] animals1 = new String[]{"Dog", "Cat", "Cow"};
String[] animals2 = {"Dog", "Cat", "Cow"};

String[] animals3 = new String[3];
animals3[0] = "Dog";
animals3[1] = "Cat";
animals3[2] = "Cow";

String[] animals4 = animals1;
```

### Comparing Array References

```java
System.out.println(animals1 == animals2); // false
System.out.println(animals1 == animals3); // false
System.out.println(animals1 == animals4); // true

System.out.println(animals2 == animals3); // false
System.out.println(animals2 == animals4); // false
System.out.println(animals3 == animals4); // false
```

* `animals1` and `animals4` refer to the same array object.

### Using `.equals()` with Arrays

```java
System.out.println(animals1.equals(animals2)); // false
System.out.println(animals1.equals(animals3)); // false
System.out.println(animals1.equals(animals4)); // true

System.out.println(animals2.equals(animals3)); // false
System.out.println(animals2.equals(animals4)); // false
System.out.println(animals3.equals(animals4)); // false
```

* Arrays do not override the `equals()` method from the `Object` class.
* Therefore, calling `.equals()` on an array works like `==` and compares object references rather than array contents.
* This applies to arrays, not to `ArrayList`.

### Comparing Array Contents

Use the `Arrays.equals()` method to compare the contents of arrays:

```java
import java.util.Arrays;

System.out.println(Arrays.equals(animals1, animals2)); // true
System.out.println(Arrays.equals(animals1, animals3)); // true
System.out.println(Arrays.equals(animals1, animals4)); // true
System.out.println(Arrays.equals(animals2, animals3)); // true
System.out.println(Arrays.equals(animals2, animals4)); // true
System.out.println(Arrays.equals(animals3, animals4)); // true
```

* `Arrays.equals()` compares the corresponding elements of each array.

## Immutability of Strings

* `String` objects are immutable.
* Once a `String` object is created, its data or state cannot be changed.
* String methods may create and return a new object.
* The reference variable can be changed to refer to another object.

```java
// Example 01
String message = "Hello";

message.concat(" World");

System.out.println(message); // Hello
```

* `concat()` creates and returns a new `String`.
* The `message` reference is not changed because the returned value was not assigned.

```java
// Example 02
int age = 20;
age++;

System.out.println(age); // 21
```

* A primitive variable can be reassigned.
* Primitives are not objects, so describing them as mutable objects is not precise.

```java
String message = "Hello";

message.concat(" World");
System.out.println(message); // Hello

message = message.concat(" World");
System.out.println(message); // Hello World
```

```java
// Example 03
String s = "android";

s.toUpperCase();
System.out.println(s); // android

String s2 = s.toUpperCase();
System.out.println(s2); // ANDROID
```

## Useful Methods of the String Class — Part 1

| Method                                    | Description                                                                                |
| ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| `toUpperCase()`                           | Returns a string in uppercase.                                                             |
| `toLowerCase()`                           | Returns a string in lowercase.                                                             |
| `concat(String s)`                        | Adds the given string to the end of the current string.                                    |
| `isEmpty()`                               | Returns `true` when the string length is `0`; otherwise, returns `false`.                  |
| `equals(Object another)`                  | Compares the contents of two strings.                                                      |
| `substring(int beginIndex)`               | Returns a substring from `beginIndex` to the end of the string.                            |
| `substring(int beginIndex, int endIndex)` | Returns a substring from `beginIndex` inclusive to `endIndex` exclusive.                   |
| `length()`                                | Returns the number of characters in the string.                                            |
| `charAt(int index)`                       | Returns the character at the specified index.                                              |
| `indexOf(String s)`                       | Returns the index of the first occurrence of the given string, or `-1` if it is not found. |
| `equalsIgnoreCase(String s)`              | Compares two strings without considering uppercase and lowercase differences.              |
| `startsWith(String s)`                    | Returns `true` if the string starts with the given string.                                 |
| `endsWith(String s)`                      | Returns `true` if the string ends with the given string.                                   |
| `contains(String s)`                      | Returns `true` if the string contains the given character sequence.                        |
| `replace()`                               | Replaces characters or character sequences in a string.                                    |
| `split(String regex)`                     | Splits a string and returns an array.                                                      |
| `trim()`                                  | Removes leading and trailing whitespace.                                                   |

```java
// Example 01
String str1 = "Hello Java Developer";
String str2 = "Java is fun";
String[] str3;

System.out.println(str1.length());
System.out.println(str1.charAt(0));
System.out.println(str1.isEmpty());
System.out.println(str1.substring(6));
System.out.println(str1.equals(str2));
System.out.println(str1.concat(str2));
System.out.println(str1.toLowerCase());
System.out.println(str1.toUpperCase());

System.out.println(str1.replace('i', 'L'));
// No change because str1 does not contain the character 'i'

str3 = str1.split(" ");

System.out.println(Arrays.toString(str3));
// [Hello, Java, Developer]
```

### Character Indexes

* Spaces are also considered characters and have indexes.

```text
Character: H e l l o   J a v a
Index:     0 1 2 3 4 5 6 7 8 9
```

```java
String st1 = "Hello Java";

System.out.println(st1.charAt(0)); // H
System.out.println(st1.charAt(25));
// StringIndexOutOfBoundsException at runtime

System.out.println(st1.charAt(st1.length() - 1)); // a
```

### `indexOf()` and `substring()`

```java
String s1 = "Java Developers";

System.out.println(s1.indexOf('p'));      // 11
System.out.println(s1.indexOf('v', 5));   // 7
System.out.println(s1.indexOf("lop"));    // 9
System.out.println(s1.indexOf("lop", 5)); // 9
System.out.println(s1.indexOf("lop", 12)); // -1
System.out.println(s1.indexOf("lop", 25)); // -1
```

* The second argument of `indexOf()` specifies the index from which the search should begin.
* When the value is not found, `indexOf()` returns `-1`.

```java
System.out.println(s1.substring(5));     // Developers
System.out.println(s1.substring(5, 12)); // Develop
System.out.println(s1.substring(12));    // ers
```

* The beginning index is included.
* The ending index is excluded.
* The length of a substring is calculated as:

```text
endIndex - beginIndex
```

### `startsWith()`, `endsWith()`, `contains()`, `replace()`, and `trim()`

```java
String s2 = "Hello";

System.out.println(s2.startsWith("H")); // true
System.out.println(s2.startsWith("h")); // false
System.out.println(s2.startsWith("h".toUpperCase())); // true
System.out.println(s2.startsWith("Hel")); // true
```

* `endsWith()` works in a similar way.

```java
String s1 = "Java Developers";

System.out.println(s1.contains("op")); // true
System.out.println(s1.contains("J".toLowerCase())); // false

System.out.println(s1.replace(" ", "-"));
// Java-Developers

System.out.println("Hello Java World ".trim());
// Hello Java World
```

## StringBuffer Class

* `StringBuffer` is used to create mutable or modifiable character sequences.
* Its length and content can be changed through method calls.
* A `StringBuffer` object is stored in heap memory.

```java
StringBuffer strBuffer = new StringBuffer("Java");

strBuffer.append(" Program");

System.out.println(strBuffer); // Java Program
```

* Unlike `String`, the result does not need to be reassigned because the existing `StringBuffer` object is modified.

```java
// Example 01
StringBuffer buffer = new StringBuffer("Welcome to");

buffer.append(" Java");

System.out.println(buffer);          // Welcome to Java
System.out.println(buffer.length()); // 15

buffer.insert(buffer.length(), " World");

System.out.println(buffer);
// Welcome to Java World

System.out.println(buffer.reverse());
// dlroW avaJ ot emocleW
```

* The `String` class does not have a `reverse()` method.
* `StringBuffer` and `StringBuilder` have a built-in `reverse()` method.

### Does `String` Have `insert()` and `delete()` Methods?

* No. The `String` class does not have `insert()` or `delete()` methods because strings are immutable.
* `StringBuffer` and `StringBuilder` provide both `insert()` and `delete()` methods.

```java
buffer.reverse();

System.out.println(buffer);
// Welcome to Java World

buffer.delete(0, 11);

System.out.println(buffer);
// Java World
```

* `delete(int start, int end)` deletes characters from `start` inclusive to `end` exclusive.

## StringBuilder Class

* `StringBuilder` is similar to `StringBuffer`.
* Its objects are stored in heap memory and can be modified.
* `StringBuilder` provides most of the same methods as `StringBuffer`.
* The methods in `StringBuffer` are synchronized, making it thread-safe.
* The methods in `StringBuilder` are not synchronized.
* `StringBuilder` is generally faster than `StringBuffer`.

```java
StringBuilder strBuilder = new StringBuilder("Java");

strBuilder.append(" Programming");

System.out.println(strBuilder);
// Java Programming
```

## String, StringBuffer, and StringBuilder Comparison

| Feature                | `String`                                                        | `StringBuffer`                        | `StringBuilder`                      |
| ---------------------- | --------------------------------------------------------------- | ------------------------------------- | ------------------------------------ |
| Storage                | Literals use the string pool; objects are stored in heap memory | Heap memory                           | Heap memory                          |
| Modifiable             | No — immutable                                                  | Yes — mutable                         | Yes — mutable                        |
| Thread-safe            | Safe to share because it is immutable                           | Yes — methods are synchronized        | No                                   |
| Repeated modifications | Inefficient because new objects may be created                  | Slower than `StringBuilder`           | Usually the fastest                  |
| Recommended use        | Text that does not need frequent changes                        | Mutable text used by multiple threads | Mutable text used by a single thread |

* Use `StringBuffer` when the same mutable character sequence is shared across multiple threads.
* Use `StringBuilder` for single-threaded string modifications because it generally provides better performance.
