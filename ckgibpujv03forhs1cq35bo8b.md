## Callbacks, Promises & Async/Await in JavaScript

As the title states, ***Callbacks, Promises & Async/Await in JavaScript***, have always been a bit of a mystery to me. I have used them in my work and I was there too at one point. But believe me, you too will have to deal with it. I will try my best to explain this as simpler as it can be.

Taking our [Learn Modern JavaScript](https://hashnode.com/series/learn-modern-javascript-in-2020-go-from-beginner-to-expert-level-ckg55uvf506mxdcs1ezilfzl9) series ahead, we will learn some major concepts of JavaScript in this article such as synchronous, asynchronous, callbacks, promises & async/await.

JavaScript is single-threaded, blocking and synchronous. It is easy to think that JavaScript is only an imperative and object-oriented language, but it is actually a functional/imperative hybrid and an event-based language that provides object-oriented features via “prototypical inheritance”. 

### Synchronous vs Asynchronous

**Synchronous** means the execution happens in a single event. It will only execute the next task once the previous task is finished.
 
In **Asynchronous**, the execution will never wait to complete, instead, it will execute all the events in a single go and multiple events will be in progress, hence making JavaScript non-blocking. Task 2 will be performed even if task 1 isn't finished
 
![async and sync.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603203021588/-O7p9fwyu.png)
 
As you can see in the image, for synchronous execution the tasks will be executed one after the other. This means only one task will be in progress at a time and this will block the execution of the other tasks.
 
For **asynchronous** execution, all the tasks will be executed simultaneously, and it will be handled once the result of each task is available.
For instance, if we need to do some heavy lifting with expensive database queries and make an ajax request, execution for the next lines of code needs to wait until & unless it hasn’t returned any response from the DB server. This makes your application less responsive and your application might catch performance issues.

The way of writing such an application with CPU intensive and heavy database queries can be solved by using asynchronous code. It will indicate to the user some progress while running the I/O operations in the background.

```
//Synchronous code
const result = database.query("SELECT * FROM users");
console.log("query executed");
console.log("run another query");


// Asynchronous  code
database.query("SELECT * FROM users", function(result) {
    console.log("Query finished");
});
console.log("run another query");
```

**Synchronous code (Blocking)** - In the above example, the code will run sequentially and all the steps will be completed synchronously.

**Asynchronous code (Non-Blocking)** - The next code in the sequence will run and it won't the be blocked by the previous one.


### Callback
 
The callback function is a function which will get called after the execution of the first function and it will run the second function.

A function which can be passed as an argument to the higher-order functions and can return function. The argument passed as a function to the higher-order function is called as a callback.

```js
doSomething1(arg1, (err, data1) => {
  //some code...
  doSomethingAfter1(arg2, (err, data2) => {
    //some code
    doSomethingAfter2(arg3, (err, data2) => {
      //some code...
    });
  });
});
```

Because many of your tasks would run sequentially and you would end up running async actions like the one which you see in an above pyramid-like structure. This is called as **callback-hell**

```js
function getUser() {
  setTimeout(() => {
    const userids = [10, 20, 30, 40];
    console.log(userids);
    setTimeout(
      (id) => {
        const user = {
          name: "Fazle Rocks",
          age: 25,
        };
        console.log(
          `User ID : ${id} : User name : ${user.name}, User Age: ${user.age}`
        );
        setTimeout(
          (age) => {
            console.log(user);
          },
          1000,
          user.age
        );
      },
      1000,
      userids[3]
    );
  }, 1500);
}

getUser();

//Output:

(4) [10, 20, 30, 40]
User ID : 40 : User name : Fazle Rocks , User Age: 25
{name: "Fazle Rocks", age: 25}
```

In this example, we have three callbacks nested inside one another, which means three chained ajax calls to get the data from the server. It might have more and more chained callback functions and this might get out of hand. This concept is called “Callback hell” in JavaScript.

*When speaking in terms of Node.js, it is pretty famous for its event-driven and async. It can handle n number of executions at the same time in a single thread with a single core, but this is out of scope for this article*

### Promises
 
A promise is an object which keeps track of whether the asynchronous event has happened already or not and determines what happens after the event has occurred. 

Instead of providing a callback, a promise has its own methods (resolved or rejected),  which you call to tell the promise what will happen when it is successful or when it fails. 

It can be in three states, fulfilled, pending or rejected. It helps to catch all the errors that occurred after rejection or attach a callback to the handle of the fulfilled value.

![promises.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1603204307708/xY8qSPbGI.jpeg)
 
Before the event has happened the promise is pending, then after the event has happened it is called resolved. When the promise is successful and the result is available, then it will be fulfilled, but if it caught errors, then it will be called rejected.

```js
/*
Below sample example will be used to convert promises to async/await in further examples
*/

const getUsername = (userid) => {
  return new Promise((resolve, reject) => {
    setTimeout(
      (id) => {
        const user = {
          name: "John Doe",
          age: 25,
        };
        resolve({ user_id: id, username: user.name, age: user.age });
      },
      1500,
      userid
    );
  });
};

const getUserage = (userid) => {
  return new Promise((resolve, reject) => {
    setTimeout(
      (id) => {
        const user = {
          name: "John Doe",
          age: 25,
        };
        resolve({ user_id: id, user_age: user.age });
      },
      1500,
      userid
    );
  });
};

const getuserIds = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([10, 20, 30, 40]);
  }, 1500);
});

getuserIds
  .then((IDS) => {
    console.log(IDS);
    return getUsername(IDS[3]);
  })
  .then((userObj) => {
    console.log(userObj);
    return getUserage(userObj.user_id);
  })
  .then((userage) => {
    console.log(userage);
  })
  .catch((erorr) => {
    console.log(erorr);
  });


//output

Promise {<pending>}
(4) [10, 20, 30, 40]
{user_id: 40, username: "John Doe", age: 25}
{user_id: 40, user_age: 25}
```

In our above example, we are not nesting the callback inside the callback, we chain .then() calls together making it more readable and easier to follow. Every .then() should either return a new Promise or value or object which will be passed to the next .then() in the chain. 

Any error that occurs in the chain will stop further execution and an error will end up in the next .catch() in the chain. This is where the **magic** lies, where you are calling n numbers of Promise, chaining with .then(), but even a single error would be watched with .catch().

We have successfully tackled the callback hell using promises.

### Async/Await

- Async/Await is used to work with promises with asynchronous functions.
- Putting Async in front of the function expects it to return the promise. This means all async function has a callback
- Await can be used for single promises to get resolved or rejected and return the data or error
- Async/Await behaves like synchronous code execution
- There can be multiple awaits in a single async function
- We will be using try/catch construct, which makes async/await easy to handle synchronous and asynchronous code
- Async/Await helps you to deal with callback hell

```js
const userfunc = async () => {
         
  try {
      const id = await getuserIds;
      console.log(id);
      const getusername = await getUsername(id[3]);
      console.log(getusername.username);
      const getuserage = await getUserage(id[3]);
      console.log(getuserage.user_age);
  } catch (error) {
      console.log(error);          	 
  }
};

userfunc();

//Output:
(4) [10, 20, 30, 40]
Promise {<pending>}
John Doe
25
```
We have achieved the same thing which we were doing with promises using async / await syntax! Compare this code to the promise version, it almost looks the same!

### Conclusion

**In this article we have learned:**

- Difference between Synchronous vs Asynchronous
- Callbacks
- Promises & how it can help us to tackle callback hell
- Async/Await - How is uses promises and it's more readable and simple to understand

Let me know the scenarios in which you have found yourself using the above concepts and how you have made your code more readable. What would you do differently the next time?

Until then feel free to explore the world of JavaScript. Let me know if you face any issues in the comment section below.

You can also find me on  [Twitter](https://twitter.com/BeingJayDesai)