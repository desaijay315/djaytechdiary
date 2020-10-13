## What are Variables, Primitive and Reference Types in JavaScript?

As a JavaScript beginner, it is essential to know the basics of the language. This concept may come in handy for day to day programming in JavaScript. 

In this article, we will go through some of the key concepts in JavaScript that can be used as a reference for building JavaScript applications.


### Variables & Declaration

There are three possible keywords to define the variable. We have **var** since the beginning of the JavaScript. We also have **let and const** which were introduced in ES2015. After the update to the modern JavaScript, we get fully supported let and const in the browser.

** Rules and Conventions for variables**

1.	Cannot start with a number
2.	Can include letters, numbers, underscore(_), a dollar sign ($)
3.	Can start a variable with a $ sign, but not with numbers, but assigning with a $ sign is not recommended, until and unless you are using jquery
4.	Can have CamelCasing, underscore 

**Let's quickly look at the examples and usage**

1.	Initialize the variable and assign the variable with some value
2.	Assign the value to the variable without initializing the variable
3.	Variable with CamelCasing and underscore


**Example:**
    
```js 
//usage of var

//Initialize the variable and assign the variable with some value
var name;
name = 'Hashnode';
console.log(name); //Hashnode

//Reassign it with different value
name = 'google';
console.log(name); //google

//Assign the value to the variable without initializing the variable
name = 'Hashnode';
console.log(name); //Hashnode
  
//Variable with Camel casing and underscore
name = 'Hashnode';
console.log(name); //Hashnode
my_name = 'Hashnode';
console.log(my_name); Hashnode
``` 


**let** 

**let** works exactly the same way as a variable. You can initialize, create the variable, assign the value, and reassign the value

**Example:
**
```js
//usage of let  
//initialize the variable
let companyname;
companyname = 'Hashnode';
console.log(companyname); //Hashnode
//re-assign the variable with a different value
companyname = 'xyz';
console.log(companyname); //xyz
``` 

**const**

**const** stands for constant. This means we cannot change the values and the values cannot be reassigned

**Example 1:**

```js
//initialize the variable
const companyname = 'Hashnode';
companyname = 'xyz';
console.log(companyname); //Uncaught TypeError: Assignment to constant variable. 
//You cannot re-assign the const variable with another value
``` 
**Example 2:**


```js
const person = {
    fullname:'johndoe',
    age: 30
}

person.fullname = 'Sandeep';

console.log(person); // This change the person name to Sandeep, but the object still remains the same

Output:
{fullname: "Sandeep", age: 50}

person = {
fullname:'johndoe',
     age: 30
}; 
console.log(person.fullname); //Uncaught TypeError: Assignment to constant variable.
``` 

**Example 3:**

- If you want the object to be immutable, you can use **Object.freeze()** method to freeze the object.

```js
const person = Object.freeze({fullname:'johndoe',
       age: 50});
person.age = 100; // TypeError

``` 

**Example 4:**

- let us consider the array of numbers


```js
const numbers = [1,2,3,4,5,6];
console.log(numbers); //[1,2,3,4,5]

numbers = [1,2,3,4,5,6,7];
console.log(numbers); //Uncaught TypeError: Assignment to constant variable. 

numbers = []; //Uncaught TypeError: Assignment to constant variable.

``` 

***Note about hoisting:***

According to hoisting, let and const are hoisted. The difference between var, let and const in the hoisting point is the initialization.

var get initialized with undefined but let and const stay uninitialized that lead to **ReferenceError** when you try to access them before the declaration.


### Primitive vs Reference Types

**Primitive Data Type**

- They are stored on the stack, directly in the location the variable access.

**JavaScript has 6 primitive data types:**

1.	String
2.	Number
3.	Boolean
4.	Null
5.	Undefined
6.	Symbols(ES6)
7.      Object (a complex data type)

**Example:**

```js
//string
const companyname = 'Hashnode';
 
//number
const age = 28;

//boolean
const isSingle = true;

//null
const aircraft = null;

//undefined
let firstname;

//symbol
const sym = Symbol()

//Javascript defined null as an empty object
let obj = null;

console.log(typeof companyname); //string
console.log(typeof age); //number
console.log(typeof isSingle); //boolean
console.log(typeof aircraft); //null
console.log(typeof firstname); //undefined
console.log(typeof sym); //symbol
console.log(typeof obj); // object
```

### Reference Data Types

It is a pointer to the location in memory and is accessed by reference and not by value. Objects are stored on a heap, which will be dynamic in nature.

**Reference data types / Objects:**

1.	Arrays
2.	Object Literals
3.	Functions
4.	Dates

Reference types return as objects.

**Example:
**

```js
const person = {
  name: 'jay'
};
console.log(typeof person); //Object

const numbers = [1,2,3,4];  
console.log(typeof numbers); // Object 
```

*Note: Technically, arrays are objects*

### Handling the alteration of the original objects

For Example, Consider below object:

```
let dog1 = {
  name: 'tiger',
  breed: 'lab'
};
let dog2 = dog1;
dog1.age = 10;
console.log(dog1.age);// 10
console.log(dog2.age);// 10
```
From the above example, we can see how we altered the original object permanently. To prevent such mutations, we need to create a new reference to the new object, in a way where each variable will have its own object.

```
let dog1 = {
  name: 'tiger',
  breed: 'lab'
}

let dog2 = {...dog1}; //we are using spread operator 

dog1.age = 10;

console.log(dog1.age); // 10
console.log(dog2.age); // undefined
```

### Type Conversion

Most other languages like C# and Java have statically typed languages. JavaScript is a dynamically typed language, it can hold multiple data types and does not need to define any types. It can hold the string and a number for the same variable in the code without getting any issues or errors. 

There are technologies which can turn JavaScript to statically typed language, like TypeScript.

You may face some cases, where you will need to convert the variable type from string to integer, or integer to string, or boolean to string.

**Examples: **

```js
//type Conversion

//bool to String
let released = true;
released = String(true);
console.log(released); // true
console.log(released.length); //4

//using toString()
released = (true).toString()
console.log(released); //true
console.log(released.length); //4

//number to string
val = String(1234)
console.log(val); //1234
console.log(val.length); //4

//string to number
num = Number('8')
console.log(num); //8

//bool to number
num = Number(true);
console.log(num); //1
num = Number(false);
console.log(num); //0

//addition example -  In this example, num2 is converted to the string while addition operation and concatenation is performed

num1 = String(1)
num2 = 7

add  = num1 + num2;
console.log(add); // 17
console.log(typeof add); //string
``` 

### Conclusion

In this article, we saw some of the essential concepts of JavaScript which are very important for any developer to begin with the JavaScript language. I hope you now have a good understanding of these important concepts.

In the  [next article](https://djaytechdiary.com/javascript-essentials-string-methods-scopes-and-switches) , we shall discuss more concepts of JavaScript, until then feel free to explore the world of JavaScript.