# MongoDB (using Mongoose)

## Overview

- Accounts
- About
- Set up
- Connecting to the database
- Model file
- Relations between collections
- Methods provided by MongoDb/Mongoose
- Storing data
- Storing images
- Fetching data
- Deleting data
- Updating data: put
- Updating data: patch
- Routing
- Query parameters
- Creating user
- Login user

---

## Accounts

---

tristandeloris@gmail.com :

---

## About

---

'Umongous': stores lots of data.

Database that stores data as **Documents** in **Collections** (as opposed to **Records** in **Tables** with SQL).  
**Database** => **Collections** => **Documents**  
MongoDB is **schemaless**, documents can have different structures.


## Set up

---

- Connect to mongodb.com
- Create a new cluster
- Add a user with password
- Add a network access (will need to be the one our server is on for production)
- Install mongoose to connect to the MongoDB database: `npm install --save mongoose`

---

## Connecting to the database

---

In the main file(i.e: app.js), import mongoose:  
`const mongoose = require('mongoose')`

Connect to the database:

```javascript
mongoose
  .connect('[url]')
  .then(() => {
    console.log('connection successful');
  })
  .catch(() => {
    console.log('connection failed...');
  });
```

---

## Model file

---

Create a model file.  
Import mongoose:  
`const mongoose = require('mongoose')`  
Create a schema const:  
`const Schema = mongoose.Schema;`
Create a model:

```javascript
const [SchemaName]Schema = new Schema({
  [propertyName1]: type,
  [propertyName2]: { type: [type], required: true | false, default: [value] },
});

module.exports = mongoose.model('SchemaNameToExport', [SchemaName]); // if it doesn't exist yet, mongoose will create a collection using the plural(in lowercase) of the model name
```

Import the model file in the main file where the post/get methods are declared/used:  
`const [SchemaName] = require('[path/to/model/file]')`

---

## Relations between collections

---

Collections can have relations by adding a reference to the model:

```javascript
const SchemaNameSchema = new Schema({
  propertyName1: type,
  propertyName2: {
    type: type,
    ref: 'collectionName' // the reference has to be a string
    required: true | false, },
});

module.exports = mongoose.model('SchemaNameToExport', [SchemaName]); // if it doesn't exist yet, mongoose will create a collection using the plural(in lowercase) of the model name
```

---

## Methods provided by MongoDb/Mongoose

---

- save()
- find()
- findOne()
- findById()
- findByIdAndRemove()
- select()
- populate()

### save()

Will write the collection in the database

```javascript
modelName.save();
```

### find()

Will return the desired collection

```javascript
modelName.find().then((collection) => {
  // code to run with the returned data
});
```

### findOne()

Will return the first instance of the desired collection

```javascript
modelName.findOne().then((document) => {
  // code to run with the returned data
});
```

### findById(id)

Will return the required document matching the provided id

```javascript
modelName.findById(id).then((document) => {
  // code to run with the returned data
});
```

### findByIdAndRemove(id)

Will the delete the desired document matching the provided id

```javascript
modelName.findByIdAndRemove(id).then((document) => {
  // code to run after deletion
});
```

### select()

Will return a document with the filtered properties. The desired properties will be added as strings

```javascript
modelName
  .find()
  .select('propertyName1 propertyName3 -_id')
  .then((document) => {
    // the document will have 2 properties only and no id
    // the id will always be added unless specified otherwise
  });
```

### populate()

Will add properties to a referenced document instead of only the id

```javascript
modelName
  .find()
  .populate('embeddedDocumentName')
  .then((document) => {
    // the document will have all the properties
  });
```

---

## Storing data

---

**save()**  
Add the name of the instance and the **save** method to the post method.  
It will create a new database on the cluster when post for the first time and create a document as well as a collection.  
The collection name will be the plural in lowercase of the instance/schema name:

```javascript
app.post([apiUrl], (req,res,next) =>{
    const [instanceName] = new [SchemaName]({
      [propertyName1] : req.body.[propertyName],
      [propertyName2] : req.body.[propertyName],
      ...
  })

  post.save(); // will write the data on the database

  res.status(201).json({ // response sent to the frontend
    message: 'data added successfully'
  })
})
```

---

## Storing images

---

Images and files in general will be stored on the file system, not in the database as they are too heavy. The database will only stores strings/texts.

---

## Fetching data

---

### Fetching documents

Add the name of the schema and the find() method to the get() method:

```javascript
app.get('[url]', (req, res, next) => {
  [SchemaName].find().then((documents) => {
    res.status(200).json({
      message: 'fetched successful',
      data: documents,
    });
  });
});
```

### Fetching one document

Add the name of the schema and the findbyId() method to the get() method

```javascript
app.get('[url]', (req, res, next) => {
  [SchemaName].findById(req.params.id).then((document) => {
    if (document) {
      res.status(200).json({
        message: 'document fetched successfully',
        document: document,
      });
    } else {
      res.status(404).json({
        message: 'an error occured',
      });
    }
  });
});
```

## Deleting data

Add the name of the schema and the delete() method:

```javascript
app.delete('[url/:paramName]'(req,res,next) => {
  [ScemaName].deleteOne({
    [propertyName]: req.body.[paramName] // usually the id. e.g: _id: req.params.id
  })
  .then((result) => {
    res.status(200).json({
      message: 'deleting successful'
    })
  })
})
```

## Updating data

## Routing

Create a folder **routes** and place the files inside.

Create the express Router:
`const router = express.Router();`

```javascript
const express = require('express');

const router = express.Router();

const SchemaName = require('path/to/schema');

router.post('[url]', (req,res,next) => {
  ...
})
```

Import the routes in the main js file:
`const [routeFileName] = require('[path/to/route/file]')`

`app.use('[url]', [routerFileName]);`

## Query parameters

Will filter the request with the query parameters in the url

```javascript
router.get('[url]', (req, res, next) => {
  const [firstParam] = +req.query.[queryParam]; // the query param name must match the name in the url
  const [secondParam] = +req.query.[queryParam];
  const [queryName] = [SchemaName].find();
  let documents;
  if ([firstParam] && [secondParam]) { // will check if the params are set
    [queryName]
      .skip(number // can be the firstParam) // will omit the previous documents
      .limit([number // can be the second param]) // will limit the count of documents in the response to that number
  }

  [queryName]
    .then(documents => {
      documents = documents;
      return Post.count(); // will return a Promise with the number of documents
    })
    .then(count => {
      res.status(200).json({
        message: 'documents fetched succesfully!',
        documents: documents,
        numberOfDocuments: count
      })
    })
    .catch(err => {
      console.log(err)
    })
})
```

---

## Creating user

---

A unique validator must be created when creating a user so that a new account cannot be created with the same email.
Install mongoose-unique-validator:  
`npm install --save mongoose-unique-validator`

```javascript
// Importing mongoose
const mongoose = require('mongoose');

// Importing mongoose validator
const uniqueValidator = require('mongoose-unique-validator');

// Creating the user schema(= model)
const userSchema = mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true, // here the unique validator
  },
  password: {
    type: String,
    required: true,
  },
});

userSchema.plugin(uniqueValidator); // where the validator is plugged in

module.exports = mongoose.model('User', userSchema);
```

The password will need to be encrypted with the **bcryptjs** package:  
Install bcryptjs  
`npm install --save bcryptjs`  
User controller file:

```javascript
const bcrypt = require('bcryptjs');

exports.createUser((req, res, next) => {
  bcrypt
    .hash(req.body.password, 12) // will 'hash' the password and returns a Promise
    .then((hash) => {
      const user = new User({
        email: req.body.email,
        password: hash, // the hashed password
      });
      user
        .save()
        .then((result) => {
          res.status(201).json({
            message: 'User created successfully',
            result: result,
          });
        })
        .catch((err) => {
          error: err;
        });
    });
});
```

## Login User

Install Json Web Token:  
`npm install --save jsonwebtoken`

Comparing the entered password

```javascript
bcryptjs
  .compare(password, user.password) // will compare the entered password and the password stored in the batabase
  .then((doMatch) => {
    if (doMatch) {
      req.session.isLoggedIn = true;
      req.session.user = user;
      return req.session.save((err) => {
        res.redirect('/');
      });
    }
    res.redirect('/login');
  })
  .catch((err) => console.log('compare password error:', err));
```
