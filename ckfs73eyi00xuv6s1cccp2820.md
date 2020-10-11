## DevOps 101 - Dockerizing your Next.js application - Part 2

Next.js is the powerful react framework that solves different problems like Server Side Rendering, Pre-rendering, SEO, etc. It is very straightforward to implement it in any React Projects.

Advantages of using Next.js in your next project - 

- Built-in CSS and Sass support
- Automatic Static Optimization
- Client-side routing with optimized prefetching
- Page based routing system (with support to custom routing)
- Development Environment supports hot module reloading
- Trusted and used by big companies of the world


These are some reasons why I love Next.js. If you want to learn more about Next.js, visit  [here](https://nextjs.org/learn/basics/create-nextjs-app)
  

**Docker** 

Docker has grown rapidly in popularity and has been adopted by many companies and teams. Docker helps to speed up this process by making it easier to link together small, independent services locally. 

In this article, we would dockerize our Next.js application by creating a customer server on our local machine with a production environment. With the help of our CI/CD pipeline, we would be deploying this project on the Kubernetes.

If you want to learn more about docker, here you go:

%[https://djaytechdiary.com/what-is-docker-and-why-it-is-so-popular]

This article is a part of our devops series and here is part one from our series:

%[https://djaytechdiary.com/install-and-configure-jenkins-on-amazon-ami?guid=9aaa1535-0d09-4f7e-a348-0c754907f335&deviceId=cdc869e5-1ba3-41e0-be28-fdd1968008eb]

### Creating the Next.Js App


```
npx create-next-app kubernetes-next-app

``` 
**Configure the custom server for our Next.Js Application**

 - Install Express

```
yarn add express

```

- Create a server.js file in the root and insert the below code 

%[https://gist.github.com/desaijay315/809baebaf1324eccd3e85f90c7660ccb]

- Now, add the command **start_prod** in the script in your package.json file

%[https://gist.github.com/desaijay315/6ce94bf8b5a316b59693af9e838490f0]


### Dockerize your application

- Create the docker file and insert the code as shown below - 


%[https://gist.github.com/desaijay315/0c7a5e75f2d62672e16080a702b87f0a]

**Create a docker-compose file**


%[https://gist.github.com/desaijay315/b6946a87ae98848d72d90ed4f4e61743]


**Run docker-compose up --build
**

*You can now visit, localhost:3000 and see your application running!
*

![welcome to next.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601636129031/Dv1DZdMcO.png)

***Conclusion***

In this article, we have learned how to deploy the Next.Js app with a custom server and how can we dockerize the next.js application. The similar steps can be applied to any Node.js, React.js, angular or Vue js applications. Please feel free to implement on your own.


Feel Free to reach out to me if you face any issues on  [Twitter](https://twitter.com/beingjaydesai) 

Check out my  [LinkedIn](https://www.linkedin.com/in/iamjaydesai)  | See my  [GitHub](https://github.com/desaijay315)  






