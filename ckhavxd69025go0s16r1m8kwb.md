## Realtime GraphQL Subscriptions with Node.js

In the last article,  [*Building APIs with GraphQL*](https://djaytechdiary.com/building-apis-with-graphql-nodejs-and-express), we learned how to generate mutations, build APIs with GraphQL server using Node.Js by building a real-world application.

In this article, we will learn GraphQL Subscriptions on our real-world  [application](https://github.com/desaijay315/graphql-node-express-app). 

If you are still new to the GraphQL world, feel free to read our first article  [here](https://djaytechdiary.com/why-graphql-is-the-future-a-gentle-introduction-to-graphql).

### What are GraphQL Subscriptions?

- Subscriptions can bring real-time features to your apps. It means we can subscribe to data and get notified in real-time when changes occur.

- Subscriptions uses WebSockets that allows bidirectional real-time communication between the server and the client. This will sync the data between the client and the server.

*Before diving deep into the code, let us learn the basics of polling and pub-sub:
*

### Polling

- Polling is the method of calling an API repeatedly after a determined time delay. Latency can be problematic in systems with layers of polling. This inflates the delay of data well beyond what was originally intended.

### PubSub

- Publish/Subscribe mechanism works quite well to push the updates/notifications to the client and can handle event-driven system efficiently and at a scale.

In our application, we will be using the package called PubSub (imported from graphql-yoga) that lets you wire up GraphQL with a pubsub system to implement subscriptions in GraphQL.

You can access the complete code with the subscriptions implemented,  [here](https://github.com/desaijay315/graphql-node-express-app).

- Initialize **pubsub**

```js
import { GraphQLServer, PubSub } from 'graphql-yoga';

//creating a new instance of pubsub
const pubsub = new PubSub();
```

- **Context** - Contains custom data being passed through your resolver chain. This can be passed in as an object, or as a function.

- *To use the pubsub, we need to provide it inside the context so that it can be used inside any resolvers.*

```js
const server = new GraphQLServer({
    typeDefs: './src/schema.graphql',
    resolvers: {
        ...
        Subscription
        ...
    },
    context: {
        ...
        pubsub
        ...
    }
});
```

- **Create a Subscription.js file in the resolvers folder and insert the below code:**

```js
//create the file
touch src/resolvers/Subscription.js

//insert this code snippet
const Subscription = {
    post: {
        subscribe(parent, args, ctx, info) {
            return ctx.pubsub.asyncIterator('post')
        }
    },
    user: {
        subscribe(parent, args, ctx, info) {
            return ctx.pubsub.asyncIterator('user')
        }
    },
    comment: {
        subscribe(parent, args, ctx, info) {
            const post = ctx.data.posts.find((post) => post.id === args.post && post.published)

            if (!post) {
                throw new Error('Post does not exist!')
            }
            return ctx.pubsub.asyncIterator('comment')
        }
    }
}

export default Subscription;
```

- Here, we have created the Subscription type resolver using the ***pubsub.asyncIterator*** to map the event we need. The asyncIterator takes the channel name through which the event across the app will be mapped and recommendation here is to use the unique identifier/channel name.

- Let us create the custom type definition called Subscription. We will create the Post, User and Comment Subscription where the client will get the real-time notifications whenever new posts, users or comments are created, old ones are deleted, and updates occur.

```js
type Subscription {
        post: PostSubscriptionPayload!
        user: UserSubscriptionPayload!
}

type PostSubscriptionPayload {
        mutation: String!
        data: Post!
}

type UserSubscriptionPayload{
        mutation: String!
        data: User!
}
```

- **mutation**: This defines the type of mutation we will perform. We would be performing **CREATED, UPDATED, and DELETED** mutation

- **data**: This defines the data which results in the resolver type.

- Now, update the mutation logic to connect with the Subscription resolve and use pubsub to notify the client in realtime. We would take an example of creating post mutation and integrate with pubsub

```js
createPost(parent, args, { data, pubsub }, info) {
        const userExists = data.users.some((user) => user.id === args.author)

        if (!userExists) {
            throw new Error('User does not exist!')
        }

        //use this
        const post = {
            id: v4(),
            title: args.title,
            body: args.body,
            published: args.published,
            author: args.author
        }

        pubsub.publish('post', {
            post: {
                mutation: 'CREATED',
                data: post
            }
        });

        data.posts.push(post)
        return post

    }
```

- PubSub's **publish()** function will take the event name as the first parameter and it will publish the data to the subscribers as seen in the second param.

- **Mutation:** CREATED is the name given for when the new post is created, and accordingly the client will get notified of the new post.

- Now run the GraphQL server for createPost mutation, it will output as follows:

![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604910843845/A0LSEqkgl.png)


- Run the below subscription for the post created in our GraphQL server

```js
subscription{
    post{
        mutation
        data{
            id
            body
            title
            published
            author{
                id
                name
                email
            }
        }
    }
}
```

**Subscription for Post Created
**

![2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604910855909/ELo4mhJow.png)

**Delete Post Mutation
**


```js
deletePost(parent, args, { data, pubsub }, info) {
        const isPostExists = data.posts.findIndex((post) => post.id === args.id)
        if (isPostExists === -1) {
            throw new Error('Post does not exist!')
        }
        //splice will return the index of the removed items from the array object
        const [post] = data.posts.splice(isPostExists, 1)

        data.comments = data.comments.filter((comment) => comment.post !== args.id)

        if (post.published) {
            pubsub.publish('post', {
                post: {
                    mutation: 'DELETED',
                    data: post
                }
            })
        }

        return post
    }
```

**Subscription for Post Deleted
**

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604910863200/t-v9ibaT9.png)

- For the deletePost function, we first check whether the posts exists or not, and throw the exceptions if there is no post for related id

- **Mutation**: DELETED is the name given for when a post is deleted, and accordingly the client will get a notification once the post is deleted.


**Update Post Mutations
**

```js
updatePost(parent, args, { data, pubsub }, info) {
        //posts exists
        const post = data.posts.find((post) => post.id === args.id)

        if (!post) {
            throw new Error('Post does not exist!')
        }

        //user exists - In the real world app, consider this as a session id and validate against the database.
        const userExists = data.users.some((user) => user.id === args.author)

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

        pubsub.publish('post', {
            post: {
                mutation: 'UPDATED',
                data: post
            }
        });

        return post

    }
```

**Update Post — GraphQL Server
**

![4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604910879697/3htx-6BlT.png)

**Subscription for Post Updated
**

![5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604910883581/N_MDXJZcT.png)

**Demo**


![chrome-capture (2).gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1604917262301/XQDpuHAdi.gif)


### Conclusion

In this article, we learned GraphQL Subscription for real-world application and created a real-time feature to sync up client and server.

I have created a repository of this app on my  [Github](https://github.com/desaijay315/graphql-node-express-app), please feel free to fork the code and try to run all the commands/code.

If you liked it please leave some love to show your support. Also, leave your responses below and reach out to me if you face any issues.