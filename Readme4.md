# Inheritance in Java 

Inheritance allows one class to reuse the fields and methods of another class.

It represents an **"is-a" relationship**.

```java  
class MountainBike extends Bicycle {
}
``` 
Inheritance supports code reusability.

## Terminology 
- **Subclass:** The class that inherits from another class aka derived class, extended class or child class.
- **Superclass:** The class being inherited from aka base class, parent class.
 

## Inheritance Rules 
- A Java class can have only one direct superclass.
- A superclass can have multiple subclasses unless it is declared `final`.
- The `Object` class has no superclass.
- Every other class directly or indirectly extends `Object`.
- If no superclass is written, Java treats `Object` as the superclass.

```java
class Bicycle {
}
```

This is effectively:

```java
class Bicycle extends Object {
}
```

## Inherited Members

A subclass can inherit accessible fields and methods from its superclass.

| Access modifier | Same package | Different package |
|---|---:|---:|
| `public` | Yes | Yes |
| `protected` | Yes | Yes |
| Package-private (`default`) | Yes | No |
| `private` | No | No |

Private members cannot be accessed directly by a subclass.

```java
// use getters or setters
class Parent {
    private int value = 10;

    public int getValue() {
        return value;
    }
}

class Child extends Parent {
    void display() {
        System.out.println(getValue());
    }
}
```

## Constructors 
Constructors are not inherited. 
A subclass can call a superclass constructor using `super(...)`.

```java
class Bicycle {
    Bicycle(int speed) {
        System.out.println(speed);
    }
}

class MountainBike extends Bicycle {
    MountainBike(int speed) {
        super(speed);
    }
}
```

If `super(...)` is not written, Java automatically tries to call:

```java
super();
//if there is no -no arg constructor in the parent class it will throw an error.
```

## Inherited Fields and Methods

Inherited fields and methods can be used directly inside the subclass.

```java
class Bicycle {
    protected int speed = 10;

    void move() {
        System.out.println("Moving");
    }
}

class MountainBike extends Bicycle {
    void display() {
        System.out.println(speed);
        move();
    }
}
```

A subclass can also declare new fields and methods.

```java
class MountainBike extends Bicycle {
    int suspensionLevel;

    void changeGear() {
        System.out.println("Gear changed");
    }
}
```

## Field Hiding

A subclass can declare a field with the same name as a superclass field.

```java
class Parent {
    int value = 10;
}

class Child extends Parent {
    int value = 20;

    void display() {
        System.out.println(value);       // 20
        System.out.println(super.value); // 10
    }
}
```

This is called **field hiding**.

 
## Static Method Hiding vs Method Overriding

A subclass cannot hide a normal instance method.

- **Static methods are hidden.**
- **Instance methods are overridden.**
- **Fields are hidden.**

### Example

```java
class Parent {
    static void staticMethod() {
        System.out.println("Parent static method");
    }

    void normalMethod() {
        System.out.println("Parent instance method");
    }
}

class Child extends Parent {
    static void staticMethod() {
        System.out.println("Child static method");
    }

    @Override
    void normalMethod() {
        System.out.println("Child instance method");
    }
}
```

```java
Parent obj = new Child();

obj.staticMethod(); // Parent static method
obj.normalMethod(); // Child instance method
```

### Static Method Hiding

Static methods belong to the class rather than to an individual object.

Therefore, the static method is selected using the **reference type**.

```java
Parent obj = new Child();

obj.staticMethod(); // Parent static method
```

Because the reference type is `Parent`, Java uses `Parent.staticMethod()`.

It is better to call static methods using the class name:

```java
Parent.staticMethod(); // Parent static method
Child.staticMethod();  // Child static method
```

### Method Overriding

Instance methods belong to objects.

Therefore, an overridden instance method is selected using the **actual object type** at runtime.

```java
Parent obj = new Child();

obj.normalMethod(); // Child instance method
```

Although the reference type is `Parent`, the actual object is a `Child`, so Java calls `Child.normalMethod()`.

### Private Methods

Private methods are not inherited by subclasses.

Therefore, when a subclass declares a method with the same signature as a private method in the superclass, it is neither hiding nor overriding it.

```java
class Parent {
    private void test() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void test() {
        System.out.println("Child");
    }
}
```

In this example, the two `test()` methods are separate methods.

### Summary

| Member | Subclass declares the same name/signature | Result |
|---|---|---|
| Static method | Yes | Method hiding |
| Instance method | Yes | Method overriding |
| Field | Yes | Field hiding |
| Private method | Yes | Separate method because it is not inherited |

 
## Nested Classes and Private Members

A nested class can access the private fields and methods of its enclosing class.

```java
class Parent {
    private int value = 10;

    protected class Helper {
        void showValue() {
            System.out.println(value);
        }
    }
}
```

Here, `Helper` is declared inside `Parent`, so it can access the private field `value`.

A subclass cannot access `value` directly:

```java
class Child extends Parent {
    void display() {
        // System.out.println(value); // Error
    }
}
```

However, because `Helper` is `protected`, the subclass can use it:

```java
class Child extends Parent {
    void display() {
        Helper helper = new Helper();
        helper.showValue(); // Prints 10
    }
}
```

The access happens indirectly:

```text
Child → Helper → Parent's private field
```

The `Child` class still does not inherit or directly access `value`. It only uses the inherited `Helper` class, whose code is allowed to access `Parent`'s private members.

### Important

- A `public` or `protected` nested class can be accessible to a subclass.
- A `private` nested class cannot be accessed by a subclass.
- The subclass does not automatically gain access to the superclass's private members.
- The nested class must provide a method that accesses those private members.
 
 
## Subclass Constructors

Every subclass constructor must eventually call a constructor of its direct superclass.

### Calling the Parent Constructor

Use `super(...)` to call a parent constructor.

```java
class Parent {
    Parent(int value) {
        System.out.println(value);
    }
}

class Child extends Parent {
    Child() {
        super(10);
    }
}
```

### Calling Another Child Constructor

A constructor can call another constructor in the same class using `this(...)`.

```java
class Child extends Parent {
    Child() {
        this(10);
    }

    Child(int value) {
        super(value);
    }
}
```

The constructor chain is:

```text
Child() → Child(int) → Parent(int)
```

The constructor chain must eventually call a parent constructor.

### Implicit `super()`

If neither `this(...)` nor `super(...)` is written, Java automatically attempts to call:

```java
super();
```

```java
class Parent {
    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    Child() {
        // Java automatically calls super()
        System.out.println("Child");
    }
}
```

Output:

```text
Parent
Child
```

If the parent does not have an accessible no-argument constructor, a compile-time error occurs.

```java
class Parent {
    Parent(int value) {
    }
}

class Child extends Parent {
    Child() {
        // Compile-time error:
        // Java tries to call Parent(), but it does not exist.
    }
}
```

 
### Important Rule

In Java 21, `super(...)` or `this(...)` must be the first statement in a constructor.

A single constructor cannot contain both `this(...)` and `super(...)`.

- `this(...)` calls another constructor in the same class.
- `super(...)` calls a constructor in the superclass.

If a constructor uses `this(...)`, the called constructor must eventually call `super(...)`, either explicitly or implicitly.

```java
class Parent {
    Parent(int value) {
        System.out.println("Parent: " + value);
    }
}

class Child extends Parent {
    Child() {
        this(10);
    }

    Child(int value) {
        super(value);
    }
}
```

The constructor chain is:

```text
Child() → Child(int) → Parent(int)
```

The following is invalid:

```java
Child() {
    this(10);
    super(10); // Compile-time error
}
```

If neither `this(...)` nor `super(...)` is written, the compiler automatically inserts:

```java
super();
``` 

## Types of Inheritance

### Single Inheritance

One class extends one superclass.

```java
class A {
}

class B extends A {
}
```

### Multilevel Inheritance

A class extends another subclass.

```java
class A {
}

class B extends A {
}

class C extends B {
}
```

### Hierarchical Inheritance

Multiple classes extend the same superclass.

```java
class A {
}

class B extends A {
}

class C extends A {
}
```

### Multiple Inheritance

Java does not support multiple inheritance through classes.

```java
// Invalid
class C extends A, B {
}
```

A class can implement multiple interfaces.

```java
interface A {
}

interface B {
}

class C implements A, B {
}
```

### Hybrid Inheritance

Java does not support hybrid inheritance through multiple classes.

However, similar structures can be created using classes and interfaces.

```java
class Vehicle {
}

interface Electric {
}

interface Smart {
}

class SmartCar extends Vehicle implements Electric, Smart {
}
```
 
## Inner Class Reference Type

### Code

```java
class Animal {
    class Leg {
        void info() {
            System.out.println("Animal Leg");
        }
    }
}

class Dog extends Animal {
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();

        Dog.Leg leg = d.new Leg();

        leg.info();
    }
}
```

Output:

```text
Animal Leg
```

## Explanation

### Creating the `Dog` object

```java
Dog d = new Dog();
```

Here:

- `d` is a reference of type `Dog`.
- The object created is also a `Dog`.
- Because `Dog` extends `Animal`, it inherits the accessible inner class `Leg`.

### Creating the `Leg` object

```java
Dog.Leg leg = d.new Leg();
```

This line creates an object of the inner class `Leg`.

It can be divided into two parts:

```java
Dog.Leg leg
```

This declares a reference variable named `leg`.

Its type is `Dog.Leg`, which refers to the `Leg` inner class available through `Dog`.

```java
d.new Leg()
```

This creates a `Leg` object associated with the outer `Dog` object referenced by `d`.

Because `Leg` is a non-static inner class, it requires an outer object.

The general syntax is:

```java
OuterClass.InnerClass reference = outerObject.new InnerClass();
```

In this example:

```java
Dog.Leg leg = d.new Leg();
```

## Can the reference type be `Dog`?

No.

The following is invalid:

```java
Dog leg = d.new Leg(); // Compile-time error
```

This is because:

```java
d.new Leg()
```

creates a `Leg` object, not a `Dog` object.

The reference type must therefore be compatible with `Leg`.

Valid reference types include:

```java
Dog.Leg leg1 = d.new Leg();
Animal.Leg leg2 = d.new Leg();
```

Both are valid because `Leg` was originally declared inside `Animal` and is accessible through `Dog`.

You can also use `var`:

```java
var leg = d.new Leg();
```

Java automatically infers the type as `Animal.Leg`.

Therefore:

```java
Dog.Leg leg = d.new Leg();
```

is correct, but:

```java
Dog leg = d.new Leg();
```

###
Assuming  the inner class is static then you can use this
```
Dog.Leg leg = new Dog.Leg();
    or
Leg leg = new Leg(); -> you need to import it ->import your.package.Animal.Leg;
```


is not correct because a `Leg` object cannot be stored in a `Dog` reference.


- Top level class cannot be private or protected.
    - public class Animal{} // valid
    - class Aniam{} //valid
    - private class Animal{} // invalid
    - protected class Animal{} //invalid
- Inner class can be having public, default,private , protected. The rule only apply to the outter class.
 
# Sealed Classes

Sealed classes and interfaces provide fine-grained control over which classes or interfaces may extend or implement them.

They are useful for:

- Domain modeling
- Improving security of libaries
- Defining a fixed set of permitted subtypes

 
# Sealed Classes

Sealed classes and interfaces provide fine-grained control over which classes or interfaces may extend or implement them.

They are useful for:

- Domain modeling
- Improving library security
- Defining a fixed set of permitted subtypes

The `sealed` modifier must appear before the `class` keyword.

```java
sealed class Animal {
}

```

It may also be combined with an access modifier:

```java
public sealed class Animal {
}

    or
    
sealed public class Animal {
}
```

The following is invalid:

```java
class sealed Animal {
} // Compile-time error
```

## Permitted Subclasses

```java
sealed class Animal permits Dog, Cat, Bird {
}

// Direct subclass of Animal
final class Dog extends Animal {
}

// Direct subclass of Animal
sealed class Cat extends Animal permits PersianCat {
}

// Direct subclass of Animal
non-sealed class Bird extends Animal {
}

// Direct subclass of Cat and indirect subclass of Animal
final class PersianCat extends Cat {
}
``` 
Every direct subclass of a sealed class must be declared as one of the following:

- `final`: cannot be extended
- `sealed`: can only be extended by its permitted subclasses
- `non-sealed`: removes the restriction and can be extended normally

Important:

- The keyword is `permits`, not `permit`.
- `non-sealed` can only be used when extending a sealed class or implementing a sealed interface.
- Permitted subclasses must be in the same module or, in an unnamed module, the same package.
- The `permits` clause may be omitted when all direct subclasses are declared in the same source file. 
  
## Sealed-Class Rules

### 1. Location of Permitted Subclasses

- In a **named module**, the sealed class and its permitted subclasses must be in the same module. They may be in different packages.
- In an **unnamed module** (no `module-info.java`), they must be in the same package.

#### Correct: Unnamed Module, Same Package

```java
package animals;

sealed class Animal permits Dog {
}

final class Dog extends Animal {
}
```

#### Incorrect: Unnamed Module, Different Packages

```java
// package animals
sealed class Animal permits Dog {
}

// package pets
final class Dog extends Animal {
}
```

This causes a compile-time error.

#### Correct: Named Module, Different Packages

```java
// Same named module

package animals;
public sealed class Animal permits pets.Dog {
}
```

```java
package pets;
public final class Dog extends animals.Animal {
}
```

### 2. Omitting `permits`

The `permits` clause may be omitted when all direct subclasses are declared in the same source file.

#### Correct: Same Source File

```java
sealed class Animal {
}

final class Dog extends Animal {
}

final class Cat extends Animal {
}
```

#### Different Source Files

The permitted subclasses must be listed explicitly:

```java
sealed class Animal permits Dog, Cat {
}
```

Omitting `permits` when the subclasses are in different source files causes a compile-time error.

# Method Overriding

Method overriding occurs when a subclass provides a new implementation of an inherited instance method.

The overriding method must have:

- The same method name
- The same parameter list
- The same or a covariant return type
- An access level that is not more restrictive

```java
class Vehicle {
    public void stop() {
        System.out.println("Vehicle stopped");
    }
}

class Car extends Vehicle {
    @Override
    public void stop() {
        System.out.println("Car stopped");
    }
}
```

In this example:

- `Vehicle.stop()` is the **overridden method**.
- `Car.stop()` is the **overriding method**.

## Access Rules

| Superclass method | Allowed access in subclass |
|---|---|
| `public` | `public` |
| `protected` | `protected` or `public` |
| Package-private | Package-private, `protected`, or `public` |
| `private` | Not inherited, so it cannot be overridden |

A subclass cannot reduce the access level of an inherited method.

```java
class Parent {
    protected void display() {
    }
}

class Child extends Parent {
    @Override
    public void display() {
    }
}
```

Additional rules:

- A `final` method cannot be overridden.
- A `static` method is hidden, not overridden.
- A private method is not inherited.
- An overriding method cannot declare broader checked exceptions than the overridden method.
- Using `@Override` is recommended because it allows the compiler to detect mistakes.
 
## Method Overriding Rules

> Changing the parameter list creates a different method.  
> Changing only the return type does not create a different method because the return type is not part of the method signature.

### 1. `final` Instance Method

A `final` method cannot be overridden.

#### Incorrect: Same Signature

```java
class Parent {
    final void display() {
    }
}

class Child extends Parent {
    void display() { // Compile-time error: cannot override a final method
    }
}
```

#### Correct: Different Parameters

```java
class Child extends Parent {
    void display(int value) { // Valid: overloaded method, not overriding
    }
}
```

#### Incorrect: Only Return Type Changed

```java
class Child extends Parent {
    int display() { // Compile-time error: return type alone cannot overload a method
        return 1;
    }
}
```

---

### 2. `static` Method

A static method cannot be overridden. A subclass static method with the same signature **hides** it.

#### Correct: Same Signature

```java
class Parent {
    static void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void display() { // Valid: hides Parent.display()
        System.out.println("Child");
    }
}
```

#### Correct: Different Parameters

```java
class Child extends Parent {
    static void display(int value) { // Valid: overloaded method, not hiding
    }
}
```

#### Incorrect: Changed to an Instance Method

```java
class Child extends Parent {
    void display() { // Compile-time error: instance method cannot override a static method
    }
}
```

#### Incorrect: Only Return Type Changed

```java
class Child extends Parent {
    static int display() { // Compile-time error: incompatible return type
        return 1;
    }
}
```

---

### 3. `static final` Method

A `static final` method cannot be hidden or overridden.

#### Incorrect: Same Signature

```java
class Parent {
    static final void display() {
    }
}

class Child extends Parent {
    static void display() { // Compile-time error: cannot hide a final method
    }
}
```

#### Correct: Different Parameters

```java
class Child extends Parent {
    static void display(int value) { // Valid: overloaded method
    }
}
```

#### Incorrect: Only Return Type Changed

```java
class Child extends Parent {
    static int display() { // Compile-time error
        return 1;
    }
}
```

---

### 4. `private` Method

A private method is not inherited. A subclass method with the same signature is a new, unrelated method.

#### Correct: Same Signature

```java
class Parent {
    private void display() {
    }
}

class Child extends Parent {
    void display() { // Valid: new method, not overriding
    }
}
```

#### Correct: Different Return Type

```java
class Parent {
    private int display() {
        return 1;
    }
}

class Child extends Parent {
    String display() { // Valid: unrelated to Parent.display()
        return "Child";
    }
}
```

#### Incorrect: Using `@Override`

```java
class Child extends Parent {
    @Override
    void display() { // Compile-time error: no inherited method to override
    }
}
```

---

## Checked Exceptions

An overriding method may declare:

- The same checked exception
- A narrower checked exception
- No checked exception
- An overriding method may declare any unchecked exception, even if the superclass method does not declare it.

It cannot declare a broader or unrelated checked exception.

```java
import java.io.IOException;

class Parent {
    void read() throws IOException {
    }
}
```

### Correct

```java
import java.io.FileNotFoundException;

class Child extends Parent {
    @Override
    void read() throws FileNotFoundException {
        // Valid: FileNotFoundException is a subtype of IOException
    }
}
```

```java
class Child extends Parent {
    @Override
    void read() {
        // Valid: no checked exception
    }
}
```

### Incorrect

```java
class Child extends Parent {
    @Override
    void read() throws Exception {
        // Compile-time error: Exception is broader than IOException
    }
}
```

Unchecked exceptions are not restricted:

```java
class Child extends Parent {
    @Override
    void read() throws RuntimeException {
        // Valid: RuntimeException is unchecked
    }
}
```

---

## Covariant Return Type

An overriding method must return:

- The same type, or
- A subtype of the superclass method's return type

```java
class Animal {
}

class Dog extends Animal {
}

class Parent {
    Animal create() {
        return new Animal();
    }
}
```

### Correct: Covariant Return Type

```java
class Child extends Parent {
    @Override
    Dog create() {
        return new Dog(); // Valid: Dog is a subtype of Animal
    }
}
```

### Incorrect: Unrelated Return Type

```java
class Child extends Parent {
    String create() { // Compile-time error: String is not a subtype of Animal
        return "Dog";
    }
}
```

For primitive return types, the type must be exactly the same:

```java
class Parent {
    int getValue() {
        return 1;
    }
}

class Child extends Parent {
    long getValue() { // Compile-time error: int cannot change to long
        return 1;
    }
}
```

## Summary

| Superclass method | Same signature in subclass | Different parameters |
|---|---|---|
| Normal instance method | Overriding | Overloading |
| `final` instance method | Compile-time error | Allowed |
| `static` method | Method hiding | Overloading |
| `static final` method | Compile-time error | Allowed |
| `private` method | New, unrelated method | New, unrelated method |
 

# `this` and `super`

## `this`

`this` refers to the current object.

It can be used to:

- Access current-class fields and methods
- Call another constructor using `this(...)`
- Pass the current object as a method or constructor argument
- Return the current object from a method

```java
class Animal {
    private String name;

    Animal(String name) {
        this.name = name;
    }

    Animal() {
        this("Unknown");
    }

    Animal getAnimal() {
        return this;
    }
}
``` 
## Using `this` as the Current Object

The keyword `this` refers to the current object whose method or constructor is running.

### 1. Passing the Current Object to a Method

```java
class Printer {
    void print(Person person) {
        System.out.println(person.name);
    }
}

class Person {
    String name = "John";

    void display() {
        Printer printer = new Printer();
        printer.print(this);
    }
}
```

```java
printer.print(this);
```

Here, `this` passes the current `Person` object to the `print()` method.

---

### 2. Passing the Current Object to a Constructor

```java
class Engine {
    Engine(Car car) {
        System.out.println(car.model);
    }
}

class Car {
    String model = "Toyota";

    Car() {
        Engine engine = new Engine(this);
    }
}
```

```java
new Engine(this);
```

Here, `this` passes the current `Car` object to the `Engine` constructor.

Important:

```text
this      → current object
this(...) → calls another constructor in the same class
```

---

### 3. Returning the Current Object from a Method

```java
class Person {
    String name;

    Person setName(String name) {
        this.name = name;
        return this;
    }
}
```

```java
return this;
```

This returns the current `Person` object.

It allows method chaining:

```java
class Person {
    String name;
    int age;

    Person setName(String name) {
        this.name = name;
        return this;
    }

    Person setAge(int age) {
        this.age = age;
        return this;
    }
}
```

```java
Person person = new Person()
        .setName("John")
        .setAge(25);
```

Each method returns the same current object, allowing another method to be called on it.


## `super`

`super` refers to the direct superclass portion of the current object.

It can be used to:

- Access a superclass field
- Call a superclass method
- Call a superclass constructor using `super(...)`

```java
class Animal {
    String name = "Animal";

    Animal(int age) {
        System.out.println("Age: " + age);
    }

    void display() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    String name = "Dog";

    Dog() {
        super(5);
    }

    void show() {
        System.out.println(this.name);  // Dog
        System.out.println(super.name); // Animal
        super.display();
    }
}
```

## Constructor Rules for Java 21

In Java 21, `this(...)` or `super(...)` must be the first statement in a constructor.

A constructor cannot directly contain both.

```java
class Child extends Parent {
    Child() {
        this(10);
    }

    Child(int value) {
        super(value);
    }
}
```

Constructor chain:

```text
Child() → Child(int) → Parent(int)
```

If neither `this(...)` nor `super(...)` is written, the compiler automatically inserts:

```java
super();
```

If the superclass does not have an accessible no-argument constructor, a compile-time error occurs.

```java
class Parent {
    Parent(int value) {
    }
}

class Child extends Parent {
    Child() {
        // Compile-time error because the compiler tries to call super()
    }
}
```

Constructors are not inherited, but every subclass constructor must eventually invoke a superclass constructor.

---

# The `final` Keyword

The `final` keyword can be applied to classes, methods, and variables.

## Final Class

A final class cannot be extended.

```java
final class Animal {
}

// Compile-time error
class Dog extends Animal {
}
```

## Final Method

A final method cannot be overridden.

```java
class Animal {
    final void sleep() {
    }
}

class Dog extends Animal {
    // Compile-time error
    void sleep() {
    }
}
```

## Final Variable

A final variable can be assigned only once.

```java
final int age = 5;

// Compile-time error
age = 10;
```

A blank final instance variable must be initialized in its declaration, an instance initializer, or every constructor.

```java
class Animal {
    final int age;

    Animal(int age) {
        this.age = age;
    }
}
```

## Final Reference Variables

A final reference cannot be reassigned, but the referenced object may still be modified.

```java
import java.util.ArrayList;
import java.util.List;

class Example {
    public static final List<String> ANIMALS = new ArrayList<>();

    public static void main(String[] args) {
        ANIMALS.add("Cat");  // Valid
        ANIMALS.add("Dog");  // Valid
        ANIMALS.add("Lion"); // Valid

        // Compile-time error: reference cannot be reassigned
        ANIMALS = new ArrayList<>();
    }
}
```

`final` makes the reference constant, not the object immutable.

---

# Abstract Classes and Methods

An abstract class is commonly used to define shared features for related classes.

```java
abstract class Vehicle {
    abstract int getMaxSpeed();
}
```

Important rules:

- An abstract class cannot be instantiated.
- An abstract class may contain abstract and concrete methods.
- An abstract class may contain fields, constructors, static methods, and final methods.
- An abstract class does not need to contain an abstract method.
- A class containing an abstract method must be declared `abstract`.
- A concrete subclass must implement all inherited abstract methods.
- Otherwise, the subclass must also be declared `abstract`.

```java
abstract class Vehicle {
    private String name;

    Vehicle(String name) {
        this.name = name;
    }

    abstract int getMaxSpeed();

    final void displayName() {
        System.out.println(name);
    }

    static void info() {
        System.out.println("Vehicle");
    }
}

class Car extends Vehicle {
    Car() {
        super("Car");
    }

    @Override
    int getMaxSpeed() {
        return 320;
    }
}
```

## Abstract Methods

An abstract method:

- Has no method body
- Ends with a semicolon
- Contains a return type, method name, and parameter list

```java
abstract int getMaxSpeed();
```

An abstract method cannot be:

- `private`
- `static`
- `final`

```java
private abstract void test(); // Invalid
static abstract void test();  // Invalid
final abstract void test();   // Invalid
```

Variables cannot be abstract.

```java
abstract class Vehicle {
    // Invalid
    abstract String type;
}
```

An abstract class cannot be declared `final`.

```java
// Invalid
abstract final class Vehicle {
}
```

A top-level abstract class can be `public` or `defaul`. A nested abstract class may also use `private` or `protected`.

 
# Abstract-Class Questions

## 1. Top-Level Abstract Class Access

A top-level abstract class can be:

- `public`
- Package-private, when no access modifier is written

```java
public abstract class Vehicle {
}

abstract class Animal {
}
```

A top-level abstract class cannot be `private` or `protected`.

```java
private abstract class Vehicle {
} // Compile-time error

protected abstract class Animal {
} // Compile-time error
```

---

## 2. Abstract Nested Classes

An abstract class can contain another abstract class.

```java
abstract class Vehicle {
    protected abstract class Engine {
        abstract void start();
    }
}
```

A member abstract class can be:

- `public`
- `protected`
- `private`
- Package-private
- `static` or non-static

```java
class Outer {
    public abstract class A {
    }

    protected static abstract class B {
    }

    private abstract class C {
    }

    abstract class D {
    }
}
```

An abstract class cannot also be `final`.

```java
abstract final class Vehicle {
} // Compile-time error
```

`abstract` means the class must be extended, while `final` prevents it from being extended.

---

## 3. Abstract Variables

Java does not support abstract variables.

```java
abstract class Vehicle {
    abstract String type;
    // Compile-time error
}
```

The `abstract` keyword can be applied to classes and methods, but not fields.

Fields in an abstract class are normal fields. They are not automatically `public`, `static`, or `final`.

```java
abstract class Vehicle {
    private String name;
    protected int speed;
    static int count;
    final int wheels = 4;
    public double weight;
}
```

---

## 4. Method Modifiers

The compiler does not automatically add `public`, `static`, or `final` to methods in an abstract class.

```java
abstract class Vehicle {
    abstract void stop(); // Package-private abstract method

    void move() {         // Package-private concrete method
    }

    public void start() { // Public concrete method
    }
}
```

An abstract method can be:

```java
public abstract void start();

protected abstract void stop();

abstract void move(); // Package-private
```

An abstract method cannot be `private`, `static`, or `final`.

```java
private abstract void test(); // Compile-time error

static abstract void test();  // Compile-time error

final abstract void test();   // Compile-time error
```

---

## 5. Constructors in Abstract Classes

An abstract class can have constructors.

```java
abstract class Vehicle {
    Vehicle() {
        System.out.println("Vehicle constructor");
    }
}

class Car extends Vehicle {
    Car() {
        System.out.println("Car constructor");
    }
}
```

```java
Car car = new Car();
```

Output:

```text
Vehicle constructor
Car constructor
```

An abstract class cannot be instantiated directly, but its constructor runs when a subclass object is created.

If no constructor is declared, the compiler provides a default no-argument constructor.

---

## 6. Concrete and Static Methods

An abstract class can contain concrete instance methods.

```java
abstract class Vehicle {
    void move() {
        System.out.println("Moving");
    }
}
```

It can also contain static methods.

```java
abstract class Vehicle {
    static void info() {
        System.out.println("Vehicle");
    }
}
```

```java
Vehicle.info();
```

An abstract method cannot be static.

```java
abstract class Vehicle {
    static abstract void info();
    // Compile-time error
}
```

The `default` method keyword is used only in interfaces.

```java
abstract class Vehicle {
    default void move() {
    } // Compile-time error
}
```

A method with no access modifier is package-private. It is not an interface-style default method.

--- 

# Interfaces

An interface defines a contract containing behaviors that implementing classes agree to provide.

```java
interface Drivable {
    void turnLeft();

    void turnRight();
}

class Car implements Drivable {
    @Override
    public void turnLeft() {
    }

    @Override
    public void turnRight() {
    }
}
```

A class can implement multiple interfaces.

```java
interface CanEat {
    void eat();
}

interface CanDrink {
    void drink();
}

class Dog implements CanEat, CanDrink {
    @Override
    public void eat() {
    }

    @Override
    public void drink() {
    }
}
```

Important:

- An interface cannot be instantiated.
- An interface does not have constructors.
- A class uses `implements` to implement an interface.
- An interface uses `extends` to inherit from one or more interfaces.
- A class can extend one class and implement multiple interfaces.

```java
class Eagle extends Bird implements CanMove, CanFly {
}
```

## Interface Fields

All interface fields are implicitly:

- `public`
- `static`
- `final`

They must be initialized when declared.

```java
interface Animal {
    int TYPE = 5;
}
```

This is equivalent to:

```java
interface Animal {
    public static final int TYPE = 5;
}
```

This is invalid:

```java
interface Animal {
    int TYPE; // Compile-time error: not initialized
}
```

## Interface Methods

An interface can contain:

- Abstract methods
- Default methods
- Static methods
- Private methods
- Private static methods

### Abstract Interface Methods

A method without `private`, `default`, or `static` is implicitly `public abstract`.

```java
interface CanFly {
    void fly();
}
```

This is equivalent to:

```java
interface CanFly {
    public abstract void fly();
}
```

An implementing class must declare the method as `public`.

```java
class Bird implements CanFly {
    @Override
    public void fly() {
    }
}
```

### Default Methods

A default method must have a body.

```java
interface Employee {
    default int getSalary() {
        return 500;
    }
}
```

Implementing classes inherit default methods and do not have to override them unless there is a conflict.

### Static Methods

A static interface method must have a body.

```java
interface Employee {
    static int getAge() {
        return 5;
    }
}
```

Static interface methods are not inherited. Call them using the interface name:

```java
int age = Employee.getAge();
```

### Private Methods

Private interface methods must have a body and can only be called from within the interface.

```java
interface Employee {
    private void log() {
        System.out.println("Log");
    }

    default void work() {
        log();
    }
}
```

The following combinations are invalid:

```java
abstract static void test();
abstract default void test();
private abstract void test();
```

## Interface Declaration Rules

Valid top-level declarations:

```java
public interface CanFly {
}

interface CanMove {
}
```

Invalid:

```java
interface public CanFly {
}
```

A top-level interface can only be:

- `public`
- Package-private

A member interface may also be `private` or `protected`.

Interfaces support an **“is-a” relationship**:

```java
CanFly bird = new Bird();
```

The interface describes what an object can do, while the implementing class defines how it does it.
