## GraphQL Subscriptions with Redis PubSub

In the last article, [Realtime GraphQL Subscriptions with Node.js](https://djaytechdiary.com/realtime-graphql-subscriptions-with-nodejs), we learned to create a real-time application using GraphQL subscriptions using In-built Pubsub mechanism by ***graphql-yoga***

In this article, we would be combining the graphql-subscriptions with the Redis publish/subscribe mechanism which is a recommended way to make our production applications more scalable.

**Pre-requisites**

- Install  [Node.js](https://nodejs.org/en/) 
- Install  [Redis server](https://github.com/desaijay315/graphql-node-express-app/wiki/Install-Redis-Server-using-Docker) 

### Graphql Redis Subscriptions

Graphql Redis Subscription allows you to connect your subscriptions manager to a Redis PubSub mechanism to support multiple subscription-manager instances.

### Let's Understand Redis PubSub Mechanism

Let's see the Redis way in which we can publish a channel and subscribe to the channel to listen to all the realtime messages.

- **Publish messages via a channel **- Create a channel and publish a new message

![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605254641342/OBo-eDjM8.png)

- Subscribe to the channel to listen to all the incoming messages

![2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605254751682/3z20chwD7.png)

- Now, send the publish command again and check the output for the subscribed channel in another terminal

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605254845824/NJ8KtQnvy.png)

- **Output** - Once subscribe the channel, you can listen to all the incoming messages publish to the channel

![4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605254865322/nHFtJsfGd.png)


### Integration with Graphql Redis Subscriptions

Kindly check out the codebase from our GitHub  [repo](https://github.com/desaijay315/graphql-node-express-app)  for further configuration of Redis pubsub mechanism

- Install dependencies

```sh
npm install --save graphql-redis-subscriptions ioredis
```

- **graphql-redis-subscriptions** - This package implements the PubSubEngine interface and can be easily integrated with Redis
- **ioredis** - It's a redis client for Node.js used by  [Ali-Baba](https://www.alibaba.com/)

- Insert below code to configure *redis and graphql-redis-subscriptions*

```js
import { RedisPubSub } from 'graphql-redis-subscriptions';
import Redis from 'ioredis';


const options = {
    host: process.env.REDIS_HOST || '127.0.0.1',
    port: process.env.REDIS_PORT ? process.env.REDIS_PORT : '6379',
    retryStrategy: times => {
        // reconnect after
        return Math.min(times * 50, 2000);
    }
};

const pubsub = new RedisPubSub({
    publisher: new Redis(options),
    subscriber: new Redis(options)
});
```

**Demo**

- Run our application 

```sh
npm run build
npm run start
```

- Visit  [localhost](http://localhost:8005/)

- Subscribe to the post channel -  from the graphql playground as well as redis-cli

**Graphql Playground 
**

![5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605256202043/QxD5FbTzN.png)

**redis-cli 
**

![6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605256275016/EGmlZp9P_.png)

- Now, let's create and delete the post from the playground, and check them out in both the graphql playground and redis-cli

**Create Post Mutation
**

![7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605256469760/ReVOAMjfI.png)

**Post Subscription with Payload
**

![8.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605256536835/CX9unDhnm.png)

**Verify the same on redis server
**

![10.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605256595791/awsC_l7Y4.png)


AND WE'RE DONE!


### Conclusion

Congratulation🥳 , If you made till here. Your entire application is now using Redis pubsub mechanism and it would make your application much more scalable.

I have created a repository of this app on my  [Github](https://github.com/desaijay315/graphql-node-express-app/tree/graphql-redis-subscriptions), please feel free to fork the code and try to run all the commands/code.

If you liked it please leave some love to show your support. Also, leave your responses below and reach out to me if you face any issues.