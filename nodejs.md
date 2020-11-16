# Node JS

```javascript
```

- REPL
- Event Loop
- Import files
- Export files
- Create server
- Auto restart of the server: nodemon
- Request
- CORS: Cross-Origin Ressource Sharing
- ExpressJs
- Response methods
- Middleware
- Router
- Static files
- Templating
- Model View Controller (MVC)
- Cookies
- Sessions
- Cross-Site Request Forgery (CSRF)
- Showing errors
- Mailing
- Reset password
- Validation
- Error handling
- File upload
- REST API
- Web Socket
- GraphQL
- Deploying AWS

---

## REPL

---

Used to write code directly in the terminal using the **node** command.  
**R** - **Read**: Read the user input  
**E** - **Eval**: Evaluate the user input  
**P** - **Print**: Print output/result  
**L** - **Loop**: Wait for the new Input

---

## Event Loop

---

Runs as long as the server is open.  
Will push/pull functions in and out of the stack queue.

---

## Import files

---

### require()

Used in NodeJS to use core and third party modules/packages.
No slash means that it will not look for custom files but for files that come with it.  
`const http = require('http')` // http comes with node  
`const http = require('./http')` // this would be a custom file present in the same folder.

---

## Export files

---

### Single export

```javascript
const functionToExport = () => {
  // some code...
};
module.exports = functionToExport;
```

### Multi export

Use the object syntax:

```javascript
const functionToExport1 = () => {
  // some code...
};
const functionToExport2 = () => {
  // some code...
};
```

```javascript
module.exports = {
  function1: functionToExport1,
  function3: functionToExport2,
  function3: 'Hard coded stuff...',
};
```

**or**

```javascript
(module.exports.function1 = functionToExport1),
  (module.exports.function2 = functionToExport2),
  (module.exports.function3 = 'Hard coded stuff...');
```

**or**

```javascript
(exports.function1 = functionToExport1),
  (exports.function2 = functionToExport2),
  (exports.function3 = 'Hard coded stuff...');
```

---

## Create server

---

```javascript
const http = require('http'); // creates a server
const portNumber = 5000;
const server = http.createServer((req, res) => {
  // some code...
});
server.listen(portNumber);
```

Then run `node nameOfFile.js` in the terminal to start the server.  
Open the browser at **localhost:portNumber** to view the content.

The `createServer()` method takes a function that will be executed for every incoming request (e.g: call from the browser)

---

## Auto restart from the server: nodemon

---

Installing **nodemon**:  
`npm i -D nodemon`

In the _package.json_, in the _scripts_ object:  
`"start:server":"nodemon [nameOfFileToWatch]"`  
This will start the node server and watch the file changes and restart the server each time a change occurs

---

## Request

---

- Overview
- body-parser
- buffer

### Overview

Client => Request => Server => Response => Client

The **GET** method is the default method when making a request, entering an URL in the browser.

### body-parser

Will parse the incoming requests.  
Install **body-parser**:  
`npm i --save body-server`  
Declare it:  
`const bodyParser = require('body-parser')`

In the case of view rendering:  
`app.use(bodyParser.urlencoded())` // will parse data from html forms (form data)

For applications:  
`app.use(bodyParser.json())` // will parse json data

### Buffer

Buffer = bus stop: will allow us to transform data

---

## CORS: Cross-Origin Ressource Sharing

---

Ressources that are shared accross different URLs will not be accepted by the browsers by default. If the client runs on a different server than the backend then it is necessary to allow CORS policy.  
In the **app.js**:

```javascript
app.use((req, res, next) => {
  res.setHeader('Access-Control-Allow-Origin', '*'); // will allow any URL to access the APi
  res.setHeader(
    'Access-Control-Allow-Methods',
    'GET, POST, PUT, PATCH, DELETE'
  ); // will allow the specified HTTP methods
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization'); // will allow the client to set the header
  next();
});
```

---

## ExpressJs

---

Install Express.js as a depedency:  
`npm i --save express`

Import Express in the js file:  
`const express = require('express')`

The **express** package exports a function:  
`const app = express()` // app is a valid request handler

### Sending a response to the client/browser

Using the **send()** method to send back a response:

```javascript
app.use((req, res, next) => {
  res.send('<h1>Whatever text you like</h1>');
});
```

**Important :** the _next()_ method should never be called if a response if sent.

### Creating a server

```javascript
app.listen([portNumber]);
// replaces :
const server = http.createServer(app);
server.listen(portNumber);
```

---

## Response methods

---

- send
- redirect
- status
- sendFile
- status codes

### Send

`res.send()` // will send back some data

### Redirect

`res.redirect('/url')` // will redirect to the desired URL

### status

Will inform on the status of the request  
`res.status(404).send(<h1>Page not found</h1>)`

### sendFile

Will return a file.  
The **path** module needs to be imported to target the file:

```javascript
const path = require('path');
res.get('/', (req, res) => {
  res.sendFile(
    path.join(
      // creating a path
      __dirname, // global variable that holds the absolute path on the operating system to the current project
      '../', // needed if the file is in another folder
      'path1',
      'path2'
    )
  );
});
```

### Status code

- 200: ressource fetched successfully
- 201: ressource created successfully
- 401: unauthorized
- 404: ressource not found

---

## Middleware

---

Request => Middleware => Middleware => Response

**It basically is a big chain of middlewares.  
Each middleware can do something with the request.**

Function that runs between each request. It will execute some code on each request. It is useful for spliting the code into several functions(each of them being a middleware), instead of having one big function that does everything.

```javascript
const express = require('express');
const app = express();
app.use((req, res, next) => {
  // code to run...
  next();
});
```

It can also be used to filter the request. It will only work on a certain route if given the right parameter:

```javascript
const express = require('express');
const app = express();

app.use('/add-product', (req, res, next) => {
  // this middleware will only run if the URL matches the parameter '/add-product'
});
```

**next()** will let the next function/middleware being called. It **must not** be added if the middleware sends back a response.

Methods:
(they can be chained to the response function)

- use
- get
- post
- delete
- patch
- json
- status
- sendFile

### Use

**use()** will accept all incoming requests, all HTTP requests

### Get

**get()** will accept only get requests

### Post

**post()** will accept only post requests

### Delete

### Patch

### json

---

## Router

---

- Overview
- Targeting the root dir on every system

Adds routing to the app that can be slipped into different files:  
`const router = express.Router()`

Adding **Middleware**

```javascript
router.get((req, res, next) => {
  // code to run...
});
```

### Targeting the root dir on every system

Separate file ('path.js'):

```javascript
const path = require('path');
module.exports = path.dirname(process.mainModule.filename); // will target the root directory of any operation system
```

Router file:

```javascript
const rootDir = require('pathTo/path.js');
router.get((req, res, next) => {
  res.sendFile(path.join(rootDir, 'pathToHtmlFile'));
});
```

---

## Static files

---

Import CSS or JS files that don't require authorisation:

```javascript
const path = require('path');
const express = require('express');

app.use(express.static(path.join(__dirname, 'public')));
```

---

## Templating

---

- ejs
- handlebars
- pug

### Overview

Templating allows us to use dynamic html. The custom html file templates will be rendered as normal html files

### Using the templating engines

Import the template module via npm
`npm i --save ejs`

Set the engine in the app.js file

```javascript
app.set('view engine', 'ejs');
app.set('views', 'views');
```

In the controller file:

```javascript
res.render('nameOfTheFile', (variableName: value));
```

### ejs

Standard syntax:  
`<% %>`

Adding variables/constants:  
`<%= value %>`

Adding some html:  
`<%- include('pathOfFileToInclude') %>`

---

## Model View Controller (MVC)

---

- Models
- Views
- Controllers

### Models

Responsible for representing, managing the data(saving, fetching).  
Contains data related logic.  
They usually work as a Class, a blueprint of an object.

```javascript
module.exports = class Product {
  constructor(t) {
    this.title = t;
  }

  save() {
    products.push(this);
  }

  static fetchAll() {
    // the static ...
    return products;
  }
};
```

### Views

What the user sees. e.g: HTML files

### Controllers

Handles the logic between views and models, connects them. Should only make sure that the two can communicate.  
They are slipt between Middleware functions.

---

## Cookies

---

---

## Sessions

---

---

## Cross-Site Request Forgery (CSRF)

---

Avoid the sessions to get stolen.  
Install the **csurf** package
`npm i --save csurf`

---

## Showing errors

---

Using the **connect-flash** to show errors on authentication.  
Install the **connect-flash** package:  
`npm install --save connect-flash`  
In the **app.js** file:  
`const flash = require('connect-flash')`  
Declaring the flash function **after** the session has been initialized:  
`app.use(flash());`  
When checking if the authentication is valid and if the authentication is invalid:  
`req.flash('error', 'Invalid email or password...')`  
When rendering a page:

```javascript
res.render('auth/login', {
  pageTitle: 'Login',
  path: 'login',
  errorMessage: req.flash('error'),
});
```

Displaying the error message in the template:

```html
<% if(errorMessage){ %>
<div><%= errorMessage%></div>
<% } %>
```

---

## Mailing

---

Install **nodemailer** using npm:  
`npm i --save nodemailer`

- SendGrid

### Using SendGrid

Create an account and verify/validate the address that will send the emails.  
Get the API key from the **Settings** tab in the menu.
Install **nodemailer-sendgrid-transport** using npm:  
`npm i --save nodemailer-sendgrid-transport`

### Sending email

In the controller file that will send the email:
Import nodemailer and nodemail-sendgrid-transport  
`const nodemailer = require('nodemailer');`
`const sendGridTransport = require('nodemailer-sendgrid-transport');`

Declare a transporter variable that will hold the API key.

```javascript
const transporter = nodemailer.createTransport(
  sendGridTransport({
    auth: {
      api_key:
        '...',
    },
  })
);
```

In the middleware:

```javascript
transporter
  .sendMail({
    to: 'address to send the mail to',
    from: '...',
    subject: `any string...`,
    html: ``,
  })
  .then((response) => {
    console.log('response from mailer:', response);
    res.status(201).json({
      message: 'Message sent successfully',
    });
  })
  .catch((err) => console.log('mailer error response: ', error));
```

---

## Validation

---

Official doc: **https://express-validator.github.io/docs/**

Install **express-validator**:  
`npm i --save express-validator`

In the routes, import the sub-modules from the **express-validator** module using _object destructuring_:  
`const {check, body} = require('express-validator')`  
Then use it in the route:  
`router get('url', check(email).isEmail().withMessage('Please enter a valid email'), postSignUp)` // will check the email entered and return errors if not valid.  
`router get('url', [body('title').trim().isLength({min:5}).body('content').trim()], postSignUp)` // will check the title and content properties for white spaces and mininum length.

In the controller file, import the sub-module:

```javascript
const { validationResult } = require('express-validator');
// in the middleware:
const errors = validationResult(req);
if (!errors.isEmpty()) {
  return res.status(422).render('auth/signup', {
    pageTitle: 'Signup',
    path: 'signup',
    errorMessage: errors.array()[0].msg,
  });
}
```

### List

- check()
- body()
- isEmail()
- trim() // deletes white spaces
- normalizeEmail() // checks for email
- custom()
- isLength({min:number, max: number})
- isURL() // check for URL
- isString() // accepts white spaces

### Sanitizing

- trim()
- normalizeEmail()

---

## Error handling

---

Insert code into the **catch** blocks, not just only a console.log but a redirect to a 500 page with some clear messages.  
Use the error middleware:  
`app.use(error, req, res, next)...`

---

## Token

---

Import **crypto** from express:  
`const crypto=require('crypto')`

---

## File upload

---

Install **multer**:  
`npm i --save multer`

In the **app.js**:  
Import and use **multer**  
`const multer = require('multer')`
`app.use(multer().single('[inputNameToBeChecked]'))`

Will return the data of the file.
**Buffer**: The **buffer** property is like a _bus stop_. It will return some data(the file in some format). We can then work with that data and turn it into a file.

```javascript
app.use(
  multer({
    dest: '[folderName]',
    storage: '',
  }).single('[inputNameToBeChecked]')
);
```

---

## REST API

---

**Representational State Transfer**
Transfer Data instead of User Interfaces. They don't render views, only data.

**Endpoints**: http methods + /path

### HTTP Methods

- get
- post
- put
- patch
- delete

---

## Web Socket

---

### Backend code

`npm i --save socket.io`
.emit() // will emit to all client
.broadcast() // will emit to all client except the one making the request

### Front code

`npm i --save socket.io-client`

---

## GraphQL

---

---

## Deploying AWS

---

Upload the .zip folder containing the entire app.  
Set the environment variables in the Software section.  
Set up the ip address of AWS in the Mongo cluster.
