## Map, Filter, Reduce and Template Literals in JavaScript

In the first & second part of this series of JavaScript, we took a look at some of the concepts like let and const, type conversion, primitive and reference types, switches and Hoisting.
  
In this article, we will go through some more concepts of JavaScript.

### Template Literals
 
- Template literals improve the code readability, look better, and provide features such as:
 

1. String interpolation
2. Embedded expressions
3. Multiline strings without hacks
4. String formatting
5. String tagging for safe HTML escaping, localization and more.
 
- It will result in the same as concatenating two strings.
 
**Example:
**
```js 
<html>
	<body></body>
	<script>
    	//template literals
    	const firstname = 'jay';
    	const lastname  = 'desai';
    	const age = 26;
    	const city = 'Mumbai';
    	const address = 'Kandivali';
    	let html;

    	html = `
        	<ul>
        	<li> Firstname: ${firstname} </li>
        	<li> Lastname: ${lastname}</li>
        	<li> Age: ${age} </li>
        	<li> City: ${city} </li>
        	<li> Address: ${address} </li>
        	</ul>
        	`;

    	document.body.innerHTML = html;
      
	</script>
</html>
 
/*
Output:
Firstname: jay
Lastname: desai
Age: 26
City: Mumbai
Address: Colaba
*/
 ```
 
### Object Literals
 
- We will not be going into much detail on object-oriented programming, but we will learn various ways of accessing the object and adding properties to it.
 
**There are two ways to add new properties to an object:
**

```js
let obj = { key1: "value1", key2: "value2" };
//Using dot notation:
obj.key3 = "value3";
 
//Using square bracket notation:
obj["key3"] = "value3";
 
//ES6 way to add the properties to the object:
Object.assign(obj, {key3: "value3"});
 
//Output for all above execution is same:
console.log(obj)
{key1: "value1", key2: "value2", key3: "value3"}
```

### Math object
 
- The Math object is used to perform mathematical operations on numbers.
 
```
//match object usage
pi = Math.PI;
euler_constant = Math.E;
round_val = Math.round(50.8);
ceil_val = Math.ceil(40.5);
floor_val = Math.floor(8.9);
max_val = Math.max(4, 5, 6, 7, 100, 200);
pow_val = Math.pow(4, 2);
sqrt_val = Math.sqrt(81);
abs_val = Math.abs(-100);
min_val = Math.min(2, 3, 4, 5, 0, -1);

console.log(pi); //3.141592653589793
console.log(euler_constant); //2.718281828459045
console.log(round_val); //51
console.log(ceil_val); //41
console.log(floor_val); //8
console.log(max_val); //200
console.log(pow_val); //16
console.log(sqrt_val); //9
console.log(abs_val); //100
console.log(min_val); // -1
```
 
### Date & Time
 
- Date and time are important concepts in programming. JavaScript has the date object inbuilt. Let’s explore by going through some examples.
  
**Examples:
**
   
```js
//Date and Time
//todays date
const today = new Date();
//day of the week
let day = today.getDay();
//get the current year
let year = today.getFullYear();
//get the current month
let month = today.getMonth();
//get the current hours
let hrs = today.getHours();
//get the current minutes
let min = today.getMinutes();
//get the current seconds
let sec = today.getSeconds();

console.log(today); //Wed Oct 14 2020 20:02:26 GMT+0530 (India Standard Time)
console.log(day); //3
console.log(year); //2020
console.log(month); //9
console.log(hrs); //20
console.log(min); //2
console.log(sec); //26

let date_ = new Date("October 14, 2020 20:02:26");
date_.setFullYear(2021);
console.log(date_.getFullYear());
// expected output: 2021
date_.setFullYear(0);
console.log(date_.getFullYear());
// expected output: 0

```
 
### Array & Array Methods
 
- The Array object is used to store multiple values in a single variable. We will be looking at creating the arrays and mutating them.
 
**Example:
**
 
- **Join Arrays - Using the concat method
**

``` 
//create some array
let  num1 = [1,2,3,4];
let  num2 = new Array(5,6,7,8);
let  concat_num1_num2 = num1.concat(num2);
console.log(concat_num1_num2); //[1,2,3,4,5,6,7,8]
```
 
- **Let’s create some more arrays**

```js
// Date and Time
//todays date
const today = new Date();
//create some more arrays
const numbers = [1, 2, 3, 4, 5, 6, 7];
const numbers2 = new Array(8, 9, 10, 11, 12);
const fruit = ["Apple", "Banana", "Orange", "Pear"];

// Get array length
let values;
values = numbers.length;
console.log(values); // 7
// Check if is array
values = Array.isArray(numbers);
console.log(values); //true
// Get single value
values = numbers[3];
console.log(values); //4

values = numbers[0];
console.log(values); //1

// Insert into array
numbers[2] = 100;
console.log(numbers); //(7) [1, 2, 100, 4, 5, 6, 7]
// Find index of value
values = numbers.indexOf(5);
console.log(values); //4
```
 
### Map
           
- .map() method exists on the array prototype. It utilizes the return value and returns a new array and the original array remains unchanged.
 
**Example:
**

```js
//.map() function usage
let numbers_array_original = [1, 2, 3, 4, 5, 6];
let new_number_array;

new_number_array = numbers_array_original.map((num) => {
  return num * 2;
});

console.log(new_number_array); // new array - [(2, 4, 6, 8, 10, 12)]
console.log(numbers_array_original); // original array unchanged - [(1, 2, 3, 4, 5, 6)];

``` 
  	
### Filter
 
- The filter() function will go through each element of the array and desired array output with sanitized values are expected.
 
**Example:
** 

```js
let numbers_array_original = [1, 2, 3, 4, 5, 6];
let new_number_array;

new_number_array = numbers_array_original
  .map((num) => {
    return num * 2;
  })
  .filter((data) => {
    return data !== 10 && data > 6;
  });

console.log(new_number_array); // [(8, 12)]
console.log(numbers_array_original); //(1, 2, 3, 4, 5, 6)];
```
      
   	
- Here the map() method will return the array with each element getting multiplied with 2 and the filter function will return the number array with value 10 and elements having a value greater than 6.
 
### Reduce
 
- When the transformation of an array is required, using a map would be more desirable as it is easy to read.
 
- Reduce works similar to map, but it adds more flexibility with the output we desire. If we wanted to use a map to transform our array into a new object, we couldn’t do it. In this case, we will have to use reduce.
 
- The first argument of the reduce function is the callback which is called accumulator, the second argument is the value of the array and the third argument is the index of the element of an array
 
**Example:
**
```js
//Using reduce in a similar way as we do for map:
let numbers_array_original = [1, 2, 3, 4, 5, 6];
let new_number_array;

new_number_array = numbers_array_original.reduce(
  (accumulator, value, index) => {
    accumulator[index] = value * 5;
    return accumulator;
  },
  []
);

console.log(new_number_array); //[(5, 10, 15, 20, 25, 30)]
console.log(numbers_array_original); //[(1, 2, 3, 4, 5, 6)];

```

- Convert the array into an object with reduce. Conversion to other data type is not possible with a map.

```js
new_number_array = numbers_array_original.reduce((accumulator, value, index) =>{
    accumulator[value] = true;
    return accumulator;
},{})

console.log(new_number_array); //{1: true, 2: true, 3: true, 4: true, 5: true, 6: true}
console.log(numbers_array_original); //[1, 2, 3, 4, 5, 6]
```

 
### Conclusion

I hope this post helped you understand JavaScript a little better. If you liked this article, please feel free to comment below and connect. Cheers!