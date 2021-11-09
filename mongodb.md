# MongoDB

- Local use
- CRUD operations
- Data type
- Relations
- Schema Validation
- Indexes

---

## Local use

---

### Terminal

Install:  
`https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/`

Commands  
`mongod` : will start the server / database (migth need **sudo**)  
`mongod --dbpath ~/data/db` : **/data/db** is the default target. Use a different path if the database folder has been created somewhere else.  
`mongod --port 27017` : **27017** is the default port. Use a different port if needed.  
`cls`: clears the window by scrolling to the top  
`mongosh`: in a new terminal, connects to the local database

Working with database, collections and documents:  
`show dbs`: will output all the available databases  
`db`: will output the used database  
`use [databaseName]`: will make the specified database available and will create it once we insert data  
`show collections`: will output the available collections  
`db.collectionName.stats()`: will output the stats of the collection

---

## CRUD operations

---

- Create
- Read
- Update
- Delete
- Projection
- Embbeded documents
- Dropping database and collection

### Create

`db.collectionName.insertOne({key1: value, key2: value})`: will create one document with the given values  
`db.collectionName.insertMany([{key1: value}, {key: value}])`: will delete the first document that matches the filter

#### Ordering

By default MongoDB will insert documents in an _ordered_ manner, meaning it will insert the documents one at a time. But if one document is not valid(i.e: duplicate \_id), the operation will stop at that document. All valid documents before will have been inserted.  
To overwrite the default behavior, use the parameter `{ordered: false}`:  
`db.collectionName.insertMany([{key1: value}, {key: value}], {ordered: false})`. This way all valid documents will be inserted.

#### Write Concern

MongoDB will write to the database and to a _journal_:  
`{ordered: false}`: `db.collectionName.insertMany([{key1: value}, {key: value}], {writeConcern: {w:1, j:true}})`

#### Importing Data

`mongoImport`: will import an existing database from a json file.  
`mongoimport [fileName].json -d [dataBaseName] -c [collectionName] --jsonArray --drop`  
`-d`: will specify the database name  
`-c`: will specify the collection name  
`--jsonArray`: will specify that the data imported is an array of type json  
`--drop`: will drop the database/collection if it already exists, otherwise it will append to the existing collection  
`db.collectionName.explain()`

### Read

`db.collectionName.find()`: will return every documents in the collection  
`db.collectionName.find({filter: value})`: will return every documents in the collection that matches the filter  
`db.collectionName.find({key: {$gt: value}})`: will return all documents that are **greater than** the value provided  
The **find()** method returns a cursor, not the data. We can append methods to it to get back the data in a desired format. i.e: **.toArray()**, will return an array of all the data; **.forEach()**, will loop over all the documents and return an array of them

`db.collectionName.findOne({filter: value})`: will return the first document that matches the filter

#### Operators

`$gt`: greater than the provided value  
`$lt`: lower than the provided value  
`$ne`: matches the documents **not equal**to the provided value

i.e:  
`db.collectionName.findOne({key: {$gt: value}})`: will return the first document that is **greater than** the value provided

#### Elements

`$exists`: Matches documents that have the specified field.  
`$type`: Selects documents if a field is of the specified type.

#### Querying arrays

`$size`: will look for a given size array. `db.collectionName.find({arrayName: {$size: valueToCheck}})`  
`$all`: will look for values in an array no matter the order. `db.collectionName.find({arrayName: {$all: [value1, value2, ...]}})`  
`$elemMatch`: will look for all given values in the same array.

#### Cursors

`next()`: will output the next document if any exists  
`hasNext()`: will output a boolean if there is a next document  
`sort()`: will sort the documents in a ascending or descending order  
`skip()`: will skip the number of requested documents  
`limit()`: will only output the number of requested documents

#### Projection

Will return the required fields of each documents:  
`db.collectionName.find({}, {key: 1, _id: 0})`, 1 will tell mongodb to return the key, 0 will ignore it. By default everything is ignored but the \_id, we have to explicitly tell mongodb that we don't need it.  
`$slice`: can be used with projection on an array

### Update

`db.collectionName.updateOne({filter: value}, {$set: {key: value}})`: will update the first document that matches the filter. The value passed will be updated or created if it doesn't exist  
`db.collectionName.updateMany({filter: value}, {$set: {key: value}})`: will update all documents that matches the filter. The value passed will be updated or created if it doesn't exist  
`db.collectionName.updateMany({}, {$set: {key: value}})`: will update all the documents. The value passed will be updated or created if it doesn't exist  
`db.collectionName.replaceOne({filter: value}, {key: value})` will replace the desired document

#### Rename

`db.collectionName.update({filter:value}, {$rename: {fieldName: updatedFieldName}})`: will update all the documents. The value passed will be updated or created if it doesn't exist

#### Upsert

`db.collectionName.updateOne({filter: value}, {$set: {key: value}}, {upsert: true})` will update the value and create it if it doesn't exist

#### Update Arrays

`db.collectionName.updateOne({filter: value, arrayToModify: value}, {$set: {"arrayName.$": newValue}})` will update the value of an array  
`db.collectionName.updateOne({filter: value}, {$set: {"arrayName.$[]": value}})` will update the value of all the matching arrays

### Delete

`db.collectionName.deleteOne({filter: value})`: will delete the first document that matches the filter  
`db.collectionName.deleteMany({filter: value})`: will delete all the documents that matches the filter  
`db.collectionName.deleteMany({})`: will delete all documents

### Embbeded documents

`db.collectionName.updateMany({}, {$set:{key: {embeddedKey1: value,embeddedKey2: value }})`  
Documents can have arrays as values  
`db.collectionName.updateMany({filter: value}, {$set:{key: [value1, value2, value3]})`

### Dropping database and collection

`db.dropDatabase()`: will delete the database  
`db.collectionName.drop()`: will delete the collection

---

## Data type

---

- Text
- Boolean
- Number
  - Integer (int32)
  - NumberLong(int64): default number value
  - NumberDecimal
- ObjectId
- ISODate
- Timestamp
- Embedded document
- Array

---

## Relations

---

Use embedded documents or references depending on the needs of the application.

- One to one : **embedded documents**
- One to many : **embedded documents**
- Many to many : **references**

All this will also depend on the size of the data we use and how often these data will change. If a document needs to be updated often then **embedded documents** will not work as the same data will need to be modified in various places.

---

## Schema Validation

---

### Creating a schema validation

Every document inserted or updated will have to be of their respective required type

`db.createCollection('collectionName',{})`

```javascript
// Example of a 'post' collection with comments
db.createCollection('posts', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required',
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required',
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required',
              },
            },
          },
        },
      },
    },
  },
});
```

### Updating a schema validation

By adding **validationAction: 'warn'**, the document will be inserted or updated but with a warning if it isn't valid

`db.runCommend({collMod: 'collectionName', validator:{...}})`

```javascript
db.runCommand({
  collMod: 'posts', // Collection Modification
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required',
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required',
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required',
              },
            },
          },
        },
      },
    },
  },
  validationAction: 'warn',
});
```

---

## Indexes

---

Can help speed up the queries. It's useful for returning a small part of the collection, not to use if we need to return the whole collection or most of it.

`db.collectionName.createIndex({key: 1})`: will create an index  
`db.collectionName.dropIndex({key: 1})`: will drop an index

---

## Geospatial data

---

Longitude first
Latitude second
Create an Index
near
geowithin
geo
type : Point, Polygon....  
/ coordinates

---

## Aggregation framework

---

Allows the transforming of data through stages.  
aggregate()
$match
$project : will take one document and return that document but transformed
$group : will 'group' several documents into one document
$sort
$avg
$convert
$bucket
$bucketAuto
$toDate
$limit
$skip
$limit
$lookup

$geoNear

### Array

$unwind

---

## Numbers

---

int(32): NumberInt(), will save up space. Perfect for simple intergers like age  
int(64): NumberLong()  
doubles(64): **Default type**  
high precision doubles(128): NumberDecimal(), will take more space but is necessary when doing calculations

---

## Security

---

`db.createUser()` will create a user that has an admin role:

```javascript
mongod --auth // will require an authentication to use the database
use admin // Log in to the admin database first to create a user
db.createUser({'user': '[userName]', 'pwd': '[password]', roles:['userAdminAnyDatabase']}); // provide a username and a password
db.auth('userName','password'); // also needs to be run in the admin database

// or before running the mongo command
mongo -u 'userName' -p 'password' --authenticationDatabase admin // will connect to the database and authenticate at the same time

```

### Roles

- userAdminAnyDatabase:
- dbAdminAnyDatabase:
- read:
- readWrite:
- ...

### SSL

Allow ssl data encryption

---

## Performance

---

**Capped**: will add a size and a max number of documents to a collection: `db.createCollection('collectionName', {capped: true, size: maxSizeOfTheCollection, max: maxNumberOfDocuments})`.  
Once the max number of documents is reached, it will be possible to add other documents, but the first documents will be removed.

### Replica sets

Will create duplicates of the data so that if the Primary Node is down, data will be accessed through the other Nodes:  
**Client** >> **MongoDB Server** >> **Primary Node** >> **Secondary Node**

### Sharding

Will **split** the data into several **shards** to ease the load on the server.  
Use **Shard Key** to make the queries faster.

---

## MONGODB Certificate

---

mongodb+srv://<username>:<password>@<hostname>/<database>
