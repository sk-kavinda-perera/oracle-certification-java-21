## Polymorphism

Polymorphism allows the same method call to produce different behavior depending on the object.

Java supports two main types of polymorphism:

* **Static polymorphism:** Method overloading
* **Dynamic polymorphism:** Method overriding

### Method Overriding

Method overriding occurs when a subclass provides its own implementation of an inherited instance method.

```java
class Vehicle {
    public void accelerate() {
        System.out.println("Vehicle is accelerating");
    }
}

class Car extends Vehicle {
    @Override
    public void accelerate() {
        System.out.println("Car is accelerating");
    }
}

class Motorcycle extends Vehicle {
    @Override
    public void accelerate() {
        System.out.println("Motorcycle is accelerating");
    }
}
```

> These classes are shown together for brevity. Only one top-level `public` class is normally allowed per `.java` file.

---

## Upcasting

A subclass object can be assigned to a superclass reference.

```java
Vehicle vehicle = new Car();
vehicle.accelerate(); // Car is accelerating
```

This is called **upcasting** and happens automatically.

* Reference type: `Vehicle`
* Actual object type: `Car`

For overridden instance methods, Java uses the actual object type at runtime. Therefore, `Car.accelerate()` is invoked.

---

## Downcasting

A superclass reference cannot be assigned directly to a subclass reference.

```java
Vehicle vehicle = new Car();

Car car = vehicle; // Does not compile
```

An explicit cast is required:

```java
Car car = (Car) vehicle;
car.accelerate(); // Car is accelerating
```

The cast compiles because a `Vehicle` reference could refer to a `Car`.

At runtime, Java checks whether the referenced object is actually a `Car`.

```java
Vehicle vehicle = new Vehicle();

Car car = (Car) vehicle; // ClassCastException at runtime
```

The cast does not change the object. It only tells the compiler to treat the reference as a more specific type.

---

## Using `instanceof`

Use `instanceof` to check the object's type before downcasting.

```java
if (vehicle instanceof Car) {
    Car car = (Car) vehicle;
    car.accelerate();
}
```

Java 21 supports pattern matching for `instanceof`:

```java
if (vehicle instanceof Car car) {
    car.accelerate();
}
```

This performs the type check and creates the cast variable automatically.

---

## Invalid Assignments and Casts

```java
Car car = new Vehicle(); // Does not compile
```

A superclass object cannot be assigned to a subclass reference.

```java
Car car = (Car) new Vehicle();
```

This compiles, but throws a `ClassCastException` at runtime because the object is a `Vehicle`, not a `Car`.

---

## Polymorphic Method Parameters

Without polymorphism, separate methods might be created for every subclass:

```java
static void printAcceleration(Car car) {
    car.accelerate();
}

static void printAcceleration(Motorcycle motorcycle) {
    motorcycle.accelerate();
}
```

Polymorphism allows them to be replaced with one method:

```java
static void printAcceleration(Vehicle vehicle) {
    vehicle.accelerate();
}
```

The method can accept any `Vehicle` subclass:

```java
printAcceleration(new Car());        // Car is accelerating
printAcceleration(new Motorcycle()); // Motorcycle is accelerating
printAcceleration(new Vehicle());    // Vehicle is accelerating
```

---

## Which Method Is Invoked?

For overridden instance methods, the method implementation is selected using the **actual object type**, not the reference type.

```java
Vehicle first = new Vehicle();
first.accelerate(); // Vehicle.accelerate()

Vehicle second = new Car();
second.accelerate(); // Car.accelerate()

Car third = new Car();
third.accelerate(); // Car.accelerate()
```

| Reference type | Object type | Method invoked         |
| -------------- | ----------- | ---------------------- |
| `Vehicle`      | `Vehicle`   | `Vehicle.accelerate()` |
| `Vehicle`      | `Car`       | `Car.accelerate()`     |
| `Car`          | `Car`       | `Car.accelerate()`     |

If the child class does not override the method, the inherited parent implementation is invoked.

```java
class Bus extends Vehicle {
    // Does not override accelerate()
}

Vehicle vehicle = new Bus();
vehicle.accelerate(); // Vehicle is accelerating
```

---

## Calling the Parent Implementation

A child class can call the overridden parent method using `super`.

```java
class Car extends Vehicle {
    @Override
    public void accelerate() {
        super.accelerate();
        System.out.println("Car is accelerating");
    }
}
```

```java
new Car().accelerate();
```

Output:

```text
Vehicle is accelerating
Car is accelerating
```

`super.accelerate()` explicitly invokes the parent class implementation.

---

## Overloading vs. Overriding

### Overriding

Overridden instance methods are selected at runtime using the object's actual type.

```java
Vehicle vehicle = new Car();
vehicle.accelerate(); // Car.accelerate()
```

### Overloading

Overloaded methods are selected at compile time using the declared reference and argument types.

```java
static void inspect(Vehicle vehicle) {
    System.out.println("Vehicle");
}

static void inspect(Car car) {
    System.out.println("Car");
}
```

```java
Vehicle vehicle = new Car();
Car car = new Car();

inspect(vehicle); // Vehicle
inspect(car);     // Car
```

Although `vehicle` refers to a `Car`, its declared type is `Vehicle`, so `inspect(Vehicle)` is selected.

--- 
* Overloading is resolved at compile time.
* Overriding is resolved at runtime.

---
 ## Encapsulation

Encapsulation restricts direct access to an object's internal state.

Fields are usually declared `private` and accessed through methods.

```java
class Vehicle {
    private String model;
    private int maxSpeed;
    private boolean automatic;

    public void setMaxSpeed(int maxSpeed) {
        if (maxSpeed < 0) {
            throw new IllegalArgumentException(
                    "Maximum speed cannot be negative");
        }

        this.maxSpeed = maxSpeed;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public boolean isAutomatic() {
        return automatic;
    }
}
```

### Important Points

* `private` fields cannot be accessed directly from another class.
* Getters return field values.
* Setters can validate values before modifying fields.
* `this.maxSpeed` refers to the current object's field.
* Boolean getters commonly use the `is` prefix. A boolean getter can use either is or get.

```java
Vehicle vehicle = new Vehicle();

vehicle.setMaxSpeed(200);
System.out.println(vehicle.getMaxSpeed()); // 200

// vehicle.maxSpeed = 300; // Does not compile
```

 Encapsulation does not require every field to have a getter and setter. Expose only the operations that are required.

---
 ## Nested Classes

A nested class is a class declared inside another class.

```text
Nested Classes
├── Static Nested Class
└── Inner Class (Non-static Nested Class)
    ├── Anonymous Class
    ├── Member Inner Class
    └── Local Class
```

* A **static nested class** does not require an instance of the enclosing class.
* An **inner class** is non-static and is associated with an instance of the enclosing class.

---

# 1. Anonymous Class

An anonymous class is an inner class without a declared name.

It declares and creates an object in the same expression. It is useful when a class implementation is needed only once or in a small part of the program.

An anonymous class can:

* Extend one class, or
* Implement one interface

```java
interface Animal {
    void show(String name, int speed);
}

public class Test {
    public static void main(String[] args) {
        Animal animal = new Animal() {
            @Override
            public void show(String name, int speed) {
                System.out.println(name + ": " + speed);
            }
        };

        animal.show("Cheetah", 90);
    }
}
```

Output:

```text
Cheetah: 90
```

Although anonymous classes are commonly used for one-off implementations, evaluating the expression multiple times creates multiple objects.

---

## Anonymous Class Syntax

An anonymous class expression contains:

1. The `new` operator
2. The class to extend or interface to implement
3. Constructor parentheses
4. A class body

```java
Animal animal = new Animal() {
    @Override
    public void show(String name, int speed) {
        System.out.println(name + ": " + speed);
    }
};
```

When implementing an interface, empty parentheses are used:

```java
new Animal() {
    // Anonymous class body
};
```

When extending a class, constructor arguments may be passed:

```java
class Vehicle {
    Vehicle(int maxSpeed) {
        System.out.println(maxSpeed);
    }
}

Vehicle vehicle = new Vehicle(200) {
    // Anonymous subclass of Vehicle
};
```

---

## Accessing Local Variables

An anonymous class can access local variables only when they are `final` or **effectively final**.

An effectively final variable is assigned once and never changed.

```java
String animalName = "Cheetah";
int speed = 90;

Animal animal = new Animal() {
    @Override
    public void show(String name, int ignored) {
        System.out.println(animalName + ": " + speed);
    }
};
```

The following does not compile:

```java
int speed = 90;

Animal animal = new Animal() {
    @Override
    public void show(String name, int value) {
        System.out.println(speed);
    }
};

speed = 100; // Does not compile
```

Because `speed` is captured by the anonymous class, it must remain effectively final.

---

## Accessing Enclosing Class Members

An anonymous class can access members of its enclosing class, including `private` members.

```java
class Zoo {
    private String location = "Colombo";

    void displayAnimal() {
        Animal animal = new Animal() {
            @Override
            public void show(String name, int speed) {
                System.out.println(name + " at " + location);
            }
        };

        animal.show("Cheetah", 90);
    }
}
```

---

## Members of an Anonymous Class

An anonymous class can declare:

* Fields
* Extra methods
* Instance initializers
* Static members
* Static initializers
* Nested classes and interfaces

```java
Animal animal = new Animal() {
    private int calls;

    {
        calls = 0;
    }

    private void log(String message) {
        System.out.println(message);
    }

    @Override
    public void show(String name, int speed) {
        calls++;
        log(name + ": " + speed);
    }
};
```

An extra method is not accessible through the interface reference:

```java
animal.show("Cheetah", 90); // Valid

// animal.log("Hello"); // Does not compile
```

The reference type is `Animal`, and `Animal` does not declare `log()`.

---

## Static Members in Java 21

Java 21 allows anonymous and other inner classes to declare static members and static initializers.

```java
Animal animal = new Animal() {
    static int count;

    static {
        count = 10;
    }

    static void printCount() {
        System.out.println(count);
    }

    @Override
    public void show(String name, int speed) {
        printCount();
        System.out.println(name + ": " + speed);
    }
};
```

> Before Java 16, inner classes could generally declare only static constant variables. This restriction does not apply in Java 21.

---

## Constructors

An anonymous class cannot explicitly declare a constructor because it has no name.

Use an instance initializer instead:

```java
Animal animal = new Animal() {
    {
        System.out.println("Anonymous object created");
    }

    @Override
    public void show(String name, int speed) {
        System.out.println(name + ": " + speed);
    }
};
```

---

# 2. Member Inner Class

A member inner class is a non-static class declared directly inside another class.

It is associated with an instance of the enclosing class.

```java
class Zoo {
    private String location = "Colombo";

    class AnimalKeeper {
        void showLocation() {
            System.out.println(location);
        }
    }
}
```

To create a member inner class object, first create an enclosing class object:

```java
Zoo zoo = new Zoo();

Zoo.AnimalKeeper keeper = zoo.new AnimalKeeper();

keeper.showLocation(); // Colombo
```

A member inner class can directly access all members of the enclosing class, including `private` members.

```java
class Outer {
    private int value = 10;

    class Inner {
        void printValue() {
            System.out.println(value);
        }
    }
}
```

## Important Points

* It does not use the `static` modifier.
* It requires an instance of the enclosing class.
* It can access enclosing instance and static members.
* Java 21 allows it to declare static members.
* It can use access modifiers such as `public`, `protected`, `private`, or package-private.

---

# 3. Local Class

A local class is declared inside a method, constructor, or initializer block.

Its scope is limited to the block in which it is declared.

```java
class Zoo {
    void displayAnimal() {
        class LocalAnimal {
            void show() {
                System.out.println("Local animal");
            }
        }

        LocalAnimal animal = new LocalAnimal();
        animal.show();
    }
}
```

The local class cannot be used outside the method:

```java
Zoo zoo = new Zoo();
zoo.displayAnimal();

// LocalAnimal animal; // Does not compile It means LocalAnimal exists only inside the displayAnimal() method.
```

A local class can access:

* Members of the enclosing class. its class not enclosing method
* Local variables that are `final` or effectively final

```java
class Zoo {
    private String location = "Colombo";

    void displayAnimal() {
        String name = "Cheetah";

        class LocalAnimal {
            void show() {
                System.out.println(name + " at " + location);
            }
        }

        new LocalAnimal().show();
    }
}
```

## Important Points

* It is declared inside a block.
* It cannot have an access modifier such as `public` or `private`.
* It cannot be declared `static`.
* It can access effectively final local variables.
* It has a name and can create multiple objects within its scope.

```java
LocalAnimal first = new LocalAnimal();
LocalAnimal second = new LocalAnimal();
```

---

# 4. Static Nested Class

A static nested class is declared with the `static` modifier inside another class.

It is not an inner class because it is not associated with an instance of the enclosing class.

```java
class Zoo {
    static class AnimalInfo {
        void show() {
            System.out.println("Animal information");
        }
    }
}
```

It can be instantiated without creating an enclosing class object:

```java
Zoo.AnimalInfo info = new Zoo.AnimalInfo();

info.show();
```

A static nested class can directly access only static members of the enclosing class.

```java
class Zoo {
    private static String country = "Sri Lanka";
    private String location = "Colombo";

    static class AnimalInfo {
        void show() {
            System.out.println(country); // Valid

            // System.out.println(location); // Does not compile
        }
    }
}
```

To access instance members, it needs an enclosing class object:

```java
static class AnimalInfo {
    void show() {
        Zoo zoo = new Zoo();
        System.out.println(zoo.location);
    }
}
```

## Important Points

* It is declared using `static`.
* It does not require an enclosing class instance.
* It can directly access only static members of the enclosing class.
* It can have constructors, fields, methods, and nested classes.
* It may use any access modifier.

---

## Comparison

| Type                | Requires enclosing object | Can access enclosing instance members directly | Declared where          |
| ------------------- | ------------------------: | ---------------------------------------------: | ----------------------- |
| Anonymous class     |               Usually yes |                                            Yes | Inside an expression    |
| Member inner class  |                       Yes |                                            Yes | Directly inside a class |
| Local class         |                       Yes |                                            Yes | Inside a block          |
| Static nested class |                        No |                                             No | Directly inside a class |

---

## Anonymous Class vs. Local Class

| Anonymous class                                 | Local class                                          |
| ----------------------------------------------- | ---------------------------------------------------- |
| Has no declared name                            | Has a name                                           |
| Declared and instantiated together              | Declared first and instantiated later                |
| Cannot declare an explicit constructor          | Can declare a constructor                            |
| Best for a one-off implementation               | Can create multiple objects                          |
| Can extend one class or implement one interface | Can extend a class and implement interfaces normally |

---

## Inner Class vs. Static Nested Class

```java
class Outer {
    class Inner {
    }

    static class Nested {
    }
}
```

Creating the objects:

```java
Outer outer = new Outer();

Outer.Inner inner = outer.new Inner();

Outer.Nested nested = new Outer.Nested();
```

* `Inner` requires an `Outer` object.
* `Nested` does not require an `Outer` object.

---

## Important OCP Points

* Every inner class is a nested class, but not every nested class is an inner class.
* A static nested class is not an inner class.
* Inner classes are associated with an enclosing class instance.
* Anonymous classes have no declared name.
* Anonymous classes cannot explicitly declare constructors.
* Local and anonymous classes can access only final or effectively final local variables.
* Member inner classes can access all members of their enclosing object.
* Static nested classes can directly access only static enclosing members.
* Java 21 allows static declarations inside inner classes.
* A lambda is usually shorter than an anonymous class when implementing a functional interface.

## Record Classes

A record class is a compact way to create a **data carrier**.

Records became a permanent Java feature in Java 16. They reduce the boilerplate normally required in a POJO or data class.

```java
public record Person(String name, int age) {}
```

Java automatically provides:

* A canonical constructor: `Person(String name, int age)`
* Private final fields for `name` and `age`
* Public accessor methods: `name()` and `age()`
* `equals()`
* `hashCode()`
* `toString()`

```java
Person person = new Person("John", 30);

System.out.println(person.name()); // John
System.out.println(person.age());  // 30
System.out.println(person);        // Person[name=John, age=30]
```

> Record accessors are named `name()` and `age()`, not `getName()` and `getAge()`.

---

## Record Immutability

Record component fields are implicitly:

* `private`
* `final`
* Non-static

Therefore, they cannot be reassigned after construction.

```java
Person person = new Person("John", 30);

// person.age = 40; // Does not compile
```

Records do not automatically generate setters.

However, records are only **shallowly immutable**. If a component refers to a mutable object, that object may still be modified.

```java
record Team(List<String> members) {}

List<String> names = new ArrayList<>();
Team team = new Team(names);

names.add("John"); // The list inside the record also changes
```

A defensive copy can be used:

```java
record Team(List<String> members) {
    Team {
        members = List.copyOf(members);
    }
}
```

---

## Important Record Rules

* A record class is implicitly `final`.
* A record cannot be `abstract`, `sealed`, or `non-sealed`.
* A record cannot explicitly extend another class.
* Every record implicitly extends `java.lang.Record`.
* A record can implement one or more interfaces.
* A record can contain methods, constructors, static fields, static methods and nested types.
* A record cannot declare additional non-static instance fields.
* A record cannot contain instance initializer blocks.
* Empty parentheses are required even when there are no components.

---

## Empty Record

A record may have no components, but the parentheses are still required.

```java
public record Person {} // Does not compile

public record Person() {} // Valid
```

An empty record has a no-argument canonical constructor.

---

## Record Modifiers

For a **top-level record**:

```java
public record Person() {}       // Valid
record Person() {}              // Valid: package-private
public final record Person() {} // Valid, but final is redundant
```

The following top-level declarations are invalid:

```java
protected record Person() {} // Does not compile
private record Person() {}   // Does not compile
static record Person() {}    // Does not compile
abstract record Person() {}  // Does not compile
sealed record Person() {}    // Does not compile
```

`protected`, `private`, and `static` may be valid for a **member record** declared inside another class:

```java
class Outer {
    private record PrivatePerson() {}   // Valid

    protected static record Employee() {} // Valid
}
```

The `static` modifier is redundant because member records are implicitly static.

Repeated modifiers are not allowed:

```java
public public record Person() {} // Does not compile

public private record Person() {} // Does not compile
```

---

## Fields Inside a Record

A record cannot declare additional instance fields.

```java
public record Person() {
    int age;      // Does not compile
    int speed = 10; // Does not compile
}
```

Static fields are allowed:

```java
public record Person() {
    static int count;              // Valid
    static int age = 10;           // Valid
    static final int MAX_AGE = 150; // Valid
}
```

The values stored in each record object must be completely described by the components in the record header.

---

## Inheritance and Interfaces

A record cannot contain an `extends` clause:

```java
record Person() extends Object {} // Does not compile
```

It implicitly extends `java.lang.Record`.

A record can implement interfaces:

```java
interface Drink {
    void canDrink(String drinkName);
}

public record Person(String name, int age) implements Drink {
    @Override
    public void canDrink(String drinkName) {
        System.out.println(name + " can drink " + drinkName);
    }
}
```

Record classes may implement multiple interfaces.

---

## POJO Compared with a Record

A similar normal Java class requires more code:

```java
import java.util.Objects;

public final class PersonTwo {
    private final String name;
    private final int age;

    public PersonTwo(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public boolean equals(Object object) {
        if (this == object) {
            return true;
        }

        if (!(object instanceof PersonTwo other)) {
            return false;
        }

        return age == other.age
                && Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "PersonTwo[name=" + name + ", age=" + age + "]";
    }
}
```

The equivalent record is:

```java
public record Person(String name, int age) {}
```

Records provide functionality similar to ordinary POJOs, Kotlin data classes and tools such as Lombok, but records are built directly into the Java language.

---

## `equals()` and `hashCode()`

Consider two different objects:

```java
Person first = new Person("John Doe", 30);
Person second = new Person("John Doe", 30);

System.out.println(first == second);      // false
System.out.println(first.equals(second)); // true

System.out.println(first.hashCode());
System.out.println(second.hashCode());
```

`first == second` is `false` because they are different objects.

`first.equals(second)` is `true` because they:

* Are instances of the same record class
* Have equal component values
* == checks if two references point to the same object, while equals() checks content equality (records automatically override it to compare their component values).
Equal objects must produce the same hash code.

For an ordinary class, `equals()` uses object identity unless the class overrides it. A record automatically provides value-based `equals()` and `hashCode()`.

---

## Canonical Constructor

The canonical constructor has the same parameters, names, types and order as the record header.

Java creates it automatically:

```java
public record Person(String name, int age) {}
```

This is approximately equivalent to:

```java
public record Person(String name, int age) {
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

A record’s canonical constructor must have the same access level as the record: public record → public constructor, package-private record → package-private constructor.

---

## Additional No-Argument Constructor

A record with components does not automatically receive a no-argument constructor.

This is invalid:

```java
public record Person(String name, int age) {
    public Person() {
        // Does not compile: components are not initialized
    }
}
```

A non-canonical constructor is allowed if it delegates to another constructor using `this(...)`:

```java
public record Person(String name, int age) {
    public Person() {
        this("Unknown", 0);
    }
}
```

```java
Person person = new Person();

System.out.println(person);
// Person[name=Unknown, age=0]
```

Therefore:

* `record Person() {}` has a no-argument canonical constructor.
* `record Person(String name, int age) {}` may declare a no-argument constructor, but it must delegate using `this(...)`.

---

## Compact Canonical Constructor

A compact canonical constructor does not repeat the parameter list.

Notice that there are **no parentheses** after `Person`.

```java
public record Person(String name, int age) {
    public Person {
        if (age < 0) {
            throw new IllegalArgumentException(
                    "Age cannot be negative");
        }
    }
}
```

```java
Person person = new Person("Paul", -5);
// Throws IllegalArgumentException
```

The component fields are assigned automatically after the compact constructor body finishes.

Do not assign directly to the fields:

```java
public record Person(String name, int age) {
    public Person {
        // this.age = age; // Does not compile
    }
}
```

The constructor parameters may be validated or changed before automatic assignment:

```java
import java.util.Objects;

public record Person(String name, int age) {
    public Person {
        name = Objects.requireNonNull(name).strip();

        if (age < 0) {
            throw new IllegalArgumentException(
                    "Age cannot be negative");
        }
    }
}
```

A record cannot contain both a normal canonical constructor and a compact canonical constructor because both represent the same constructor.

---

## Custom Methods

A record can contain custom instance methods:

```java
public record Person(String name, int age) {
    public boolean isAdult() {
        return age >= 18;
    }
}
```

```java
Person person = new Person("John", 30);

System.out.println(person.isAdult()); // true
```

A record may also explicitly provide its own:

* Accessor methods
* `equals()`
* `hashCode()`
* `toString()`

Example of a custom accessor:

```java
public record Person(String name, int age) {
    @Override
    public String name() {
        return name.toUpperCase();
    }
}
```

The accessor must be:

* `public`
* Non-static
* Without parameters
* The same return type as its component

---

## Nested Record Classes

A record declared inside another class is implicitly static.

```java
public class OuterPerson {
    private int instanceValue = 10;
    private static int staticValue = 20;

    public record InnerPerson(String name) {
        public void show() {
            System.out.println(staticValue); // Valid

            // System.out.println(instanceValue);
            // Does not compile
        }
    }
}
```

Because `InnerPerson` is implicitly static, it cannot directly access the enclosing object's instance field `instanceValue`.

It can directly access static fields such as `staticValue`.

Create the nested record without creating an `OuterPerson` object:

```java
OuterPerson.InnerPerson person =
        new OuterPerson.InnerPerson("John");

person.show(); // 20
```

The following are equivalent:

```java
class Outer {
    record Person(String name) {}
}
```

```java
class Outer {
    static record Person(String name) {}
}
```

The explicit `static` modifier is allowed but redundant for a member record.

> The record class itself is static. Its components and normal methods are still instance members.

---

## Local Record Classes

A record can be declared inside a method:

```java
void process() {
    record Person(String name, int age) {}

    Person person = new Person("John", 30);
    System.out.println(person);
}
```

A local record is also implicitly static.

Unlike an ordinary local class, it cannot capture variables from the enclosing method:

```java
void process() {
    int minimumAge = 18;

    record Person(String name, int age) {
        boolean isAdult() {
            // return age >= minimumAge;
            // Does not compile
            return age >= 18;
        }
    }
}
```

You cannot explicitly write `static` on a local record:

```java
void process() {
    // static record Person() {}
    // Does not compile
}
```

Member records and local records are both implicitly static.

---

## Important OCP Points

* Records became permanent in Java 16.
* Record components create private final fields.
* Record accessors use `name()`, not `getName()`.
* Records are shallowly immutable.
* A record is implicitly final.
* A record cannot be abstract, sealed or non-sealed.
* A record cannot explicitly extend another class.
* A record implicitly extends `java.lang.Record`.
* A record can implement interfaces.
* Additional instance fields are not allowed.
* Static fields and methods are allowed.
* Additional constructors must delegate to the canonical constructor.
* A compact constructor has no parameter list.
* Nested and local records are implicitly static.
* An empty record is valid, but `()` is required.


# Var Keyword

* Introduced in **Java 10 (2018)**.
* Before `var`, developers had to explicitly declare the type of every local variable.
* This could create unnecessary repetition, especially when the type was obvious from the assigned value.
* `var` allows **local variable type inference**, where the compiler determines the variable type from its initializer.

Example:

```java
public class Test {
    public static void main(String[] args) {

        var message = "Hello World";  // inferred as String
        var number = 49;              // inferred as int
        var list = new String[5];     // inferred as String[]
    }
}
```

The compiler decides the type at compile time. `var` is not dynamically typed.

---

## Usage of var

`var` can be used:

* Inside a method
* Inside a block
* With primitive types
* With reference types

Example:

```java
var age = 20;              // int
var name = "Java";         // String
var person = new Person(); // Person object
```

---

# Limitations of var

### 1. Only local variables

`var` cannot be used for instance variables or class variables.

```java
class Test {

    var age; // Compilation error
}
```

---

### 2. Cannot be used for method parameters or return types

```java
public static void print(var name) { } 
// Compilation error

public static var getValue() { }
// Compilation error
```

---

### 3. Requires an initializer

The compiler needs a value to infer the type.

```java
var age; // Compilation error

var age = 20; // Valid
```

---

### 4. Cannot assign null directly

`null` does not have a type, so Java cannot infer the variable type.

```java
var test = null; // Compilation error
```

---

### 5. Cannot be used with lambda expressions directly

A lambda requires a target type.

```java
var function = name -> name.length(); 
// Compilation error
```

Correct:

```java
Function<String, Integer> function =
        name -> name.length();
```

---

## Important var Examples

```java
class Test {

    public static void main(String[] args) {

        var num = 10;

        num = "Java"; // Compilation error
    }
}
```

`num` is inferred as `int`, so it cannot store a `String`.

---

### var with arrays

Wrong:

```java
var[] numbers = new int[5]; // Compilation error
```

Correct:

```java
var numbers = new int[5];
```

The compiler already knows it is an `int[]`.
 
### var with Arrays

You cannot use square brackets (`[]`) with `var` because the compiler automatically infers the array type from the initializer.

Wrong:
```java

var[] numbers = new int[5]; // Compilation error
var numbers[] = new int[5]; // Compilation error
```


```java 

### var as a class name

`var` is a reserved type name.

```java
class Var { } // Valid

class var { } // Compilation error (keyword reserved)
```

---

# Wrapper Classes

Wrapper classes convert primitive values into objects.

Primitive type → Wrapper class

| Primitive | Wrapper   |
| --------- | --------- |
| byte      | Byte      |
| short     | Short     |
| int       | Integer   |
| long      | Long      |
| float     | Float     |
| double    | Double    |
| boolean   | Boolean   |
| char      | Character |

Wrapper classes are available in the `java.lang` package.

---

# Converting Between Primitive and Wrapper Types

There are two conversions:

1. Primitive → Wrapper
2. Wrapper → Primitive

---

## Primitive to Wrapper

### Using `valueOf()`

```java
Integer number = Integer.valueOf(20);
```

`valueOf()` is preferred.

Older constructor style:

```java
Integer number = new Integer(20);
```

The wrapper constructors are deprecated and should not be used.

---

## Wrapper to Primitive

Using methods:

```java
Integer number = Integer.valueOf(20);

int value = number.intValue();
```

Other methods:

```java
doubleValue();
shortValue();
longValue();
floatValue();
```

---

# Autoboxing and Unboxing

Java automatically converts between primitives and wrapper classes.

---

## Autoboxing

Primitive → Wrapper automatically

Before:

```java
Integer number = Integer.valueOf(5);
```

With autoboxing:

```java
Integer number = 5;
```

---

## Unboxing

Wrapper → Primitive automatically

Before:

```java
int value = number.intValue();
```

With unboxing:

```java
Integer number = 5;

int value = number;
```

---

## Examples

```java
public class Test {

    public static void main(String[] args) {

        double value = 12.8;

        Double object = value; // Autoboxing
        double result = object; // Unboxing

        System.out.println(object); // 12.8
        System.out.println(result); // 12.8


        Character character = 'b'; // Autoboxing
        char letter = character;   // Unboxing

        System.out.println(character); // b
        System.out.println(letter);    // b
    }
}
```

---

# Null with Wrapper Classes

Wrapper classes can store `null`.

```java
Integer number = null; // Valid
```

Primitive types cannot:

```java
int number = null; // Compilation error
```

---

## Unboxing null causes NullPointerException

```java
Integer number = null;

int value = number;
```

Compilation succeeds, but runtime throws:

```
NullPointerException
```

Because Java tries to convert:

```java
number.intValue()
```

but the object is `null`.

---

## Casting null

```java
int value = (Integer) null;
```

This compiles, but throws `NullPointerException` at runtime because Java tries to unbox `null`.

---
 
### Autoboxing with Numeric Literals

Autoboxing does not combine **widening conversion** and **boxing** in one step.

Example:

```java
Double d1 = 7;   
// ❌ Compilation error
// 7 is an int literal.
// Java would need to do: int → double → Double
// Java does not combine widening conversion + boxing automatically.

Double d2 = 7.0; 
// ✅ Valid
// 7.0 is a double literal.
// Java only needs to do: double → Double
// This is direct autoboxing.
 
``` 

# Important OCP Points

* `var` is only for local variables.
* `var` does not make Java dynamically typed.
* The compiler determines the type during compilation.
* `var` requires an initializer.
* `var` cannot be used with `null`.
* `var` cannot be used with lambda expressions without a target type.
* Wrapper classes are objects representing primitive values.
* Autoboxing converts primitive → wrapper.
* Unboxing converts wrapper → primitive.
* Unboxing a `null` wrapper causes `NullPointerException`.
* Wrapper classes are immutable.
* Prefer `Integer.valueOf()` over deprecated wrapper constructors.
