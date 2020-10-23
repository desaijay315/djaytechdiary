## MongoDB - A Beginner's Guide

Before jumping into the MongoDB, and the features they provide, we instead will start by knowing the history of the databases, advantages and disadvantages of traditional databases.

In this  [series](https://hashnode.com/series/learn-modern-databases-go-from-beginner-to-expert-level-ckgm7e2ph0a82nzs14mnacymh), we learn MongoDB, its advanced concepts, techniques, best practices and many more...

### Historical context on databases

Human beings began to store information very long ago. In ancient times, elaborate database systems were developed by government offices, libraries, hospitals and some of those principles are still being used today.

In the early 1990s, a number of the tools very developed after the shakeout of the database industry and these included Oracle Developer, PowerBuilder, VB, ODBC, Excel/Access. 

### Traditional File-Based System

In a file-based system, records were treated as the discrete objects which could be placed in directories(folders). The application programs performed service for the end-users such as the production of the reports. Each program can manage its own data.

**Disadvantages of File-Based System**

- Duplication of data
- Data duplication, resulting in data redundancy and inconsistency
- Difficulty in accessing and querying data since a new program has to be
written to carry out each new task
- Very less flexibility in meeting changing needs

![file based system (5).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603477145419/stI-OXwf9.png)

### Databases

A software system that enables users to define, create and maintain the database and provides controlled access to the containers of data.

**Advantages of using database systems**

- Database solves all the problems of file-based systems
- Data independence
- Data consistency, lack of data redundancy
- Concurrent access, backup, transactions, recovery
- Provides better accuracy
- Size - You may have thousands or millions of rows 

Different Relational Database Management Systems(RDBS) are Oracle, MySQL, PostgreSQL, SQL Server, SQLite, DB2, …etc.

![Blank diagram (2).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603477064408/BXIY7rzKi.png)

###  NoSQL Databases

- NoSQL is also known as non-relational databases, allows us to store and retrieve unstructured data using a dynamic schema. 

- NoSQL is popularly used for its flexible ability to create a unique structure and can be a document, graph, column, or even KeyValue organized as a data structure.

- **Redis (Remote Dictionary Server)**, is the most popular key-value database. It is open-source, with a fast and distributed in-memory implementation, and supports many abstract data structures (some rarely found in other NoSQL).

- **Amazon’s DynamoDB** implement two other key-value stores. **CouchDB** is another document store which solves the queries slightly different providing views to retrieve the information without doing a full query every time.

- Graph databases include **ArangoDB, InfiniteGraph, and Neo4j.**

In this article, we would learn more about MongoDB, its features and perform basic 
CRUD operations. *Other databases are currently out of scope for this article*

*MongoDB is a document store and a rich document-oriented NoSQL database, differing from traditional relational databases. The name is derived from Humongous DB*

### Fundamentals of MongodB

- The data in Mongodb is stored in an un-normalized format, as a collection of documents. 

- A collection in MongoDB is equivalent of a table in RDBMS and a document is equivalent of a record. However, unlike a record, a document need not have the same structure as other documents in the same collection.

**MongoDB is Schemaless**

```js
{
 "_id": 1,
 "name": "Jay",
 "location": "Mumbai"
}
{
 "_id": 2,
 "name": "Jiten",
 "location": "Raleigh",
 "age": 30
}
```

- MongoDB has no predefined schema. If you look again at the documents above, you can see that **Jatin** has an age element, which is missing from **Jay**

*You can even change the schema of an existing document as follows:*

```js
//let's say it's a users schema
db.users.updateOne({_id:1},{$set:{occupation:"Developer"}})
```
...this will add occupation detail to Jay's document.

### Features of MongoDB

**Let see some of the powerful features of MongoDB:-**

**Supports ad hoc queries**

- MongoDB can provide a specific recordset from any or multiple merged collections available on the DB server. Which means it can provide recordset on the fly. (*that's what ad hoc query means*). It supports range queries, regular expressions and field searches. 
 
**Aggregation Framework**

- An Aggregation Pipeline is a series of blocks of computation that we apply one by one to set of documents. Each stage of the pipeline process data records and return computed results. We can use lookup, group, project and unwind to build powerful queries.

**MongoDB Sharding**

- Sharding is a method for distributing data across multiple servers and makes mongodB more scalable. MongoDB uses sharding to support deployments with very large data sets and high throughput operations. Sharding will make it possible to scale our MongoDB scale horizontally. 

- In this process, each shard will hold some portion of data. This will require more complex configurations.

**Replication**

- Replication in mongoDb helps distribute data across several servers. A replica set is like a master-slave replication. 
- A master(primary node) can read and write and slave (secondary node) can copy the data from the master and can be used for primarily for reads.

**MongoDB Indexing**

- Indexes support the efficient execution of queries in MongoDB and improve the performance of searching for the documents.
- MongoDB provides the filed as well as schema level indexing.

**File storage**

- This function, called Grid File System, is included with MongoDB drivers which stores files. 
- MongoDB exposes functions for file manipulation.


**Setting Up mongoDB**

You can set up the mongodb to run all the database queries locally or via using a docker.

- To set it up locally, you can refer their official documentation,  [here](https://www.docker.com).
- To set this up with docker, please visit the [GitHub Wiki](https://github.com/desaijay315/docker-volumes/wiki/MongoDB-Docker-Volume). It will also show, how to use docker volume to persist MongoDB data.

### CRUD Operations with MongoDB

- Show all the DBs

```js
show dbs
```

- Create the database

```js
use hashnodedb
```

- Insert some data to the user's collection

```js
> db.users.insert({"userName": "jaydesai", "age": 28});
> db.users.insert({"userName": "johndoe", "age": 50});
```

- Fetch the data from the user's collection (read operation)

```js
db.users.find({})
```

- Update the user with **city**

```js
db.users.updateOne({_id:1},{$set:{city:"San Francisco"}})
db.users.updateOne({_id:2},{$set:{city:"Delhi"}})
```

- Let's fetch the user data from the collection

```js
db.users.find({})

//Output:
{ "_id" : ObjectId("5f816c888d621b5887605b4f"), "userName" : "jaydesai", "age":28, "city": "San Francisco" } 
{ "_id" : ObjectId("5f816c8f8d621b5887605b50"), "userName" : "johndoe", "age": 50, "city": "Delhi"}
```

Let's update city of user **John** from **Delhi to Dallas **

```js
db.users.updateOne({_id:2},{$set:{city:"Dallas"}})
```

- Delete a document. Let's delete user 2

```js
db.users.deleteOne({_id:2})
```

- If you want to delete all the user who resides in the city Dallas

```js
db.users.deleteMany({"city": "Dallas"})
```

### Conclusion

In this article, we have learned some historical context on databases, traditional file-based systems, NoSQL databases and it's advantages, features of mongoDB database, and how to perform CRUD operations.

In the next article, we would look into many advanced concepts of MongoDB like aggregate framework.

If you liked it please leave some claps to show your support. Also, leave your responses below and reach out to me if you face any issues.

> _Follow me on_ [_Twitter_](http://www.twitter.com/beingjaydesai) _| Check out my_ [_LinkedIn_](https://www.linkedin.com/in/iamjaydesai/) _| See my_ [_GitHub_](https://github.com/desaijay315)

