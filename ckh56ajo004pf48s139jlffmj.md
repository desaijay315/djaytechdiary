## Building APIs with GraphQL, Node.js and Express

In my previous article,  [*A Gentle Introduction to GraphQL*](https://djaytechdiary.com/why-graphql-is-the-future-a-gentle-introduction-to-graphql), we learned what is graphQL and how can we generate GraphQL queries.

In this article, we will learn:

- GraphQL Mutations 
- GraphQL Queries
- Create a blog application APIs where we would be able to create new user, post a blog, update a post, update a user, comment on the blog and

many more...  


### Mutations

There are three things which fall under the umbrella of GraphQL mutations and they are:

- Create (POST)
- Update (PUT/PATCH)
- Delete

The structure remains the same as queries, and the only change is we take the data from the variables for the fields which we need to create, change, and delete via the API request.


![image1.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1604560817781/LrVGMBUT1.jpeg)

This is an example of a blog application where we will initially create the users, their posts, and comments. We will also update their attributes and delete them too with mutations.

We will begin by adding new operations in the GraphQL type definitions (schemas), arguments, and resolver functions.

If you wanna check out the code without moving ahead, you can go to my Github repo  [here](https://github.com/desaijay315/graphql-node-express-app) 

- **Install dependencies:**

```sh
mkdir graphql_nodejs
npm init -y
npm install --save graphql-yoga uuid express

npm install --save-dev nodemon
```

- Create an index.js file in the root of our project folder ***graphql_nodejs***:

```sh
touch index.js
```

**Let's create some type Definitions**

```js
const typeDefs = `
   type Query {
        greeting(name: String): String!
        users(query: String):[User!]!
        getMyPosts: User!
        post(query: String):[Post!]!
    }

    type User{
        id: ID!
        name: String!
        email: String!
        age: Int
        posts:[Post!]!
    }

    type Post{
        id:ID!
        title:String!
        body:String!
        published:Boolean!
        author: User!
    }
`
```

### Dummy Data

Let's consider the user and the post array for some dummy data. You can consider any database or REST APIs endpoints, structure/transform the data and resolve for further usage with GraphQL.

```js
//array of users

const users = [{
    id: "1234",
    name: "jay desai",
    email: "jay@jay.com",
    age: 27
}, {
    id: "5678",
    name: "jhon",
    email: "jhon@jhon.com",
    age: 27
},
{
    id: "8910",
    name: "ram",
    email: "ram@ram.com",
    age: 27
}];

//array of posts
const posts = [{
    id: '123',
    title: 'my new blog',
    body: 'new blog body',
    published: true,
    author: '8910',
    commentId: '123'
}, {
    id: '456',
    title: 'blog 2',
    body: 'blog body 2',
    published: false,
    author: '1234',
    commentId: '456'
}, {
    id: '789',
    title: 'blog 3',
    body: 'blog body 3',
    published: true,
    author: '5678',
    commentId: '789'
}];


// comments on the posts
const comments = [
    {
        id: '222',
        body: 'This post is amazing',
        author: '1234',
        post: '789'
    }
]

```

### Creating Resolvers

Now, here comes the interesting part to create the resolvers. Resolvers are the functions that resolve a value for a type or field in a schema. Resolvers can return objects or scalars like Strings, Numbers, Booleans, etc. If an object is returned, execution continues to the next child field. If a scalar is returned (typically at a leaf node), execution completes.

### Query Resolvers

- Creating the Query resolvers to get all the users

```js
const resolvers = {
    Query: {
        users(parent, args, ctx, info) {
            if (!args.query) {
                return users
            }
            return users.filter((user) => {
                return user.name.toLowerCase().includes(args.query.toLowerCase())
            })
        }
    }
```

### Arguments

- **parent** - The object that contains the result returned from the resolver on the parent field.

- **args** - An object with the arguments passed into the field in the query.

- **ctx(context)** - This is an object shared by all resolvers in a particular query.

- **info** - It contains information about the execution state of the query, including the field name, a path to the field from the parent.


- Get all the posts

```js
posts(parent, args, ctx, info){
    if (!args.query) {
        return posts;
    }
    return posts.filter((post) => {
        const body = post.body.toLowerCase().includes(args.query.toLowerCase())
        const title = post.title.toLowerCase().includes(args.query.toLowerCase())
        return body || title;
    })
}
```

- Get the Users Posts & vice versa. 

```js
Post: {
    author(parent, args, ctx, info) {
        return users.find((user) => {
            return user.id === parent.author
        })
    }
},
User: {
    posts(parent, args, ctx, info) {
        return posts.filter((post) => {
            return post.author === parent.id
        })
    }
}
```

- Get a user profile and post related to a user

```js
getMyProfileData() {
    return {
        id: '1234',
        name: 'mike',
        email: 'a@a.com',
        age: 10
    }
}
```

### Mutation Resolvers

Now, let's create some mutations to create, update, and delete users/posts.

- Create a new post mutation

```js
createPost(parent, args, ctx, info) {
    const userExists = users.some((user) => user.id === args.author)

    if (!userExists) {
        throw new Error('User does not exist!')
    }

    //use this
    const post = {
        id: uuidv4(),
        title: args.title,
        body: args.body,
        published: args.published,
        author: args.author
    }


    posts.push(post)
    return post

}
```

- Create a new user mutation

```js
createNewUser(parent, args, ctx, info){
    const isEmailExists = users.some((user) => user.email === args.email)
    if (isEmailExists) {
        throw new Error('Email already Taken')
    }

    const user = {
        id: uuidv4(),
        name: args.name,
        email: args.email,
        age: args.age
    }
    users.push(user)

    return user
},
```

- Update a user mutation

```js
updateUser(parent, args, ctx, info) {
    const user = users.find((user) => user.id === args.id)
    if (!user) {
        throw new Error('User does not exist!')
    }

    if (typeof args.email === 'string') {
        const isEmailExists = db.users.some((user) => user.email === args.email)
        if (isEmailExists) {
            throw new Error('Email already Taken')
        }
        user.email = args.email
    }

    if (typeof args.name === 'string') {
        user.name = args.name
    }

    if (typeof args.age !== 'undefined') {
        user.age = args.age
    }

    return user
}
```

- Update a post mutation

```js
updatePost(parent, args, ctx, info) {
    //posts exists
    const post = posts.find((post) => post.id === args.id)

    if (!post) {
        throw new Error('Post does not exist!')
    }

    //user exists - In the real world app, consider this as a session id and validate against the database.
    const userExists = users.some((user) => user.id === args.author)

    if (!userExists) {
        throw new Error('User does not exist!')
    }

    if (typeof args.title === 'string') {
        post.title = args.title
    }

    if (typeof args.body === 'string') {
        post.body = args.body
    }

    if (typeof args.published === 'boolean') {
        post.age = args.published
    }

    return post

}
```

- Delete a user mutation

```js
deleteUser(parent, args, ctx, info) {
    const isUserExists = users.findIndex((user) => user.id === args.author)

    if (!isUserExists) {
        throw new Error('User does not exist!')
    }
    //splice will return the removed items from the array object
    const userdeleted = users.splice(isUserExists, 1)
    return userdeleted[0]
}
```

- Update the package.json

```js
"scripts": {
    "start": "nodemon src/index.js"
  }
```

- Run the application

```js
const server = new GraphQLServer({
    typeDefs,
    resolvers
})

// since the property name matches up with a variable with the same name, I am using object property shorthand

const options = {
    port: process.env.PORT || 3000;
}

server.start(options, ({ port }) =>
    console.log(
        `Server started, listening on port ${port} for incoming requests.`,
    ),
)
```

**Demo**

![chrome-capture (1).gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1604596297253/vE4KHeAIf.gif)


### Conclusion

In this article, we have learned the practical way of implementing GraphQL APIs with a GraphQL server in action.

The entire application with many other features is on my  [Github](https://github.com/desaijay315/graphql-node-express-app) repo, please feel free to fork the code and try to run all the commands/code as mentioned above.
 
I hope this article was helpful and hope you were able to understand how APIs are built with GraphQL. 

> 
If you liked it please leave some hearts to show your support. Also, leave your responses below and reach out to me if you face any issues.