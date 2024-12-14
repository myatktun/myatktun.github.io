=======
MongoDB
=======
* `What is MongoDB`_
* `Basic Commands`_
* `Insert Commands`_
* `Query Commands`_
* `Update Commands`_
* `Delete Commands`_
* `Connecting MongoDB on Atlas with mongoose`_
* `Storing hashed passwords in database`_

What is MongoDB
===============
* NoSQL, Non Relational database
* Store JSON
* Easy to get started
* Free cloud hosting - Atlas
`back to top <#top>`_

Basic Commands
==============

.. code-block:: sh

   show dbs
   use DATABASE_NAME
   show collections
   db.dropDatabase()
   db.COLLECTION.drop()

`back to top <#top>`_

Insert Commands
===============

.. code-block:: sh

   db.COLLECTION.insertOne({ key : "value", key : "value"}) # auto create collection
   db.COLLECTION.find() # list all items in collection
   db.COLLECTION.insertMany([{ key1 : "value1"}, { key2 : "value2" }])


`back to top <#top>`_

Query Commands
==============

.. code-block:: sh

   db.COLLECTION.find().limit(NUMBER)
   db.COLLECTION.find().sort({ key1 : 1, key2 : -1 }) # 1 for alphabetical, -1 for reverse
   db.COLLECTION.find().skip(NUMBER) # skip the first NUMBER results found
   db.COLLECTION.find({ key : "value" })
   db.COLLECTION.find({ key : "value" }, { key1 : 1, _id : 0})  # return field of key1 but
                                                                # not _id of the result
   db.COLLECTION.find({ key : "value" }, { key1 : 0 }) # return all fields expect key1


* complex queries
    .. code-block:: sh

       // pass an object into fields
       db.COLLECTION.find({ key : { $eq : "value" }}) # return results equal to "value"
       db.COLLECTION.find({ key : { $ne : "value" }}) # return results not-equal to "value"
       db.COLLECTION.find({ key : { $gt : "value" }}) # return results greater than "value"
       db.COLLECTION.find({ key : { $gte : "value" }}) # return results greater-than-or-eq "value"
       db.COLLECTION.find({ key : { $lt : "value" }})
       db.COLLECTION.find({ key : { $lte : "value" }})
       db.COLLECTION.find({ key : { $in : ["value1", "value2"] }}) # return results equal to
                                                                 # value1 or value2
       db.COLLECTION.find({ key : { $nin : ["value1", "value2"] }}) # return results not-equal to
                                                                  # value1 or value2
       # will also return if { key:null }, only checks key exists, not value
       db.COLLECTION.find({ key : { $exists : true }}) # return results that have key
       db.COLLECTION.find({ key : { $exists : false }}) # return results that don't have key
   
       ## AND query
       db.COLLECTION.find({ key : { $gt : "value" , $lt : "value" }})
       db.COLLECTION.find({ $and : [{ key1 : "value1" }, { key2 : "value2" }] }) # rarely used
   
       ## OR query
       db.COLLECTION.find({ $or : [{ key1 : { $gt : "value"} }, { key2 : "value2" }] })
   
       ## NOT query
       db.COLLECTION.find({ key : { $not : { $gt : "value" } } })  # return results not gt value
   
       ## comparing
       db.COLLECTION.find({ $expr : { $gt : ["$key1", "$key2"] } })  # return results if value of
                                                                   # key1 is gt key2, $ syntax
   
       ## querying with nested keys
       db.COLLECTION.find({ "key1.NESTED_key" : "value" })
       db.COLLECTION.findOne({ key : "value" }) # return first result
   
       ## counting
       db.COLLECTION.countDocuments({ key : "value" }) # return NUMBER


`back to top <#top>`_

Update Commands
===============

.. code-block:: sh

   db.COLLECTION.updateOne({ key : value}, { $set : { newKey:"newValue" } }) # use $ syntax
   # usually update based on "_id"
   db.COLLECTION.updateOne({ _id : value}, { $inc : { key : 3 } }) # increment value of key
   db.COLLECTION.updateOne({ _id : value}, { $rename : { key : "newValue" } }) # rename key
   db.COLLECTION.updateOne({ _id : value}, { $unset : { key : "" } }) # remove key from object
   db.COLLECTION.updateOne({ _id : value}, { $set : { key : "" } }) ## add key to object
   db.COLLECTION.updateOne({ _id : value}, { $push : { key : "value" } }) # push value as array
   db.COLLECTION.updateOne({ _id : value}, { $pull : { key : "value" } }) # remove from array
   db.COLLECTION.updateMany({ key: { $exists : true } }, { $unset : { key :"" } }) # remove from
                                                                                   # all if exist
   db.books.replaceOne({ key : "value" }, { newKey : "newValue"} ) # replace all properties


`back to top <#top>`_

Delete Commands
===============

.. code-block:: sh

   db.COLLECTION.deleteOne({ key : "value" })
   db.COLLECTION.deleteMany({ key : "value" })


`back to top <#top>`_

### Connecting MongoDB on Atlas with mongoose

````js
// in connect.js (file for setting up MongoDB)
const mongoose = require('mongoose')
const connectDB = (url) => { mongoose.connect(url) }
module.exports = connectDB

// in .env file to store mongo_connection_string, gitignore this file
MONGO_URI=mongo_connection_string

// in app.js
require('dotenv').config()

// use async as mongoose.connect() returns a promise
const start = async () => {
  try{
    await connectDB(process.env.MONGO_URI)
    app.listen(PORT)
  } catch (err) {
    console.log(error)
  }
}
````

* setting up schema for Atlas document
    * an instance of the model is called document
    * document by default has no schema

    ````js
    // schema.js in models (by convention) folder
    const mongoose = require('mongoose')
    const Schema = new mongoose.Schema({
    name: String, completed: Boolean
    })

    module.exports = mongoose.model('NAME', Schema)
    ````

* querying to db

    ````js
    const COLLECTION = require('../modles/collection')

    // can directly pass as req.query is an object
    const findItem = await COLLECTION.find(req.query)
    ````

`back to top <#top>`_

Storing hashed passwords in database
====================================

````js
// using bcryptjs
const bcrypt = require('bcryptjs')

const salt = await bcrypt.genSalt(10)
const hashedPassword = await bcrypt.hash(password, salt)
````

`back to top <#top>`_
