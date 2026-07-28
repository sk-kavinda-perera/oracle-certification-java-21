- Alternatively you can intialzie like 
    - nums[1] = 5; //2 element has value 5
    - nums[2] = 6; // 3 element has value 6

Size is required during intialization of an array
```diff
+ int [] numArray = new int[5]; //size is required during intialization
 or else

+ int[] numArray = {5,10,2,3};
+ int ag2e[] = new int[]{15,16 ,17} ; -> this is valid too

```

```
To print the array length
  - System.out.println(numArray.length);

To print Array numbers
 - System.out.println(Array.toString(numArray));

If you assign an element for over the size or negative index you will get ArrayIndexOutOfBoundException which is a runtime error. but during compilation it doesnt throw.

```


```
  int [] numArray = new int[5];
        numArray [0]=1;
        numArray [1]=15;
        numArray [2]=30;
        numArray [3]=45;
        numArray [4]=55;
        System.out.println(numArray); //[I@6acbcfc0 Printing the memory address.
        System.out.prinlnt(Arrays.toString(numArray)) // no need any custom tostring() method. output- > [1, 15, 30, 45, 55]

        ** toString method is not need for simple primitive data types or String data type or simple object data type . but for custom classes  you need.


        //Without toString method.

        Student[] students  = new Student[2];
        students[0] = new Student(10, "KAVI");
        students[1] = new Student(43, "SAMPLE"); 
        System.out.println(students); // [Lsample.Student;@3feba861
        System.out.println(Arrays.toString(students));//[sample.Student@5b480cf9, sample.Student@6f496d9f]


```


```
int [] numArray = new int[5];
numArray[0] = 5;
//now we didnt assign any other values to this array's elements but compiler doesnt give any erorr cause its assinging the default value . if it is an int array defautl value is 0 and if it is string default is null and for float 0.0

```

```
You can update the value of a specific element in an array by
numArray[3] = 10;

numArray.length = 8; -> compilation error. you can modfiy the size.
int age[] = new int[]{15,16.5,17} //compilation error since 16.5 is not integer
int age[] = new int[]{15,(int) 16.5,17} -> this is valid.


if you want to create an array which can have values of different type the nuse Object class. Every class has the object as the superclass.

Object mixedArray  = {3,5,5.8f, "cat", true}; -> valid
```

```
- Sorting Array

method -> Arrays.sort(arrayName);
    -> For numbers -> negative values and then all the way to the highest number
    -> For Char - > It sorts capital letters first and then simple letters
    -> For String
        - Compare the first character of each string.
        - Uppercase letters come before lowercase letters.
        - If the first characters are the same, compare the second character.
        - Continue comparing character by character until a difference is found.
        - If one string ends first (and all previous characters are the same), the shorter string comes first.

        Example ->
        
```
### Example

```java
import java.util.Arrays;

String[] words = {"cat", "Car", "cab", "Bat", "apple", "Cab", "Cbr"};

Arrays.sort(words);

System.out.println(Arrays.toString(words));
```

**Output:**

```text
[Bat, Cab, Car, Cbr, apple, cab, cat]
```



 `
### String Numbers

```java
String[] array5 = {"3", "300", "20", "35", "37", "31", "4", "70"};

Arrays.sort(array5);

System.out.println(Arrays.toString(array5));
```

**Output:**

```text
[20, 3, 300, 31, 35, 37, 4, 70]
```

### Rule

- `String` numbers are sorted **alphabetically (lexicographically)**, **not numerically**.
- Java compares the characters from **left to right**.
- If the first characters are the same, it compares the second character, then the third, and so on.
 
 ### String Numbers, and Text

```java
String[] array6 = {"3", "300", "20", "35", "37", "31", "4", "70", "ant", "Zebra"};

Arrays.sort(array6);

System.out.println(Arrays.toString(array6));
```

**Output:**

```text
[20, 3, 300, 31, 35, 37, 4, 70, "Zebra", "ant"]
```

### Searchin an Array
```
int numbers = {0, 2, 4,6,8};
Arrays.binarySearch(numbers, 8) -> output -> 4 (which is index of 8)

**The array should be already in the sorted form in order to use this method **


Arrays.binarySearch(numbers, 7) - > 7th value should be in element  4th index if available and since it is not available -4-1 (we always negate the value 4 and also add a -1 to it) -> -5

** Also if the array is not sorted it will give you unpredicatable results **

```


### Multi -Dimensional Arrays

```
dataType[][][nth] arrayName = new dataType [][][nth];

        OR
dataType arrayName[][][nth] = new dataType [][][nth];


int[][] nums = new int [3][4];
//-> [3] -> count of array(row)
//-> [4] -> count of element each array(column)


int[][] nums ={
    {1,2,3,4},
    {4,5,7,8},
    {9,10,11,12}
}
```

#### 3D Arrays
```
int [][][] num = new int[3][3][4];
//-> [3] -> count of table
//-> [3] -> count of array(row)
//-> [4] -> count of element each array(column)

int[][][] nums ={
   {
        {1,2,3,4},
        {5,6,7,8},
        {9,10,11,12}
   },
   {
        {1,2,3,4},
        {5,6,7,8},
        {9,10,11,12}
   },
   {
        {1,2,3,4},
        {5,6,7,8},
        {9,10,11,12}
   }
}
```

*** 
KEY NOTE

int[][] twoDimensionalArray = new int[3][];

- you can specify like this too. the first bracket[3] is required other wise the compilation error
- the second bracket can be kept empty

- int[][] twoDimensionalArray = new int[3] -> this is invalid
```
System.out.println(Arrays.toString(twoDimensionalArray)) -> output -> 
[[1@8132], [13@91], [31231@]] /

To correctly display values use
System.out.println(Arrays.toString(twoDimensionalArray[0])) -> output->[0,1,2]


also
{} -> the culry braces used when defining an array indicate its a multi array.

int[] age = {
    {5,20},
    {25,26,27},
    {30}
}

//size can differ as you can see by defining like this.

if you write to print a three dimensional array
System.out.println(Arrays.toString(threeDimensionalArray))
output-> [[[c@342f11, 321@sqa]]] -> three square brackets indidcate is 3d.

you can access threed array like below
System.out.println(Arrays.toString(threeDimensionalArray[0])) -> return [[I@5acf9800, [I@4617c264]  
    //to print this use Arrays.deepToString(threeDimensionalArray[0]) //returns one 2d array

System.out.println(Arrays.toString(threeDimensionalArray[0][0])) -> return 2d array first column

System.out.println(Arrays.toString(threeDimensionalArray[0][0][0]))-> return int value in the first table, first row element 1.
```

***
KEY NOTE

- int age[] = new int[4] {20,25,30,35}; -> Compilation error cannot use int[4] together with an intializer list like {20,25,30,35} 



### Control Flow Statements.

- There are two types of decision making statements they are
    - if statement
        - if statement
        - if else statement
        - if else if ladder statement
        - nested statement
    - Switch Statements

```
//IF Statement
if statement condition ->
    if(condition){
        //
    }

//IF-ELSE STATEMENT
if(condition){

}else{

}

//IF-ELSE-IF STATEMENT
if(condition){
    
}else if(condition){

}else{ -> last else is optional

}

//Nested if statement
if(condition1){
    if(condition2){

    }
}
```

### Boolean in an `if` Statement

```diff
+ boolean control = true;

+ // Recommended: checks if control is true
+ if (control) {
+
+ }

+ // Also works, but unnecessary
+ if (control == true) {
+
+ }

+ // Checks if control is false
+ if (control != true) {
+
+ }

+ // Recommended: checks if control is false
+ if (!control) {
+
+ }


```

***
If the code you gonna write in if or else block consist of 1 line then you do not need curly braces for example:
```

if(age>10)
System.out.println("Age is Greater than 10"); // no need curly braces
System.out.println("Test");// This gets printed regardless its outside if block.
```

### Ternary Operator
- Its represented by symbol ?:
- used to evaluiate boolean expression and considered as short hand for if statement
- example -> result = condition? code1:code2;

### Switch Statements
## Switch Statement

- A **`switch`** statement is used to execute different blocks of code based on the value of an expression.
- It compares the switch expression with each `case` value. If a match is found, that case is executed.
- Similar to an **`if-else if`** statement, but easier to read when checking multiple fixed values.

### Supported Types (Oracle Java SE 21 - 1Z0-830)

- Primitive types:
  - `byte`
  - `short`
  - `char`
  - `int`
- Wrapper classes:
  - `Byte`
  - `Short`
  - `Character`
  - `Integer`
- Other types:
  - `String`
  - `enum`

> **Not supported:** `long`, `Long`, `float`, `Float`, `double`, `Double`, `boolean`, `Boolean`

### Case Values

- A `switch` can have **one or many `case` values**.
- Every `case` value **must be the same type** as the switch expression.
- A `case` value must be a **literal** (e.g., `1`, `'A'`, `"Java"`) or a **constant** (`final` variable).
- A normal variable **cannot** be used as a `case` value.
- Every `case` value **must be unique**. Duplicate case values cause a compile-time error.
- A `switch` can also have a **`default`** case, which executes if no case matches. It is optional.
- Also if the variable in the switch expression is not initalized. (if it is not literal we use variable right we need to make sure the variable is intialized.)


### `break` Statement

- Each `case` can have a **`break`** statement.
- The `break` statement is **optional**.
- If `break` is present, the switch **terminates immediately** after executing the matched case.
- If `break` is omitted, Java **continues executing the following cases**, even if they do not match. This behavior is called **fall-through**.

### Every `case` value must be the same type as the `switch` expression.

The data type of every `case` value must match the data type of the `switch` expression.

```java
int day = 2;

switch (day) {
    case 1:
        break;
    case 2:
        break;
}
```

✅ Correct because both `day` and the `case` values are `int`.

---

### A `case` value must be a literal or a constant.

A `case` value must be:
- a **literal** (a value written directly in the code), or
- a **constant** (`final` variable).

Examples of literals:

```java
case 1:
case 'A':
case "Java":
```

Example of a constant:

```java
final int MONDAY = 1;

switch (day) {
    case MONDAY:
        break;
}
```

---

### A normal variable cannot be used as a `case` value.

A normal variable can change while the program is running, so Java does **not** allow it as a `case` value.

❌ Invalid

```java
int value = 2;

switch (day) {
    case value:      // Compile-time error
}
```

✅ Valid

```java
final int VALUE = 2;

switch (day) {
    case VALUE:
        break;
}
```

---

### Every `case` value must be unique.

Each `case` value can appear **only once** in a `switch`.

❌ Invalid

```java
switch (day) {
    case 1:
        break;

    case 1:      // Compile-time error
        break;
}
```

✅ Valid

```java
switch (day) {
    case 1:
        break;

    case 2:
        break;
}
```
```
Also need to make sure the switch statement expression variable is intialzied. 

        int test ;
        final int VALUE = 4;

        switch (test) {// compilation error since test is not intialzied.
            case VALUE:      
                System.out.println("c");
        }


```

```
You can define switch statements like

int day = 6;
switch(day){
    case 5:
        System.out.println("Thursday");
        break;
    case 6:
        System.out.println("Friday");
        break;
    default:
        System.out.println("Sunday");
        break;
}

//you can have default any where you want. and also you can have break for default too .

or

switch(day){
    case 1: case 2 : case 3 :
        System.out.println("weekday");
    case 6: case 7:
        System.out.println("Weekend");
        break;
}



// you can also have if statement inside switch

switch(day){
     case 1: case 2 : case 3 :
        if(day==3){
            System.out.println("hey");
        }else{
            System.out.println("yo");
        }
        break;
    case 6: case7: 
        System.out.println("Weekend");
        break;
}
```
---

//To get a char from user we use - > Char Operator = input.next().chartAt(0);

- next() -> just reads the next word
- nextLine() -> reads the entire line


---
- break -> to exist switch statement
- return -> this keyword is used to exit a method(what ever defined method). if it is defined under public static void main(String[ args]) main method then it simply termiantes the main method.


## Loop Statements

### Structure of `for` Loop

```java
for(int i = 0; i <= 10; i++){

}
```

or

```java
for(int i = 0; i <= 10;){

    i++;
}
```

or

```java
for(int i = 0; i <= 10;){   // infinite loop.

}
```

or

```java
for(int a = 10, b = 1; a < 5 && b < 11; a++, b++){

}
```
 
or

```diff
- for(int a = 10, double b = 1; a < 5 && b < 11; a++, b++){
-
- }
```

❌ Invalid since variables declared together in the `for` loop initialization must have the same data type.

### Nested Loop

```
for(int x =1; x<4 ; x++){
    for(int y=0; y<= 5 ; y++>){
        System.out.println("x" + x + "Y" + y); //ouput-> 
  output               x - 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3 4 4 4 4 4 4 
  ouput                y - 0 1 2 3 4 5 0 1 2 3 4 5 0 1 2 3 4 5 0 1 2 3 4 5
    }
}
```

```
for(;;){ -> this is valid loop. (this is considered infinite loop)

}
```


## Enhanced For Loop
- for each is like a for loop to work with Arrays and collections. It is an enhanced form of for loop

    ```
        int [] nums = {-3, 4, 6, 7, 10};
        for(int eachNum : nums){
            //loop body
        }

    ```

- if you need to use the index of an array inside this loop that means you need to use for loop instead of for each.


```

String movie = "Titanic";
for(char eachLetter : movie.toCharArray()){
    System.out.println(eachLetter);
}

```

## While Loop
```
int i  =  0;
while(i<5){
    System.out.println(i);
    i++;
}



Example - >
String[] animals = {"cat", "Dog", "Horse", "Cow"};
int i = 0;
while(i <animals.length>){
    System.out.println(animals[i])
}


```
- You can have a while loop inside a if condition or else condition

###   Control Statement Nesting Summary

Any control statement can be placed inside another control statement. ✅

Possible:
- `if` inside `switch` / `for` / `while` / `do-while`
- `switch` inside `if` / `for` / `while` / `do-while`
- `for` inside `if` / `switch` / `while` / `do-while`
- `while` inside `if` / `switch` / `for` / `do-while`
- `do-while` inside `if` / `switch` / `for` / `while`

Not possible:
- No restrictions on nesting valid control statements. ❌


## Infinite Loop

### Using Constant `true`

```java
while(true){
    System.out.println("Hello World");
}

System.out.println("Loop Ended"); // ❌ Unreachable code
```

- Since `true` is a constant value, the loop will never end.
- Any code after the loop is unreachable, so the compiler gives an error.

---

### Using a Normal Boolean Variable

```java
boolean isCheck = true;

while(isCheck){
    System.out.println("Hello World");
}

System.out.println("Loop Ended"); // ✅ Valid
```

- Since `isCheck` is a normal variable, its value can change during execution.
- The compiler cannot guarantee that the loop will run forever.
- Therefore, code after the loop is considered reachable.

---

### Using a `final` Boolean Variable

```java
final boolean isCheck = true;

while(isCheck){
    System.out.println("Hello World");
}

System.out.println("Loop Ended"); // ❌ Unreachable code
```

- `final` variables cannot change.
- The compiler knows `isCheck` will always be `true`.
- Therefore, the loop is treated as an infinite loop.

---

### Important Rule

- Constant conditions (`true`, `false`, `final` variables) are evaluated at compile time.
- Normal variables are evaluated at runtime.

---

### `while(false)` Example

```java
while(false){
    System.out.println("Kavi"); // ❌ Unreachable code
}
```

- The compiler knows the loop body will never execute.
- Therefore, the code inside the loop is unreachable.

---

### Exceptions

Using `break`, `continue`, or `return` can change the flow, so Java may allow the code.

Example:

```java
while(true){

    break;
}

System.out.println("Loop Ended"); // ✅ Reachable
```

###
To Generate a Random numer you can use the Random Method

```
Random random = new Random();
random.nextInt(5,100); //Generate a number between 5 and 99 . (100 -1 );
```

### Do-While Loop
```
int i = 0;
do{
    System.out.println(i);
    i++;
}while(i<5);  //semicolon is needed

Do while loop gurantess the code execute at least once before the condition is evaluated.


```
# Break, Continue, Return

## 1. break

- `break` is used to **terminate (stop) the current loop or switch statement**.
- After `break`, control moves to the statement after the loop or switch.

## Where can we use `break`?

✅ Can be used inside:
- `for` loop
- `while` loop
- `do-while` loop
- `switch` statement

❌ Cannot be used directly inside:
- `if` statement
- method

**Note:**
- An `if` statement itself cannot use `break`.
- But if the `if` statement is **inside a loop or switch**, then `break` can be used.


## Example: break in switch

```java
int a = 0;

switch(a){

    case 0:
        System.out.println("Hi");

    case 1:
        System.out.println("Hello");
        break;
}
```

### Output:

```
Hi
Hello
```
## Example: break in loop

```java
String[] animals = {"Monkey", "Bee", "Cat", "Dog", "Cow"};

for(String animal : animals){

    if(animal == "Dog"){
        break;
    }

    System.out.println(animal);
}
```

### Output:

```
Monkey
Bee
Cat
```

# 2. continue

- `continue` is used to **skip the current iteration of a loop**.
- It does not terminate the loop.
- The loop continues with the next iteration.

## Where can we use `continue`?

✅ Can be used inside:
- `for` loop
- `while` loop
- `do-while` loop

❌ Cannot be used directly inside:
- method
- switch statement
- if 

**Note:**
- `continue` is usually used inside an `if` statement that is inside a loop.

Example:

```java
for(int i = 1; i <= 5; i++){

    if(i == 3){
        continue;
    }

    System.out.println(i);
}
```

### Output:

```
1
2
4
5
```
 
---

# 3. return

- `return` is used to **exit from the current method**.
- Control is returned back to the method that called the current method.

## Where can we use `return`?

✅ Can be used inside:
- Any method

It can be used:
- Directly inside a method
- Inside an `if` statement
- Inside a loop
- Inside Switch statemnt

Example:

```java
public static void checkAge(int age){

    if(age < 18){
        return;
    }

    System.out.println("Adult");
}
```

If:

```java
checkAge(15);
```

Output:

```
(no output)
```

### Explanation:

- When `age < 18`, `return` exits the method immediately.
- The remaining code inside the method will not execute.



# Compilation Error Example

```java
int age = 50;
age--;

if(age == 40){

    break;
}
```
 - ❌ **Compilation Error:** `break` statement cannot be used outside a loop or switch statement.

### Explanation:

- The `if` statement is not a loop or switch.
- Therefore, there is nothing for `break` to terminate.

---
 

# return with switch statement

- `return` inside a `switch` exits the **entire method**.
- If all possible paths return, the code after the switch becomes unreachable.

## Example: Unreachable code

```java
public static void checkDay(int day){

    switch(day){

        case 1:
            System.out.println("Monday");
            return;

        case 2:
            System.out.println("Tuesday");
            return;

        default:
            System.out.println("Invalid day");
            return;
    }

    System.out.println("End of method"); // ❌ Unreachable code
}
```

❌ Compilation Error:

```
unreachable statement
```

### Why?

- `day == 1` → returns from method
- `day == 2` → returns from method
- Any other value → `default` returns from method

So there is no possible path to reach:

```java
System.out.println("End of method");
```

---

## Example: Reachable code (default does not return)

```java
public static void checkDay(int day){

    switch(day){

        case 1:
            System.out.println("Monday");
            return;

        case 2:
            System.out.println("Tuesday");
            return;

        default:
            System.out.println("Invalid day");
    }

    System.out.println("End of method");
}
```

### Output:

For:

```java
checkDay(1);
```

Output:

```
Monday
```

For:

```java
checkDay(5);
```

Output:

```
Invalid day
End of method
```

### Why?

- `case 1` and `case 2` exit the method using `return`.
- `default` does not return, so execution continues after the switch.

Therefore:

```java
System.out.println("End of method");
```

is reachable.


## continue causing infinite loop

```java
String[] animals = {"Monkey", "Bee", "cow", "Dog", "cow"};

int index = 0;
String animal;

while(index < animals.length){

    animal = animals[index];

    if(animal == "cow"){
        continue;
    }

    System.out.println(animal);
    index++;
}
```

### Result:

```
Monkey
Bee
```

Then it creates an **infinite loop**.

### Why?

- When `animal == "cow"`, `continue` skips `index++`.
- `index` does not increase.
- The loop keeps checking the same `"cow"` again and again.

```
continue → skips remaining code in the current iteration
```

 

## Labelled Loops

- A **label** gives a name to a loop.
- It allows `break` or `continue` to control a specific outer or inner loop.

## Example: Labelled outer and inner for loops

```java
outer:
for(int i = 1; i <= 3; i++){

    inner:
    for(int j = 1; j <= 3; j++){

        if(i == 2 && j == 2){
            break outer;
        }

        System.out.println(i + " " + j);
    }
}
```

### Output:

```
1 1
1 2
1 3
2 1
```

### Explanation:

- `outer:` is the label for the outer `for` loop.
- `inner:` is the label for the inner `for` loop.
- `break outer;` terminates the **outer loop completely**.
- Without the label, `break` would only exit the inner loop.


# Methods
- A method is a set of statements that perform an operation. Method is also known as procedure or function
- Main advantage of method is the code resuability.
- void means no return type.
- access modifies -> public, private, protected and default (if you do not specify anything then it is considered default).
- NOTE- FOR DEFAULT DO NOT SPECIFY ANYTHING (for default you have to keep empty . if u add the word ``default`` then it is compilation error. you cannot add that keyword.)
- braces are required for method for example
    ```
        void start(){
        }
    ```

```
You can specify method in either way:

public static void test(){}
        or 
static public void test(){}


```

```diff
- void static public test() {} // not valid
```

- Also do remember you can only use one access modfier like public or private or default or protected but you can use both final and static for example
    - final static public test(){} -> valid

- one access modifier and one return type is allowed.

```
public void test(){
    return; // this is valid since it doesnt return anything you can write like return; but its unnecessary.
}

```

- Method names cannot start with number. it can only start with character upper or lower case and then _ (underscore) and dollar sign . subsqeuent can be cahracters, special character,n umbers. also method name cannot cotnain reserved words.


```diff
- public void private(){} // not valid since private is reserved keyword
+ public void Private(){} //valid since it is capital P thus not a reserved keyword.
```

### Method calling

- Types of method
    - Standard libary method
    - user defined method.

- we cannot store void methods to a variable (since it doesnt return any value.)
- standard libary method example like Math class
    - Math.min(int a, int b) -> to find minimum value.

### Variable Arguments



```diff
public static void getPersonInfo(int age, boolen gender, string... hobbies){
    //method body
}

//string ... hobbies -> this can be 0 or more arguments. its an array of string.
````
- DO REMEMBER -> There can be at most one variable argument in a method and it should be the last parameter.

```diff
-  public static void main sumNumbers(String... numbers, int  nums){
    // method body
    //not valid it should be only one variable argument and it should be last.
}

this is only with vargs but with array fine for example
+ public static void sumNumbers(String[] numbers, int nums){
    //method body
}
```

- vargs -> flexible input
- array -> fixed structure.

- even if you do not pass any parameter for vargs its still valid. (it can be 0 or  more) . only for vargs. if method expects any other parameter alogn with varags for example lets say situation where it expects int and vargs. then u need to definitely pass the int value.


Integer.parseInt(stringNumber); -> to convert a string to integer.


### Method Overloading
- when a class has two or more methods with the same name but different parameters its known as method overloading.
```
int add (int x, int y){return x + y};
int add (int x, int y, int z ){return x +y +z};
double add (double x, double y){return x + y};
```
- The return type or the access modifier can be the same or different.
- changing the return type only or access modifier only doesnt matter. there has to be a change in the parameter.
    - It can be more parameters
    - different parameter data type etc.

- Note - to get the power of a number use - > Math.pow(num,power);
```diff
int add (int x, int y){return x + y;};
- double add (int x, int y){return (double) x + y;}; -> just chaning the return type will not help. it will throw a compilation error. (you need to change parameters. either increase or different data type.)

```