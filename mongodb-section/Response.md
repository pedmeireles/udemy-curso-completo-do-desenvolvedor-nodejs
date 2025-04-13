Consider to install the mongodb into local machine.

Here's the guide to install on ubuntu: https://www.mongodb.com/pt-br/docs/manual/tutorial/install-mongodb-on-ubuntu/ 


useful cheatsheet:

- Start mongodb server: sudo systemctl start mongod
- Check mongodb status: sudo systemctl status mongod
- stop mongodb: sudo systemctl stop mongod
- restart mongodb: sudo systemctl restart mongod

for future reference:
Mongodb documentation, in portuguese: https://www.mongodb.com/pt-br/docs/
Mongodb documentation, in english: https://www.mongodb.com/docs/

CRUD documentation: https://www.mongodb.com/docs/manual/crud/


1)
To connect to mongo via terminal: mongosh

Then to create a database: use the command 
"
use <databaseName>
"

So, in our case:
use library


to confirm the creation was successful:
show dbs

2) to create a collection: 
There are 2 ways to create a collection:
- We can do it explicitly with the command "db.createCollection("collectionName")"

on our case:
db.createCollection("books")

- Or we could create adding already our first document on the respective collection. 

For instance, if we want to add the first document, our syntax would be the following:

db.books.insertOne({
    title: "Introduction to HTML markup language",
    value: 25.00,
    author: "John"
})

both would create the collection; but the second one would already create the first document. 

To confirm that the creation was successful:
show collections

or

db.getCollectionNames()

3. For the insertion documentation, you can checkout this link: https://www.mongodb.com/docs/manual/tutorial/insert-documents/


To create a single document on the collection:
db.<collectionName>.insertOne(payload)

ref: https://mongodb.github.io/node-mongodb-native/3.6/api/Collection.html#insertOne

on our case:
db.books.insertOne(payload)

so, if we want to create the documents, assuming that we didn't added none until now, with the insertOne, we can do the following queue of operations:

db.books.insertOne({title: "Introduction to HTML markup language",
value: 25.00,
Author: "John"})

db.books.insertOne({title: "NodeJS from basics to advanced",
value: 280.00,
Author: "Jorge"})



db.books.insertOne({title: "Android - creating real apps",
value: 290.00,
Author: "Jamilton"}
)

db.books.insertOne({title: "PHP and MySQL",
value: 190.00,
Author: "Fernando"})

db.books.insertOne({title: "Programming Logic",
value: 20.00,
Author: "Maria"})




or, which would be quicker, we could add many in a single operation, with the Collection.insertMany()
(reference: https://mongodb.github.io/node-mongodb-native/3.6/api/Collection.html#insertMany)


the operation would be the following:

db.books.insertMany(
    [
        {
            title: "Introduction to HTML markup language",
            value: 25.00,
            Author: "John"
        },

        {
            title: "NodeJS from basics to advanced",
            value: 280.00,
            Author: "Jorge"
        },
        {
            title: "Android - creating real apps",
            value: 290.00,
            Author: "Jamilton"
        },
        {
            title: "PHP and MySQL",
            value: 190.00,
            Author: "Fernando"
        },

        {
            title: "Programming Logic",
            value: 20.00,
            Author: "Maria"
        },
    ]
)

to confirm that all the documents has being added, you can find() without a criteria/filter:

db.books.find( {} )

4. 
Useful references:
Query filter documents: https://www.mongodb.com/docs/manual/core/document/#std-label-document-query-filter

Query selectors: https://www.mongodb.com/docs/manual/reference/operator/query/#std-label-query-selectors
And operator, inside Logical operations: https://www.mongodb.com/docs/manual/reference/operator/query/and/


so, the generic syntax would be the following:
db.books.find({<field1>: { <operator>: <value> }, <field2>: { <operator>: <value> }, ...})



The following queries are:

Q:Create a query that returns only book documents with values ​​greater than 200.00
A: db.books.find({value: {$gt: 200}})

Q:Create a query that returns only documents with values ​​between 10 and 30
A: db.books.find({$and: [{value: {$gt: 10}},{value: {$lt: 30}} ]})

Noticed that we must use the and operation, appending the greater than, and lower than on it

Q:Create a query that returns all documents, except those whose author is Fernando
A: db.books.find({Author: {$ne: "Fernando"}})


5) 
For reference:
Update documentation: https://www.mongodb.com/docs/manual/tutorial/update-documents/

Syntax of updateX:

db.books.updateX(filter, update, options, callback )

There's also the update() method, but the documentation affirms that this one is deprecated, and should be avoided the usage.
ref: https://mongodb.github.io/node-mongodb-native/3.6/api/Collection.html#update

Update the following documents:

Q:Update the document whose title is PHP and MySQL, changing its value from 190.00 to 175.00
A: db.books.updateOne({title: "PHP and MySQL"}, {
    $set: {value: 175.00}
})

to confirm:
db.books.find({title: "PHP and MySQL"})

Q:Update the document whose author is Jorge, changing its title to Complete NodeJS Course
A: db.books.updateOne({Author: "Jorge"}, {$set: {title: "Complete NodeJS Course"}})

to confirm:
db.books.find({Author: "Jorge"})

Q:Update all documents whose value is equal to or less than 25.00 to the value 27.00
A: db.books.updateMany({value: {$lte: 25}}, {$set:{value:27}})

to confirm: 
db.books.find({value:27})

6. Reference for removing documents:
https://www.mongodb.com/docs/manual/tutorial/remove-documents/

syntax:
deleteMany(filter, options, callback)

deleteOne(filter, options, callback)

for the following queries, we won't need to add no options or callback.

Remove the following documents:

Q: Remove the document whose author is John
A: db.books.deleteOne({Author: "John"})
to confirm:
db.books.find({Author: "John"})

Q: Remove all documents whose value is greater than 280.00
A: db.books.deleteMany({value:{$gt: 280}})

to confirm:
db.books.find({value:{$gt: 280}})

Q: Remove all documents whose value is less than 30.00
A: db.books.deleteMany({value:{$lt: 30}})

to confirm:
db.books.find({value:{$lt: 30}})
