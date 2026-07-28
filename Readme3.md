# OOP Concepts, Objects and Classes

## Object-Oriented Programming (OOP)

- OOP is a method of designing and implementing software using **objects and classes**.
- OOP improves:
    - Flexibility
    - Maintainability
    - Reusability

## Main OOP Concepts

- Class
- Object
- Inheritance
- Polymorphism
- Abstraction
- Encapsulation

---

# Objects and Classes

## Object

- A Java object is a **self-contained component** that contains:
    - Properties (data)
    - Methods (behaviour)

- Objects are created in memory and occupy memory space.
- A class is a **blueprint/template** used to create objects.

Example:

```java
Car myCar = new Car();
```

- `new Car()` → Creates a new object and calls the constructor.
- `myCar` → Reference variable that refers to the created object.

---

# Characteristics of Objects

## 1. State

- Represents the data of an object (properties/variables).

Example:
```java
color = "Red";
speed = 100;
```

## 2. Behaviour

- Represents the actions an object can perform (methods).

Example:
```java
start();
stop();
```

## 3. Identity

- Used internally by JVM to uniquely identify each object.
- The identity value is not visible to the user.

---

# Creating Objects

- Objects are created using the `new` keyword.

Example:

```java
Car myCar = new Car();
```

- `new Car()` → Allocates memory and calls the constructor.
- `myCar` → Stores the reference of the object.

---

### Java Application and Classes

- Every Java application must have at least one class.
- A standalone Java application requires a class containing the `main()` method.

# Stack Memory vs Heap Memory
 - Stack Memory - It is an area of memory used at program run time. This is materializers as a pshyical space in ram.

 - Heap Memory - It is an area of memory that is allocated dynamically at program runtime.
 
 
## Stack Memory

- Stores:
    - Local variables
    - Primitive data types
    - Reference variables
    - Method calls (stack frames)

- Stack memory is limited.
- Follows **Last In First Out (LIFO)** order.
- Each thread has its own separate stack memory.
- Each method call creates a new stack frame to store:
    - Local variables
    - Primitive values
    - Object references

- Stack memory exists until the method execution is completed.
- If stack memory is exhausted, JVM throws:

```
java.lang.StackOverflowError
```

Example:

```java
public void test(){
    test();
}
```

- Infinite method calls keep adding stack frames until the stack is full.

---

## Heap Memory

- Stores:
    - Objects
    - Instance variables
    - Arrays

- Objects are created in heap memory.
- Heap memory does not follow a specific order.
- It is shared among all threads.

Example:

```java
Cat myPet = new Cat();
```

Memory allocation:

```
Stack:
myPet → reference

Heap:
Cat object
```

- The reference variable is stored in stack (if it is a local variable).
- The actual object is stored in heap.

---

## Garbage Collection

- Java provides automatic memory management using the **Garbage Collector (GC)**.
- The garbage collector removes objects that are no longer reachable.

Heap memory remains in use until:
- Program termination
- Garbage collection removes unused objects

If heap memory is exhausted, JVM throws:

```
java.lang.OutOfMemoryError
```

---

# Stack vs Heap

| Stack Memory | Heap Memory |
|---|---|
| Stores local variables, primitives, and references | Stores objects and instance data |
| Each thread has its own stack | Shared among all threads |
| Follows LIFO order | No specific order |
| Smaller and limited | Larger memory space |
| Faster access | Managed by Garbage Collector |
| StackOverflowError occurs when full | OutOfMemoryError occurs when full |

---

# Example

```java
public class Main {

    public static void main(String[] args){

        Cat myPet = new Cat();

    }
}

class Dog {

    double height;
    double weight;
}

class Cat {

    Dog dog = new Dog();
    String color;
}
```

## Memory Allocation

### Stack

```
myPet → reference to Cat object
```

### Heap

```
Cat object
    |
    |-- dog reference
    |        |
    |        v
    |     Dog object
    |        |
    |        |-- height = 0
    |        |-- weight = 0
    |
    |-- color = null
```

Explanation:

- `myPet` is a local reference variable, so it is stored in the stack.
- `Cat` object is stored in the heap.
- `dog` is an instance variable inside the `Cat` object, so the reference is also stored in the heap.
- The `Dog` object is also created in the heap.

---

# When to Use Stack or Heap?

- Use stack for:
    - Small local data
    - Primitive values
    - Temporary variables inside methods

- Use heap for:
    - Objects
    - Large data
    - Data whose size is determined at runtime

```
Stack → stores variables and references

Heap → stores objects and instance data

Heap Memory -> manual memory managmeent is not needed. since we have garbage collector.
```

# Stack and Heap Memory Example

```java
public class Main {

    public static void main(String[] args){

        int age = 25;                 // Primitive data type (Stack)

        Cat myPet = new Cat();        // Reference variable (Stack)
                                      // Cat object created in Heap

        int[] numbers = {1,2,3};      // Array reference (Stack)
                                      // Array object created in Heap

        myPet.color = "Black";

        printDetails(age);
    }

    public static void printDetails(int value){

        int marks = 90;               // Primitive local variable (Stack)

    }
}

class Cat {

    String color;                     // Instance variable (Heap)
    int age;                          // Instance variable (Heap)

}
```

## Stack Memory

Stores:
- Primitive values
- Local variables
- Method calls (Stack frames)
- References to objects

Example:

```
main()

age = 25                         → Primitive value

myPet → Reference to Cat object

numbers → Reference to Array object


printDetails()

value = 25                       → Primitive value

marks = 90                       → Primitive value
```

- Each method call creates a new stack frame.
- Stack frame is removed when the method execution finishes.

---

## Heap Memory

Stores:
- Objects
- Instance variables
- Arrays
- Reference type objects

Example:

```
Cat object

color = "Black"                  → Instance variable

age = 0                          → Instance variable


Array object

[1, 2, 3]


String object

"Black"
```

---

## Memory Relationship

```
STACK

myPet  --------------------->  Cat object (Heap)

numbers -------------------->  Array object (Heap)


HEAP

Cat object
    |
    |-- color
    |-- age

Array object
    |
    |-- [1,2,3]
```

Summary:

```
Stack → primitive values, method calls, object references

Heap → objects, instance variables, arrays
```

### Access Modifiers

- There are 4 main access modifiers
    - default -> it is visible within the same package
    - public -> it is visible to any part of the project(least restrictive).  sub classes or classes with  other packages can use public access modifiers
    - private -> it is visible to current class only. (the most restrive acces control). no outside access
    - protected -> visible to within the same package and all subclasses from its defined class.


# Java Access Modifiers

| Access Modifier | Same Class | Different Class (Same Package) | Subclass | Different Package |
|---|---|---|---|---|
| `public` | ✅ Accessible | ✅ Accessible | ✅ Accessible | ✅ Accessible |
| `protected` | ✅ Accessible | ✅ Accessible | ✅ Accessible | ⚠️ Accessible only through inheritance |
| `default` (no modifier) | ✅ Accessible | ✅ Accessible | ❌ Not accessible (unless same package) | ❌ Not accessible |
| `private` | ✅ Accessible | ❌ Not accessible | ❌ Not accessible | ❌ Not accessible |

## Access Level Order (Highest → Lowest)

```
public
   ↓
protected
   ↓
default (no modifier)
   ↓
private
```
 
 

# Naming Conventions

## Class Name

- Should start with an **uppercase letter** (Java convention).
- Should be a **noun**.
- Examples:
    - `Car`
    - `System`
    - `Student`

### Class Naming Rules

✅ Valid:
```java
Car
Car2
Car123
Car_Model
_Car
$Car
```

❌ Invalid:
```java
2Car      // Cannot start with a number
```

- Numbers are allowed:
    - In the middle ✅
    - At the end ✅
    - Not at the beginning ❌

Example:

```java
Car2      // Valid
Car123    // Valid
2Car      // Invalid
```

- Starting with uppercase is only a **naming convention**, not a Java rule.

Valid but not recommended:

```java
car
student
```

---

## Method Name

- Should start with a **lowercase letter** (Java convention).
- Usually represents an action (verb).

Examples:

```java
start()
stop()
calculateTotal()
```

### Method Naming Rules

✅ Valid:

```java
start()
start2()
calculate123()
_start()
$calculate()
```

❌ Invalid:

```java
2start()   // Cannot start with a number
```

- Numbers are allowed in the middle and end.
- Methods can technically start with uppercase, but it is not recommended.

Example:

```java
Start()    // Valid but bad convention
```

---

## Variable Name

- Should start with a **lowercase letter**.
- Uses camelCase.

Examples:

```java
firstName
year
studentAge
```

Rules:

- Numbers allowed in middle and end.
- Cannot start with a number.

Example:

```java
age2       // Valid
student1   // Valid
2student   // Invalid
```

---

## General Identifier Rules

Valid starting characters:

```

For both method and class names and variables.
A-Z
a-z
_
$
```

Invalid:

```
Starting with numbers
```

Examples:

```java
_name      // Valid
$price     // Valid
price2     // Valid
2price     // Invalid
```

### Constructor
- Special method that represents when an object of a class is created and usually initializes the properties of the class
- similar to a method in java
- automatically called when an object of a class is created 
- constructor have same name and it may or may not have parameters and it does not have a return type. (Not even void )
- if you do not specify any constructor by default java automaitcally provides a default constructor with no parameters. this default consturctor initalize the object with the default values.
- the consturctor method can be overlaoded too. (you can have multiple constructors with different parameters)

```
public class Person{
    int age;
    person(){
        
    }

    Person(int age){
        this.age = age;
    }
}


```

```
Also Remember this?

Person person = new Person("David", 60);
System.out.println(person) -> oopConcept.Person@30f3991
        - oopConcept-> package name
        - Person -> class name
        - 30f3991 -> Stack trace (hashcode)

you need to add a toString method in order to directly print like this.

example ->
    public String toString(){}
    // you need to add toString for custom objects we create if not it prints the object id. (does not have default)
```

- Also if you have defined any constructor (can be different constructor with some parameters). neverthless if you have specified any contructor then no longer default constructor is added.
- Constructors are **not inherited** by subclasses.

### Packages and Import.
- ```Package``` - it is a group of similar types of classes, interfaces and other sub packages. (like file directory).
- classes with same name can be on different packages which avoid collision.
```
package <top-package-name>.<sub-package-name>;
```
- Rules
    - Should declare package at the begining of the source file.
    - only one package declartion per source file.
    - if not package declared, class belongs to default package
    - package name must be hierarchical and spearated by dots.

- There are two different types of packages
    - Build-in-packages  (Array, scanner class etc)
    - User-Defined-Packages -(ones we create)

### Import
- we can use import keyword to import built-in and user-defined packages into our java source files.
```
import animals.reptiles.snake; 
import java.io.*; -> make all class accessible inside io package.  However the class and interfaces inside the sub packages will not be accessible.
 
```
- All the class in java.lang package are imported by default.
    - order
        - package
        - import
        - class
- A java source file should have a public class declartion.
- Scanner class is inside java.util.Scanner;


### Note
 
- A Java source file can contain multiple top-level classes.
- Only **one top-level class can be public**, and its name must match the file name.
- Other top-level classes must use **default access (no modifier)**.
- Top-level classes cannot be `private` or `protected`.
- Nested classes can use all access modifiers: `public`, `protected`, `private`, and `default`  
- Top-level classes cannot be `static`.
- Nested classes can be `static` (called static nested classes).

### Static Keyword
- Static keyword is used for memeory management in java. 
- It is used for sharing the same variable or method of a given class

    - static variable - > static String schoolName;
    - static method -> static void start();
    - static block -> static { }
    - static nested class- > 
        ```
        class A{
         static class B{} 
         }

- satic variables can be accessed directly by the class name and do not need any objects. (same with static method).
- static block is a block of statment inside a java class that will be executed when a class is first loaded into java virtual machine.

```
public class Multiplication{
    public static int multiply(int a , int b){
        return a*b;
    }
}

//to invoke - > Multiplication.multiply(5,4); // no objects needed

```

#### Note
- Static methods cannot reference instance variables example ->
    ```diff
    class A {
        int x = 10;
        static void show(){
            System.out.println(x); // compilation error . reference instance variables inside static block.
        }
    }



    //valid scenario
    class A{
        int x =10;
        Static void show(){
            A obj = new obj();
            System.out.println(obj.x);
        }
    }
    ```
- For instance block the static is allowed.

### Static Import
- you can use relevant methods or variables directly without using class name.

example:
```
public class Employee{
    static void showSalary(){
        //method body
    }
}

import static test.Employee.showSalary;
class MainTest{
    public static void main(String[] args){
        showSalary(); // directly use the static method
    }
}

```

- import static test.Employee.showSalary; -> showSalary. it can be any static method or vairable.
-  import static test.Employee.*; -> imports all static variables/ methods.

### Nested Class

```
public class OuterClass{
    //body of outer class

    class NestedClass{
        //body of the nested class
    }
}
```
- when you create a class with another class you get nested class.
- a nested class can be declared by private, public, protected and default and also nested class can be static.

# Nested Classes

```text
Nested Classes
│
├── Static Nested Class
│
└── Inner Class (Non-static Nested Class)
    │
    ├── Member Inner Class
    │
    ├── Local Class
    │
    └── Anonymous Class
```

- static nested classes cannot access non-static memebers of the outer class. only can access static memebers of the outer class.
- non static inner class may access all memebers such as static and non--static variables as well as methods of its outer class.
-  Inner class may access private variables and methods of its outer class.

# Static Nested Class Example

```java
public class Person {

    static String person1 = "John";
    private static String person2 = "David";
    public String person3 = "Andy";

    static class StaticPerson {

        void show(){

            System.out.println(person1); 
            // + Allowed: static nested class can access static members

            System.out.println(person2); 
            // + Allowed: static nested class can access private static members

            System.out.println(person3); 
            // - Not allowed: person3 is non-static and needs an object of Person
        }
    }
}
```

## Accessing Static Nested Class from Another Class

```java
public class StaticNestedClassTest {

    public static void main(String[] args){

        // Person.StaticPerson.show();
        // - Not allowed because show() is not static

        // If show() was static:
        // Person.StaticPerson.show();
        // + Allowed

        
        // Option 1: Create object of static nested class

        Person.StaticPerson person = new Person.StaticPerson();

        person.show();


        // Option 2: Import static nested class

        // import packageName.Person.StaticPerson;

        StaticPerson p1 = new StaticPerson();

        p1.show();
    }
}
```
## Remember

```diff
+ Static nested class can access:
    + static members of outer class
    + private static members of outer class

- Static nested class cannot directly access:
    - non-static instance variables
    - non-static methods
```

```diff
+ To call a static nested class method:
    + If method is static:
        Person.StaticPerson.show();

    + If method is non-static:
        Person.StaticPerson obj = new Person.StaticPerson();
        obj.show();
```
 
## Inner Class (Non-static Nested Class) Example

```java
public class User {

    static String person1 = "John";
    private static String person2 = "David";
    public String person3 = "Andy";

    class InnerUser {

        void show(){

            System.out.println(person1);
            // + Allowed: Inner class can access static members

            System.out.println(person2);
            // + Allowed: Inner class can access private members of outer class

            System.out.println(person3);
            // + Allowed: Inner class can access non-static members of outer class
        }
    }
}
```

## Accessing Inner Class from Another Class

```java
public class InnerClassTest {

    public static void main(String[] args){

        // Method 1:

        User outer = new User();

        User.InnerUser inner = outer.new InnerUser();

        inner.show();


        // Method 2:

        // import packageName.User.InnerUser;

        User outer = new User();

        InnerUser inner = outer.new InnerUser();

        inner.show();
    }
}
```

## Remember

```diff
+ Non-static Inner Class can access:
    + Static members of outer class
    + Private members of outer class
    + Non-static members of outer class

- Inner class object requires:
    - An object of the outer class
```

```diff
+ Creating Inner Class object:

    User outer = new User();

    User.InnerUser inner = outer.new InnerUser();
 

``` 
# Local Inner Classes

- A local class is an inner class defined inside a block (usually inside a method).
- Local classes are not members of the enclosing class; they belong to the block where they are defined.
- Local classes cannot have access modifiers (`public`, `private`, `protected`, `default`).
- Local classes can be `final` or `abstract`.
- A local class must be instantiated inside the block where it is defined.

## Main Points

- Scope of a local class is limited to the block where it is created.
- A local class cannot be accessed or instantiated outside its block.
- A local class can access:
    - Members of the enclosing class.
    - Method parameters of the enclosing method.
    - Local variables that are `final` or **effectively final**.

- Local classes are non-static.
- If a local class is declared inside a method, it can access that method's parameters.

---

## Example

```java
public class LocalInnerClass {

    public static void main(String[] args){

        checkNumbers(10);

    }

    public static void checkNumbers(int enteredNumber){

        int result = 10; // effectively final

        class NumberChecker {

            boolean check;

            public NumberChecker(int number){

                check = number % 2 == 0;

                System.out.println(result);
                // Accessible because result is effectively final
            }

            public void printNumber(){

                System.out.println("You entered " + enteredNumber);
                // Method parameter can be accessed
            }
        }

        NumberChecker checker = new NumberChecker(enteredNumber);

        checker.printNumber();
    }
}
```

# Local Class Scope and Access

```java
public class Outer {

    private String name = "Kavi";   // Enclosing class member

    public void check(int number) { // Method parameter

        int value = 100;            // Effectively final local variable

        class LocalClass {

            public void display() {

                System.out.println(name);
                // ✅ Can access enclosing class member

                System.out.println(number);
                // ✅ Can access method parameter

                System.out.println(value);
                // ✅ Can access final/effectively final local variable
            }
        }

        LocalClass obj = new LocalClass();

        obj.display();
    }

    public static void main(String[] args) {

        Outer outer = new Outer();

        outer.check(50);
    }
}
```

Output:

```
Kavi
50
100
```

---

## Important Points

### 1. Scope of Local Class

- Scope of a local class is limited to the block where it is created.

Example:

```java
public void check(){

    class LocalClass {

    }

}
```

`LocalClass` can only be used inside `check()`.

Outside the method:

```java
LocalClass obj = new LocalClass(); // ❌ Error
```

---

### 2. Access Members of Enclosing Class

```java
private String name = "Kavi";
```

A local class can access members of its enclosing class:

```java
System.out.println(name); // ✅ Allowed
```

---

### 3. Access Method Parameters

```java
public void check(int number)
```

The local class can access method parameters:

```java
System.out.println(number); // ✅ Allowed
```

---

### 4. Access Local Variables

A local class can access local variables only if they are:

- `final`
- Effectively final (value is not changed)

Example:

```java
int value = 100;

class LocalClass {

    void display(){

        System.out.println(value); // ✅ Allowed

    }
}
```

But:

```java
int value = 100;

value = 200;

class LocalClass {

    void display(){

        System.out.println(value); // ❌ Error

    }
}
```

Because `value` is no longer effectively final.

---

### 5. Local Classes are Non-static

A local class cannot be declared as `static`.

```java
static class LocalClass { } // ❌ Not allowed
```

---

## Summary

```
Local Class:
- Defined inside a method/block
- Scope limited to that block
- Cannot be accessed outside the block
- Can access enclosing class members
- Can access method parameters
- Can access final/effectively final local variables
- Cannot be static
```
 






## Important

```diff
+ Local class can access:
    + Enclosing class members
    + Method parameters
    + Final/effectively final local variables

- Local class cannot:
    - Be accessed outside the block
    - Have public/private/protected modifiers
    - Access non-final local variables
```