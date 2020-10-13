## String Methods, Scopes and Switches in Javascript

In our previous  [article](https://djaytechdiary.com/javascript-essentials-variables-primitive-reference-types), we saw some essential concepts of JavaScript. 

In this article, we will go through some more key concepts of JavaScript, in-built methods that can be used as a reference for building JavaScript applications.


### String Methods
 
- The string is used for storing and manipulating the text. We will see some of the string methods.
 
```js
const name = "joHn DoE";
const age = 30;
const address = "Electronic City Bangalore";
const fruits = "apple,banana,orange,watermelon";

let output;

// finding a string in string
output = name.indexOf("h");
console.log(output); //-1

output = name.lastIndexOf("D");
console.log(output); // 5

// Capitalization and lowercase
output = name.toUpperCase();
console.log(output); //JOHN DOE
output = name.toLowerCase();
console.log(output); //john doe

// Return a character with speified index with charAt() method
output = name.charAt("6");
console.log(output); //o

// Get last character of  the  string
output = name.trim().charAt(name.length - 1);
console.log(output); //E

// replace()
output = name.replace("joHn", "DoE");
console.log(output); //DoE DoE
output = name.replace("DoE", "joHn");
console.log(output); //joHn joHn

// includes() method will check whether the string includes the given string
output = address.toLocaleLowerCase().includes("delhi");
console.log(output); //true

// substring()  method does  not take the  negative values unlike slice() method
output = name.substring(1, 7);
console.log(output); //oHn Do

// slice()  method can take both positive and negative values as the param
output = name.slice(1, 5);
console.log(output); //oHn
output = name.slice(-3);
console.log(output); //DoE

// split()
output = address.split(" ");
console.log(output); //(4) ["Electronic", "City", "Bangalore"]
output = fruits.split(",");
console.log(output); //(4) ["apple", "banana", "orange", "watermelon"]
```

### Block scope with let & const
 
- Variables declared with var, let & const have the global scope and functional/local scope. Once defined globally **var** is available to use in the whole window and if defined in the function, can be used in the scope of the function.

**Example with var:
**

``` js
//global scope  
var name = 'fazlerocks'
   testfunc = () =>{
  	//local scope
   	var name = 'hey'
   	console.log(name); 
   }
   testfunc()
   console.log(name);
 
Output:
 
hey
fazlerocks
``` 
 
**Example with let and const
** 

```js
    
   //global scope for var,let,const
   var num1 = 500;
   let num2 = 600;
   const num3 = 700;

   numFunc = () => {
   // local/functional  scope for var,let,const
   	var num1 = 800;
   	let num2 = 900;
   	const num3 = 1000;
  	console.log("inside numFunc ", num1,num2,num3)
   }

   numFunc()

   console.log("outside numFunc ",num1,num2,num3)
 
Output:
inside numFunc  800 900 1000
outside numFunc  500 600 700
 ```
 
- As var can be used anywhere in the code, it may cause issues, as you may not know whether the variable is already declared. Let and const are the ES6 features. Let is an improved version of var.
 
- **let** is block scope. Any variable declared in the {} (curly) braces, can only be used inside that block.
 
```js
   var num1 = 500;
   let num2 = 600;
   const num3 = 700;

   if(1){
   	var num1 = 800;
   	let num2 = 900;
   	const num3 = 1000;
  	console.log("inside if ", num1,num2,num3)
   }

   console.log("outside if ",num1,num2,num3)
 
Output:
 
inside if  800 900 1000
outside if  800 600 700
``` 
 
- As you can see in the output the var declared variable num1 is assigned with the latest value, let and const declared values are blocked scope. Variables declared with let can be updated in a different scope. If you try and redefine the variable with let in the same scope, it will throw syntax errors. Variables declared with const can neither be updated nor re-defined.
 
 
```js
   let name = 'world';
   let name = 'helloworld';
   console.log(name); //Uncaught SyntaxError: Identifier 'name' has already been declared
 ```

### Scope Chain

```
var x = 10;

function foo() {
    var x = 100;

    function bar() {  
        console.log(x);
    }

    bar();
}

foo(); //100
```

- Here, the JavaScript first looks up the variable **x** inside the bar function, and it cannot find one in the scope of bar. 
- Then, it will find the varialbe in the outer execution context, and find the varialbe **x** inside the scope of foo and it stops the searching

### Switches
 
- A switch case evaluates the expression. A matched case will run until a break (or the end of the switch statement is found. 

- It basically gives you the ability to write options and it's a great piece of language which helps simplify your code if you’re repeating a lot of **if** statements
 
**Example:**

```js
let action= "";

if(action === "createUser"){
 //some api call or logic here...
 console.log("user data is created!")
}else if(action === "updateUser"){
  //some api call or logic here...
  console.log("user data is updated!");
}else if (action === "deleteUser"){
   //some api call or logic here...
  console.log("user data is deleted!");
} else {
  console.log("no action received!");
}
```
- You can write the much complicated **if-else statement** with a **switch statement ** as below, though for the smaller conditions you can definitely use if-else statements.
- Using switch statements for complex conditions looks more readable

```js
switch (action) {
  case "createUser":
    //some api call or logic here...
    console.log("user data is created!");
    break;
  case "updateUser":
    //some api call or logic here...
    console.log("User data is updated!");
    break;
  case "deleteUser":
    //some api call or logic here...
    console.log("user data is deleted!");
    break;
  default:
    console.log("please provide some action");
}
```

### Advanced Switch Use Case

 - If you ever need multiple cases in a switch statement, then you can use fallthrough or cascading feature of the switch statement.

```js
function weatherCheck(val) {
  var answer = "";
  switch( val ) {
    case 1: 
    case 2: 
    case 3:
      answer = "Windy";
      break;
    case 4: 
    case 5:
    case 6:
      answer = "Hurricanes";
      break;
    case 7: 
    case 8: 
    case 9:
      answer = "Droughts";
      break;
    default:
      answer = "Sunny?";
  } 
  return answer;  
}

weatherCheck(3); //Windy
```

### Hoisting
 
Hoisting is a JavaScript mechanism where functions and variables are moved to the top of their scope of execution.
 
**var** get initialized with undefined but **let and const** stay uninitialized that lead to **ReferenceError** when you try to access them before the declaration.
 
```js
console.log(data); // undefined
var  data;
data = 10;

//with let and const
console.log(companyname); //Uncaught ReferenceError: Cannot access 'companyname' before initialization
let companyname = "Hashnode";

console.log(name); //Uncaught ReferenceError: Cannot access 'name' before initialization
const name = "JayDesai";

```

 
### Conclusion
 
In this article, we saw some of the essential concepts of JavaScript which are very important for any developer to begin with the JavaScript language. I hope you now have a good understanding of these important concepts.
 
In the next article, we shall discuss more concepts of JavaScript, until then feel free to explore the world of JavaScript.