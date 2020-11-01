## Handle Advanced Data Processing using MongoDB's Aggregation Framework

This article is a part of our ongoing database  [series](https://djaytechdiary.com/mongodb-a-beginners-guide). 

We learned some basics in our  [previous](https://djaytechdiary.com/mongodb-a-beginners-guide) article. In this article, we are going to learn about the following topics:

- What is Aggregation Framework in MongoDB
- Aggregation Operators - Stages, Expressions and accumulators
- Examples of Aggregation Stages on a dataset

Many more...

### What is the Aggregation Framework?

![Untitled Diagram (4).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604220813166/kGwUVow66.png)

- Aggregation is the procedure of processing data in one or more stages where the result of each stage is inputted to the next stage in order to return one combined result. 

- You can think of an aggregation method as the alternative for the find method. It is a pipeline of steps which runs on the data retrieved from your collection and generates the output as you desire. This framework provides a very powerful way of modelling your data transformation.

- Every stage in the pipeline which will receive the output from the previous stage, and the structure data received can be slowly modified in the way you need it using the above stages in the pipeline.


**Pipeline stages:**

1. $project  - It selects the specific fields from the documents
2. $match   - It will match some criteria specified
3. $group   - This will analyse the documents based on the grouping and perform aggregate on the same
4. $sort      - This will sort the documents by some criteria
5. $unwind - This stage deal with the arrays. More examples for this can be seen as below 


**Below are some of the datasets used in this article...**

-  [Grades](https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/grades.json)  dataset
-  [Companies](https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/companies.json)  dataset
- Custom hand rolled datasets


You can import the above dataset to your mongodb database. You can learn how to install mongodb docker instance,  [here](https://github.com/desaijay315/docker-volumes/wiki).

**Let's take this students dataset**

```js
{
	"_id" : ObjectId("5f9a940f9d32533a3dc9b2d6"),
	"gender" : "male",
	"name" : {
		"first" : "john",
		"last" : "doe"
	},
	"location" : {
		"coordinates" : {
			"latitude" : "-10.5930",
			"longitude" : "-40.3670"
		}
	},
	"email" : "johndoe@example.com"
},
{
	"_id" : ObjectId("5f9a940f9d32533a3dc9b2d5"),
	"gender" : "male",
	"name" : {
		"first" : "jay",
		"last" : "desai"
	},
	"location" : {
		"coordinates" : {
			"latitude" : "-150.4177",
			"longitude" : "-60.2905"
		}
	},
	"email" : "jaydesai@example.com"
},
{
	"_id" : ObjectId("5f9a940f9d32533a3dc9b2d4"),
	"gender" : "female",
	"name" : {
		"title" : "ms",
		"first" : "Keily",
		"last" : "Haisley"
	},
	"location" : {
		"coordinates" : {
			"latitude" : "49.3902",
			"longitude" : "-97.8141"
		}
	},
	"email" : "keily.haisley@example.com"
},
{
	"_id" : ObjectId("5f9a940f9d32533a3dc9b2d3"),
	"gender" : "female",
	"name" : {
		"first" : "yara",
		"last" : "jonhson"
	},
	"location" : {
		"coordinates" : {
			"latitude" : "49.3344",
			"longitude" : "-115.3165"
		}
	},
	"email" : "yara.jonhson@example.com"
}
```

- Find one student from our student's collection

```js
db.students.findOne();
```

- Now, We would get the number of students with a gender male located in that city. We would use a $match to scan through the collection to get all the documents that match your criteria, then $group over the documents. When we have to do analysis on a group of data we can use the $group aggregation stage. So by this, we can perform analysis on data that are grouped together by some features. This is similar to SQL’s GROUP BY clause.


```js
db.students.aggregate(
    [
        {
            $match: {gender: 'female'}
        
        },
        {
            $group: {_id: { state: "$location.state"}, totalStudents: {$sum:1}}
        }
    ]
);

//Output:

{
	"_id" : {
		"state" : "seattle"
	},
	"totalStudents" : 2
}
```

- **Sorting** - Let’s see how we can sort the documents using the *$sort* stage in the aggregate function.


```js
    db.students.aggregate(
        [
            {$match: {gender: 'female'}},
            {
                $group: {_id: { state: "$location.state"}, totalStudents: {$sum:1}}},
                {
                    $sort: {totalStudents: -1}
                }
        ]
    );

//Output:

{
	"_id" : {
		"state" : "seattle"
	},
	"totalStudents" : 2
}

//The sorting will be done on the previous stage.

```

### Transforming the data  with project

- **$project** - This stage helps us to transform every document instead of grouping

```js

db.students.aggregate([
    { $project: {_id: 0, gender: 1, fullName: {$concat: ["$name.first"," ", "$name.last"]} }}
]);

//Output:

{"gender":"male","fullName":"john doe"}
{"gender":"male","fullName":"jay desai"}
{"gender":"female","fullName":"Keily Haisley"}
{"gender":"female","fullName":"yara jonhson"}
```

- Let's capitalize the student's name using ***$toUpper***

```js
 db.students.aggregate([
    { $project: {_id: 0, gender: 1, fullName: {$concat: [{$toUpper: "$name.first"}, " ", {$toUpper: "$name.last"}]} }}
    ]);

//output

{"gender":"male","fullName":"JOHN DOE"}
{"gender":"male","fullName":"JAY DESAI"}
{"gender":"female","fullName":"KEILY HAISLEY"}
{"gender":"female","fullName":"YARA JONHSON"}
```

- Let's capitalize only the first letter of the first and last name

```js
 db.students.aggregate([
    { $project: {_id: 0, gender: 1, studentFullName: {
                    $concat: 
                    [
                        {$toUpper: {$substrCP:["$name.first", 0, 1] }}, {$substrCP:[
                            "$name.first", 1, {$subtract:[{$strLenCP: '$name.first'}, 1]}
                            ]}," ", 
                            
                            {$toUpper: {$substrCP:["$name.last", 0, 1] }}, {$substrCP:[
                            "$name.last", 1, {$subtract:[{$strLenCP: '$name.last'}, 1]}
                            ]}
                    ]
                    
                }              
        
    }}
    ]);

//Output:

{"gender":"male","studentFullName":"John Doe"}
{"gender":"male","studentFullName":"Jay Desai"}
{"gender":"female","studentFullName":"Keily Haisley"}
{"gender":"female","studentFullName":"Yara Jonhson"}
```

- **$substrCP** - It will return the substring of a string. It takes parameters, string expression (the string which needs to be considered, in our case first and last name), the starting point of the substring and the valid point up to which we require that transformation.

- **$strLenCP** - It will get the length of the given string

- **$subtract** - This will take two numbers and give the difference between them


- Now, let's provide the result of the previous stage to the next with $project. We would convert the location to geoJSON object

```js
db.students.aggregate([
        {
            $project: {
            _id: 0,
            email: 1, 
            name:1,
            location: 
            {
                type: "Point", 
                coordinates:[
                    '$location.coordinates.longitude',
                    '$location.coordinates.latitude'
                    ]
                
            }
            
        }
        }, 
        
        {
            $project:{
                gender:1,
                email:1,
                location:1,
                fullName: {
                    $concat: 
                    [
                        {$toUpper: {$substrCP:["$name.first", 0, 1] }}, {$substrCP:[
                            "$name.first", 1, {$subtract:[{$strLenCP: '$name.first'}, 1]}
                            ]}," ", 
                            
                            {$toUpper: {$substrCP:["$name.last", 0, 1] }}, {$substrCP:[
                            "$name.last", 1, {$subtract:[{$strLenCP: '$name.last'}, 1]}
                            ]}
                    ]
                    
                }                 
                }
                
        }
    ]);

//Output:

{"location":{"type":"Point","coordinates":["-40.3670","-10.5930"]},"email":"johndoe@example.com","fullName":"John Doe"}
{"location":{"type":"Point","coordinates":["-60.2905","-150.4177"]},"email":"jaydesai@example.com","fullName":"Jay Desai"}
{"location":{"type":"Point","coordinates":["-97.8141","49.3902"]},"email":"keily.haisley@example.com","fullName":"Keily Haisley"}
{"location":{"type":"Point","coordinates":["-115.3165","49.3344"]},"email":"yara.jonhson@example.com","fullName":"Yara Jonhson"}
```

- Our first stage with $project would result above output without full name, ie, it will provide the first name and last name with email and co-ordinates. 
- Combining the first project stage with the next stage would give fullName with the capitalized first letter of first and last name

- **Type Point **- MongoDB supports the GeoJSON object types. MongoDB geospatial queries on GeoJSON objects calculate on a sphere. More on this,  [here](https://docs.mongodb.com/manual/reference/geojson/index.html).



**- Convert the coordinate from string to double**

```js
    db.students.aggregate([
        {
            $project: {
            _id: 0,
            email: 1, 
            location: 
            {
                type: "Point", 
                coordinates:[
                    {$convert: {input: '$location.coordinates.longitude', to: "double", onError: 0.0, onNull: 0.0}},
                    {$convert: {input: '$location.coordinates.latitude', to: "double", onError: 0.0, onNull: 0.0}}
                    ]
                
            }
            
        }
        }
    ]);

//output

{"location":{"type":"Point","coordinates":[-40.367,-10.593]},"email":"johndoe@example.com"}
{"location":{"type":"Point","coordinates":[-60.2905,-150.4177]},"email":"jaydesai@example.com"}
{"location":{"type":"Point","coordinates":[-97.8141,49.3902]},"email":"keily.haisley@example.com"}
{"location":{"type":"Point","coordinates":[-115.3165,49.3344]},"email":"yara.jonhson@example.com"}
```

- **$convert** - This will convert the value to a specific type, we will convert from a string to double data type
- **onError ** - This will output the error if there are due to some conversion
- **onNull** - If the values are null, it will default to 0.0


***Next, we would be using  [companies](https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/companies.json)  dataset for the below examples***


    
- How many products, competitors do a company have? Also, sort it by descending order of products count.

```js
db.companies.aggregate([
        {$project: { countOfProducts: { $size:"$products" }, countOfCompetitors: {$size: "$competitions"}}},
        {
            $sort: {countOfProducts: -1}
        }]);
```

- $size - Size will give us the length of an array


- Get all the companies which have their offices in New York?

```js
db.companies.aggregate([{$match:{offices: {$elemMatch: {city:'New York'}}}}]);
```

- To remember - **Offices** is an array of objects. 
- $elemMatch  - This will match all the elements of the **array components**


- Get the total no of offices in New York?

```js

db.companies.aggregate(
        [{
                $match:{offices: {$elemMatch: {city:'New York'}}}},
                {
                    $project: {_id: 1, offices:1, totalNoOfOffices: {$size: "$offices"}}},
                    {$sort: {totalNoOfOffices: -1}}
]);

//example output:
...
{"description":"Google New York","address1":"76 Ninth Avenue","address2":"4th Floor","zip_code":"10011","city":"New York","state_code":"NY","country_code":"USA","latitude":40.74222,"longitude":-74.004489}
...
```


- Now, let's say in our company collection, we want to convert the **updated_at** date to ISODate.

```js

//example1 - First Usage:

 db.companies.aggregate([
    {
        $project: { 
            name: 1, 
            crunchbase_url: 1, 
            permalink: 1, 
            convertupdatedDate: {
                $convert: {input: '$updated_at', to: 'date'}
            }
        }
    }
 ]);


/
//example2 - Second Usage - To use toDate

db.companies.aggregate([
    {
        $project: {
            name: 1, 
            crunchbase_url: 1, 
            permalink: 1, 
            convertupdatedDate: {
                $toDate:'$updated_at'
            }
        }
    }
 ]);

//output:

{
	"_id" : ObjectId("52cdef7c4bab8bd675297d8a"),
	"name" : "Wetpaint",
	"permalink" : "abc2",
	"crunchbase_url" : "http://www.crunchbase.com/company/wetpaint",
	"convertupdatedDate" : ISODate("2013-12-08T12:45:44.000+05:30")
}
```

- $toDate - Will convert the string to date

- How many companies founded in which year?

*Our collection has null values for **founded_year** key, so we would only match those which has founded_year and its not equal to null*

```js
 db.companies.aggregate([
    {
        $match: {founded_year: {$ne: null}}
    },
    {$group: {_id: { company_established_year: "$founded_year" }, numOfCompaniesFounded: {$sum: 1} }},
    {
        $sort: {numOfCompaniesFounded:-1}
        
    }]); 
```

### Let's Transform some arrays

*Consider the dirty  [playlist](https://gist.github.com/desaijay315/9b46cde288566cb79e8c3ed62225d7d4)  collection.
*

- What if we need to create a new array and push all the values from an array of objects or any other array??

```js
db.playlist.aggregate(
           [
               {
                   $group: {_id: {age: "age" }, musicCategory:{ $push: "$playlists.musics_category"}}
                   
               }
           ]
        );
```

We would group them by age. We can look at the output and see that we have created an array in the array. Also, we can see the duplicate data. Let's tackle that...

```js
 db.playlist.aggregate(
            [
                {
                    $unwind: "$playlists"
                    
                },
                {
                  $group: {_id: {age: "$age" }, musicCategory:{ $push: "$playlists.musics_category"}}
                  
                },
          ]
        );
```

- $unwind - This stage will destructure an array field from the input document to the output document for each element.


```js
      db.playlist.aggregate(
            [
                {
                    $unwind: "$playlists"
                },
                {
                  $group: {_id: {age: "$age" }, musicCategory:{ $addToSet: "$playlists.musics_category"}}
                  
                },
          ]
        );
```

- $addToSet - This will remove the duplicate values from the arrays generated


**- Let's say if we are interested in the first playlist of the users, we can use a $slice
**

```js
db.playlist.aggregate([
       {$project: {_id: 0, playlist: {$slice: ["$playlists", 1]}}}    
    ])
```

- **$slice ** - Slice will take two parameters, first the array and second the point till which we need to get the array elements. 

**- Let's get the last two playlists of the users**

```js
    db.playlist.aggregate([
       {$project: {_id: 0, playlist: {$slice: ["$playlists", -2]}}}    
    ]);
```

**- To know how many playlists the user has. Again $size plays an important role.**

```js
     db.playlist.aggregate([
       {$project: {_id: 0, playlist: {$size:"$playlists"}}}    
    ]);
```

***Let's take the  [grades](https://github.com/ozlerhakan/mongodb-json-files/blob/master/datasets/grades.json)  dataset for the below examples:***

- We need to get all the students with a score greater than 40

```js
  db.grades.aggregate([
       {$project: {_id:0, score: {$filter: {input: "$scores", as: "stusc", cond:{ $gt: ["$$stusc.score", 40] } } }}}    
    ]);
```

- $filter - Returns all the elements which match the specified criteria
- as      - Variable which represents the input. It's like an alias 
- cond  - This is a condition which will resolve once the condition is satisfied and the return the resolved documents


**- Let's get to slightly complex query. Get the student which has the max score from their respected classes. **

```js
  db.grades.aggregate([
       {$unwind: "$scores" },
       {$project: {_id:0, class_id:1, student_id: 1, scores: "$scores.score" }},
       {$sort: {"scores": -1}},
       {$group: {_id: {class_id: "$class_id"}, stdid: {$first: "$student_id"}, maxScore: {$max:"$scores"}}},
       {$sort: {"maxScore": -1}}
    ]);
```

**References**

- MongoDB  [Aggregation](https://docs.mongodb.com/manual/aggregation/)
- [Group](https://docs.mongodb.com/manual/reference/operator/aggregation/group/index.html) Stage
- [Project](https://docs.mongodb.com/manual/reference/operator/aggregation/project/index.html) Stage
-   [Match](https://docs.mongodb.com/manual/reference/operator/aggregation/match/index.html)  Stage


### Conclusion

Congratulations🎉🎉, you have made it to this entire article. I hope you learned some of the many usages of an aggregation framework of MongoDB.

We have now learned the aggregation framework in Mongodb with different operations on our sample datasets.

If you liked it please leave some love for this article to show your support and let me know if you would like me to write more on MongoDB. Leave your responses below and reach out to me if you face any issues.


















