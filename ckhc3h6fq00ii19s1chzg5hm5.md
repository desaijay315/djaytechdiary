## Build and Publish your First NPM package

### What is NPM?

NPM stands for Node Package Manager. It is an open-source project created in 2009 to help javascript developers easily manage and share the code in the form of packages. It helps developers to share their modules privately or publicly.


### What we need to build/publish?

This focus of this article is to build and publish a simple npm package in a few simple steps. We would be creating two validation functions which validate whether the URL provided as a parameter is a valid website URL or not. Our second function will validate whether the email is a valid email or not.

You will be able to add many more functionalities to our npm package and publish it to share it with the internal team members as well as publicly.


**Pre-requisites:
**

- Install Node.js
- Install NPM - When you install Node.js npm should be automatically get installed on your machine.


After installing node, You can check the version of node and npm by

```sh
npm -v
node -v
```

### Building our NPM package

- **Initialize the NPM**

```sh
#Create the project directory
mkdir website-validations

#goto the project directory
cd website-validations

#Initialise npm package
npm init
```

*NPM init command will ask several questions about a package for creating a package.json.  The file will automatically get created in your directory and you can check all the details are correct.
*

- **Package.json** is the most important file and without it, we won't be able to publish our code on an npm registry. 

- **main** key in package.json refers to the file(index.js) on which our code would be loaded and this pointing is useful when another package wants to refer our package.

![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604999902045/3CTEUVCVB.png)


- **Create index.js file into the project root folder and insert below code
**

```js
//create the file
touch index.js

//insert the code

//validates the URL 
function isValidWebsite(url) {
    let expression = /[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)?/gi;
    let regex = new RegExp(expression);
    if (url.match(regex)) {
        return true;
    }
    return false;
}

//validates the email
function validateEmail(email) {
    let emailReg = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    if (emailReg.test(email)) {
        return true;
    }
    return false;
}

module.exports = {
    validateEmail,
    isValidWebsite
}
```

### Publish our NPM package

- Signup to the  [npm registry](https://www.npmjs.com/)
- Login to the npm CLI 

```sh
npm login
```

![2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605000418696/p-Pe-8zHX.png)

- Now, run the below command to publish npm package

```sh
npm publish
```

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605000592329/WrEPHp1zK.png)

- Go to the npm dashboard to verify our package is published

![4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605000731461/7JvGTwoXq.png)


### Testing our NPM package

```sh
#Create the project directory
mkdir website-validations-tests

#goto the project directory
cd website-validations-tests

#Initialise npm package
npm init -y
```

- Install the package we published 

```sh
npm install website-validations
```

![5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605000904348/gsgMGO0SU.png)

- Create the file test.js and insert the below code

```js
const { isValidWebsite, validateEmail } = require('website-validations');

console.log(isValidWebsite('www'));
console.log(isValidWebsite('http://www.hashnode.com'));
console.log(validateEmail('yourid@.com'));
console.log(validateEmail('yourid@domain.com'));
```

- Run test.js file

```sh
node ./test.js
```

**Output**

![6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605001217508/MHFYadikz.png)

As seen in the above image, we have successfully installed our package, imported the function to be used and verified the website URL and email id.



AND WE'RE DONE!


### Conclusion

In this article, we have successfully built and published an npm package on the npm registry and stored the build artifacts.

I have created a GitHub  [repo](https://github.com/desaijay315/build-publish-npm-package) for the above code, please feel free to fork the code and try to run all the commands/code.

If you liked it please leave some love to show your support. Also, leave your responses below and reach out to me if you face any issues.