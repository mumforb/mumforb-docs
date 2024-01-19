# MongoDB

Connect to a mongodb instance at a URL, using a format like mongodb://localhost:27017

Load initial data via built-in methods; it might look something like this:

```
const client = new MongoClient(url);
const db = client.db(dbName);
results = await db.collection('nameOfCollection').insertMany(data);
```

This will take a seeding file and put it in a specified database. That db object is the crux of a lot of the operations; creating an instance of the MongoClient is more or less step one. The above code snippet was demonstrated in a Promise.

## Assert

Demo I was watching used assert (like, the core testing library) to verify that the number of records inserted when initially setting up the db equalled the expected number. Wild!

## _id

The _id that's automatically generated is a special type - it isn't jsut a string, even though it looks like just a string. You can convert it from a string to the special type via Mongo.ObjectID(id).

## Cursor

This is using things like .limit(), which is a marker that helps you do things that help do things in the data prior to it being permanently saved within the DB.

## Built in methods

get() - gets an array, even if you have a parameter as an argument (similar to find())
getById(id) - always returns one object (similar to findOne()). you need to use the _id that's built-in.
insertMany(data)
insertOne(data)
findOneAndReplace({_id: ObjetID(id)}, {..data}, options)
remove(id)

And so on. Idea is that all the built-in methods extend right off the repository object, and allow you to do lots of the main tasks with no additional effort. Neato!

Built in functions .serverStatus() and .listDatabases() are very useful to give lots of information on the mongo database itself. These are methods on client.db()dbName.admin().