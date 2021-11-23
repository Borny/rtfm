# Typescript

- About
- Install
- Compile
- Configuration
- Core Types
- Type Assignment
- Type Inference
- Object Types
- Array Types
- Tuple
- Enum
- Union Types
- Literal Types
- Type Aliases
- Function Return Types
- Function as Types
- Unknown
- Never
- Classes
- Interface
- Intersection Types
- Type Guards
- Discriminated Unions
- Type Casting
- Function Overload
- Optional Chaining
- Nullish Coalescing
- Generic => ???? **TO COMPLETE**
- Decorator => ???? **TO COMPLETE**
- Namespaces
- ES Modules

## About

Typescript types are checked during compilation, Javascript types are during runtime.

## Install

Global install:

`npm i -g typescript`

## Configuration

- Init
- Exclude/Include
- Source Map
- rootDir
- outDir
- removeComments
- noEmit
- noEmitOnError

### Init

`tsc --init`  
Will create a **tsconfig.json** file that will configure the settings for the project.

### Exclude/Include

In the **tsconfig.json** file:

```json
{
  "compilerOptions": {
    ...
  },
  "exclude":[ // will not compile the files listed in the exclude array
    "someFile.ts"
  ],
  "include": [ // will only compile the files listed in the include array
    "pathToFile",
    "*.ts" // all .ts files in the root folder
    "**/*.ts" // all .ts files in any folders
  ]
}
```

=> If the _include_ property is not set then all files will be compiled by default
=> By default, the **node_modules** folder will not be compiled

### Source Map

The **sourceMap** option will generate the compiled _.ts_ file so that it can be accessed in the **sources** tab in the developper tools. It is very convinient for debugging.

```json
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

### rootDir

When specified, will only target .ts files in that folder:

```json
{
  "compilerOptions": {
    "rootDir": "./src" // will target the .ts files in the "src" directory
  }
}
```

### outDir

When specified, will output the compiled files in that folder:

```json
{
  "compilerOptions": {
    "outDir": "./dist" // will place the .js files in the "dist" directory
  }
}
```

### removeComments

If set to **true**, will not include the comments in the .js file:

```json
{
  "compilerOptions": {
    "removeComments": true
  }
}
```

### noEmit

If set to true will check the files and will output errors in the console but will not compile them:

```json
{
  "compilerOptions": {
    "noEmit": true
  }
}
```

### noEmitOnError

If set to true will not compile the files if there is any error:

```json
{
  "compilerOptions": {
    "noEmitOnError": true
  }
}
```

## Compile

- Init
- Compile one file
- Compile all files
- Compile all files
- Watch Mode

### Compile one file

Compiling a .ts file into a .js file(will generate the .js file in the same folder):

`tsc [nameOfFileToCompile]`

### Compile all files

Compiling all the .ts files in the project:  
`tsc`

### Watch Mode

Watch Mode will tell Typescript to _watch_ a file or all of them, so any changes made to that file will result in the compiler running automatically:

`tsc [nameOfFile] --watch` **or** `tsc [nameOfFile] -w`

Watch **all** files:  
`tsc --watch` **or** `tsc -w`

## Core Types

- string => all text values
- number => all numbers with no differenciations between integers or floats
- boolean => true or false only (no truthy or falsy values)
- object => any Javascript objects
- array => any Javascript arrays
- tuple (**Typescript only**) => fixed-length array
- enum (**Typescript only**) => Automatically enumerated global constant identifiers
- any (**Typescript only**) => will accept all values. **AVOID IT WHENEVER POSSIBLE**

## Type Assignment

Assigning a type to a variable is done by using a colon(:) after the variable name:

```typescript
let id: number;
function(paramOne: number, paramTwo: string){}
```

## Type Inference

Adding the type to an assigned variable is redundant and not considered best practice as Typescript will infer/detect the type on its own:

```typescript
let id: number; // this is OK
const id: number = 1234; // this is useless and souldn't be done
const id = 1234; // Typescript will know that the type is not only number but 1234
```

## Object Types

Object types can be defined with **key: type** pairs:

```typescript
const person: { name: string; age: number } = {
  name: 'Peter',
  age: 23,
};
```

## Array Types

Array types are defined using the **[]** symbols:

```typescript
const hobbies: string[] = ['Surfing', 'Coding']; // Typescript expects all values to be strings
const hobbies: string[] = ['Surfing', 78]; // the compiler will throw an error as one of the value is not a string
```

## Tuple

A **Tuple** will defined a fixed-length array:

```typescript
const hobbies: [string, number] = ['Spike', 30]; // hobbies will accept only an array with two values, the first must be a string and the second a number
const hobbies: [string, number] = ['Spike']; // compile error as a value is missing
const hobbies: [string, number] = ['Spike', true]; // compile error as the second value is not of type number
const hobbies: [string, number] = ['Spike', 20, 'Faye']; // compile error as the length of the array is 3 (expected 2)
```

## Enum

An **enum** will assign values to global constants. It will assign numbers by default but can assign any values if told to do so:

```typescript
// default values
enum Role {
  ADMIN,
  USER,
  SUPERADMIN,
}
const employee = {
  role: Role.ADMIN, // role will have a value of '0' as ADMIN is the first entry in the enum Role type
};

// assigned values
enum Role {
  ADMIN = 'Admin',
  USER = 'User',
  SUPERADMIN = 'Superadmin',
}
const employee = {
  role: Role.USER, // role will have a value of 'User' as USER is assigned the value 'User'
};
```

## Union Types

Will allow to have multiple types:

```typescript
let id: string | number; // here id can be of type string or number

// both assignments will be correct
id = 29;
id = 'slkj398798lmkj_lmk';
id = false; // will result in an error
```

## Literal Types

Usefull when we know the exact value of a type:

```typescript
let id: 'onetwothree' | 'fourfivesix'; // here id must be of type string but even more specific 'onetwothree' or 'fourfivesix'
```

## Type Aliases

Custom types can be used with aliases:

```typescript
type CustomTypeName = number | string;
type CustomTextTypeName = 'some-text-value' | 'some-other-text-value';

let id: CustomTextTypeName;
```

## Function Return Types

Functions can be assigned a **return** type:

```typescript
function returnAString(firstName: string, lastName: string): string {
  return firstName + lastName;
}
```

If the function doesn't return anything then the **void** type can be used:

```typescript
function doSomethingWithNoReturn(firstName: string, lastName: string): string {
  alert('See you later ');
}
```

## Function as Types

A function can be used as a type:

```typescript
function add(n1: number, n2: number): number {
  return n1 + n2;
}

// combineValues is expected to be a function with two arguments of type number and to return a number
let combineValues: (a: number, b: number) => number;

combineValues = add;
console.log(combineValues(4, 5)); // expected result 9
console.log(combineValues(4, 'sdlfjs')); // will throw an error
```

## Unknown

The type **unknown** should be used isntead of **any** but might require an extra type checking:

```typescript
let userInput: unknown;
let userName: string;

// Checking the type of userInput to make sure it can be assigned to userName
if (typeof userInput === 'string') {
  userName = userInput;
}
```

## Never

The type **never** can be used for functions that while not returning anything will also crash the script like an error function:

```typescript
function generateError(errorMessage: string, code: number): never {
  throw { message: errorMessage, errorCode: code };
}

// The function will not return anything, not even "undefined"
console.log(generateError('An error occured !', 500));
```

## Classes

- Definition
- Private/Public
- Extends
- Protected
- Static

### Definition

**Classes** were introduced with ES6. They are _blue prints_ for objects. They are syntactic sugar and are built on **prototypes**.

```typescript
class Avengers {
  members: string[] = ['Steve', 'Bruce'];

  constructor() {}

  getEmployees() {
    return this.employees;
  }
}
```

### Private/Public

By default all properties and methods inside a Class are public,i.e: they can be accessed by all instantiations of the Class.  
A **private** property or method can only be accessed inside the Class.

```typescript
class Avengers {
  private members: string[] = ['Steve', 'Bruce']; // can only be accessed inside the Avengers Class

  constructor() {}

  public getMembers() {
    // can be call on any instance of the Class
    return this.members;
  }
}

const avengers = new Avengers();
console.log(avengers.members); // will throw an error as 'members' can't be accessed on an instance
console.log(avengers.getMembers()); // will return the array of members
```

### Extends

The keyword **extends** allows the duplication of a Class:

```typescript
class Avengers {
  private members: string[] = ['Steve', 'Bruce']; // can only be accessed inside the Avengers Class

  constructor() {}

  public getMembers() {
    // can be call on any instance of the Class
    return this.members;
  }
}

class NewAvengers extends Avengers {
  // the NewAvengers Class will have all the public properties and methods of the Avengers Class
}

const avengers = new Avengers();
console.log(avengers.members); // will throw an error as 'members' can't be accessed on an instance
console.log(avengers.getMembers()); // will return the array of members
```

### Protected

TODO: complete

=> Protected properties and methods are like Private but will be available in the extended Classes

### Static

TODO: complete
=> Static properties and methods can be called without instantiating the Class. They are like global variables/methods

## Interface

- Definition
- Readonly

### Definition

**Interfaces** describe the structure of an object with properties and methods:

```typescript
interface Villain {
  name: string;
  age: number;

  greet(phrase: string): string;
}

const Loki: Person = {
  name: 'Loki',
  age: 5000,
  greet(phrase: string) {
    console.log(phrase + ' ' + this.name);
  },
};

Loki.greet('Hey there');
```

### Readonly

Readonly is for properties/methods that can't be reassigned a value:

```typescript
interface Villain {
  name: string;
  readonly age: number;
}

const Loki: Person = {
  name: 'Loki',
  age: 5000,
};

Loki.age = 7000; // will throw an error as the age has already been set
```

## Intersection Types

Intersection Types allow the combination of multiple Types (or Interfaces):

```typescript
type Admin = {
  name: string;
  privileges: string[];
};

type Employee = {
  name: string;
  startDate: Date;
};

type ElevatedEmployee = Admin & Employee;

const employeeOne: ElevatedEmployee = {
  name: 'Thor',
  startDate: new Date(),
  privileges: ['God', 'Son', 'Half-brother'],
};
```

## Type Guards

Checking for a property type using the **in** keyword, before doing something with it:

```typescript
interface Admin {
  name : string;
  privileges: string[];
}

interface Employee {
  name: string;
  startDate: new Date();
}

type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
  console.log('Name: ' + emp.name);
  if ('privileges' in emp) {
    console.log('Privileges: ' + emp.privileges);
  }
  if ('startDate' in emp) {
    console.log('Start Date: ' + emp.startDate);
  }
}
```

## Discriminated Unions

TODO: improve the description
Will check for a certain type

```typescript
interface Bird {
  type: 'bird';
  flyingSpeed: string;
}
interface Horse {
  type: 'horse';
  runningSpeed: string;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  switch (animal.type) {
    case 'bird':
      console.log(animal.flyingSpeed);
      break;
    case 'horse':
      console.log(animal.runningSpeed);
  }
}

moveAnimal({ type: 'bird', flyingSpeed: '2000' });
```

## Type Casting

Tell Typescript of what type an element(not a variable) will be:

```html
<p>Some paragraph</p>
```

```typescript
const paragraph = document.querySelector('p');
paragraph.innerHTML = 'Some text'; // this will throw an error as Typescript doesn't know that p has the innerHTML property

// both the following syntaxes are correct
const paragraph = <HTMLParagraphElement>document.querySelector('p');
const paragraph = document.querySelector('p') as HTMLParagraphElement;
```

## Index Properties

When we don't know the value and number of properties the object will have:

```typescript
interface ErrorContainer {
  [props: string]: string;
}

const error: ErrorContainer = {
  // it can have any properties but they all must be of type string
  email: 'Invalid email !',
  password: 'Not long enough...',
  name: boolean, // will throw an error as boolean is not an expected type
};
```

## Function Overload

List the possible outcomes of a function:

```typescript
type Combinable = string | number;

// list the possibilities
function adding(a: number, b: number): number;
function adding(a: number, b: string): string;

// Describe the function
function adding(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}

const result = adding(1, 'steve');
result.split(' ');
```

## Optional Chaining

```typescript
const fetchedUser = {
  id : 001
  name: 'Jet',
  job: {title: 'Bounty hunter', alias : 'Cowboy', coolLevel: '9'}
}

// If job doesn"t exist if will throw an error at runtime
console.log(fetchedUser.job.title)

// The Javascript way of checking a value: check for some value first
console.log(fetchedUser.job && fetchedUser.job.title)

// The Typescript way of checking a value: add the question mark ("?") after the value that might be missing
console.log(fetchedUser.job?.title) // here it
```

## Nullish Coalescing

Check if a value is **null** or **undefined** only:

```typescript
const fetchedData = '';
const result = fetchedData ?? 'Default value';
console.log(result); // will print out an empty string as fetchedData is neither null nor undefined

const fetchedData = null;
const result = fetchedData ?? 'Default value';
console.log(result); // will print out Default value as fetchedData is null
```

## Generic

Type of another Type

## Decorator

## Namespaces

Will allow the App to splitted into multiple files.

Creating a file to import:

(user-interface.ts)

```typescript
namespace App {
  export interface User {
    id: string;
    name: string;
    age: number;
  }
}
```

Main file:

```typescript
// Importing the interface
/// <reference path="user-interface.ts"/>

namespace App {
  // The namespace must be the same
  // The interface can then be used
  const user: User = { id: '0.79865', name: 'Spike', age: 34 };
}
```

By default **typescript** will compile all the file separately. They can all be merged into the same file by updating the **tsconfig.json**:

```json
"module": "amd",
"outFile": "./folderName/fileName.js" => i.e: ./dist/bundle.js
```

Then in the **index.html**, update the src attribute for the js file to : <script src="dist/bundle.js">

## ES Modules

Like **namespaces**, it allows the App to be splitted into multiple files but using a different syntax:

File to import:

(project.ts)

```typescript
export class Project {
  constructor(
    public id: string,
    public title: string,
    public description: string,
    public people: number,
    public status: ProjectStatus
  ) {}
}
```

Main file:

```typescript
import { Project } from 'pathToFile.js'; // => !!IMPORTANT!!: .js should be added as the compiled code will need it in the end

const newProject = new Project(
  Math.random().toString(),
  'whatever',
  'like I said',
  1000,
  'active'
);
```

Updating the **tsconfig.json**:

```json
"module": "es2015",
// "outFile": "./folderName/fileName.js" => should be commented out
```
