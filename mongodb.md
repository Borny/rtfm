# MongoDB

- Local use
- CRUD operations
- Data type
- Relations
- Schema Validation
  
---
## Local use
---

### Terminal 

Install:   
`https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/`

Commands  
`mongod` : will start the server / database  
`mongod --dbpath /data/db` : **/data/db** is the default target. Use a different path if the database folder has been created somewhere else.  
`mongod --port 27017` : **27017** is the default port. Use a different port if needed.  
`cls`: clears the window by scrolling to the top

Working with database, collections and documents:  
`show dbs`: will output all the available databases  
`db`: will output the used database  
`use [databaseName]`: will make the specified database available and will create it once we insert data  
`show collections`: will output the available collections  

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
By default MongoDB will insert documents in an *ordered* manner, meaning it will insert the documents one at a time. But if one document is not valid(i.e: duplicate _id), the operation will stop at that document. All valid documents before will have been inserted.  
To overwrite the default behavior, use the parameter `{ordered: false}`:  
`db.collectionName.insertMany([{key1: value}, {key: value}], {ordered: false})`. This way all valid documents will be inserted.

#### Write Concern
MongoDB will write to the database and to a *journal*:  
`{ordered: false}`: `db.collectionName.insertMany([{key1: value}, {key: value}], {writeConcern: {w:1, j:true}})`

#### Importing Data
`mongoImport`: will import an existing database from a json file.  
`mongoimport [fileName].json -d [dataBaseName] -c [collectionName] --jsonArray --drop`  
`-d`: will specify the database name  
`-c`: will specify the collection name  
`--jsonArray`: will specify that the data imported is an array of type json  
`--drop`: will drop the database/collection if it already exists, otherwise it will append to the existing collection    

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
`$exists`:	Matches documents that have the specified field.
`$type`:	Selects documents if a field is of the specified type.


### Update  
`db.collectionName.updateOne({filter: value}, {$set: {key: value}})`: will update the first document that matches the filter. The value passed will be updated or created if it doesn't exist  
`db.collectionName.updateMany({filter: value}, {$set: {key: value}})`: will update all documents that matches the filter. The value passed will be updated or created if it doesn't exist    
`db.collectionName.updateMany({}, {$set: {key: value}})`: will update all the documents. The value passed will be updated or created if it doesn't exist      
`db.collectionName.replaceOne({filter: value}, {key: value})` will replace the desired document  

### Delete  
`db.collectionName.deleteOne({filter: value})`: will delete the first document that matches the filter  
`db.collectionName.deleteMany({filter: value})`: will delete all the documents that matches the filter  
`db.collectionName.deleteMany({})`: will delete all documents    

### Projection  
Will return the required fields of each documents:  
`db.collectionName.find({}, {key: 1, _id: 0})`, 1 will tell mongodb to return the key, 0 will ignore it. By default everything is ignored but the _id, we have to explicitly tell mongodb that we don't need it.  

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
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
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
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  }
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
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
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
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  },
  validationAction: 'warn'
});

```
