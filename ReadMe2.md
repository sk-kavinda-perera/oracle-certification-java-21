- Alternatively you can intialzie like 
    - nums[1] = 5; //2 element has value 5
    - nums[2] = 6; // 3 element has value 6

Size is required during intialization of an array
```diff
+ int [] numArray = new int[5]; //size is required during intialization

```

```
To print the array length
  - System.out.println(numArray.length);

To print Array numbers
 - System.out.println(Array.toString(numArray));

If you assign an element for over the size or negative index you will get ArrayIndexOutOfBoundException which is a runtime error. but during compilation it doesnt throw.

```