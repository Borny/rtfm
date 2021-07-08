# JS

```javascript

```

- Expression vs Statement
- Quotes
- Data Types
- Importing scripts in HTML
- Errors
- Consoles
- Heap / Stack
- Primitive vs Reference values
- Operator
- Spread
- Rest
- && ||
- Decimal Base Exponents
- Variables
- Functions
- Loops
- if statement
- Operators tricks: !! && || ??
- DOM
- Arrays
- Sets
- Maps
- Objects
- Classes
- Events
- setTimeout()
- setInterval()
- location
- history
- RegEx
- Event Loop
- Promise
- Async / await
- JSON
- Http request
- Modules
- File Reader

---

## Expression vs Statement

---

**Expression** is a piece of code that can be stored in a variable:
`const whateverName = value` => expression  
**Statement** cannot be stored, it cannot be placed after the equal/= sign
`if(condition){expression}` => statement

---

## Quotes

---

HTML file : **double quote** ""  
JS file : **single quote** ''  
Template literal: back ticks => ``, multiple lines and variables allowed `\${variable}` // variables/expressions can be injected inside the curly brackets
Line break : **\n**

---

## Data Types

---

- **Numbers**: integer, float, negative
- **Strings**: '', "", ``
- **Booleans**: true / false
- **Objects** : {key: value} // also called 'property' or 'field'
- **Arrays**: []
- **Undefined**
- **null**
- **NaN**: Not a number (type number)

### Casting

Changing a type to another. i.e: number to string.  
`[value].toString()` // turns a number to a string  
`+string` // turns a string to a number

### typeof

Find the type of a variable, value:  
`typeof 10` => **number**  
`typeof 'anyString'` => **string**  
`typeof []` => **object**  
`typeof {name: 'peter', aka: 'spiderman'}` => **object**  
`typeof true` => **boolean**
`typeof []` => **object**

### isNaN()

Will check if the value is not a number and return a boolean: `isNaN([value])`

### toFixed()

Will convert a number to a string with a defined number of decimal. Will also round up or down the result.  
`const number = 2.49857`  
`number.toFixed(3) // '2.499'`

---

## Importing scripts in HTML

---

- defer
- async

Import the script in the head tag:
`<script src='index.js'></script>`

### Defer

This attribute will tell the browser to load the script as soon as possible but will let the HTML parser to continue at the same time and will wait before the HTML is loaded before executing the script
`<script src='index.js' defer></script>`

### Async

This attribute will do the same as **defer** but will execute the script as soon as it is loaded
`<script src='index.js' async></script>`

---

## Errors

---

### throw :

```javascript
throw { message: 'message to print' };
```

### try - catch :

```javascript
try {
  // code to test (e.g: http request)
} catch (error) {
  // fallback if the previous code doesnt work
} finally {
  // code that should run no matter what (rarely used)
}
```

---

## Consoles

---

Consoles are used to print variables values, messages, function results... in the browser console or the terminal

`console.[method]`

- log
- table
- error
- warn
- clear
- time, timeEnd
- count
- group, groupEnd
- styling the logs

### log

Will simply log the desired value:  
`console.log('anything')` => will log _anything_ in the console

### table

Will display the values of an array as a table:  
`const array = [1,2,3,4]`  
`console.table(array)` => will log the _array_ variable in a table in the console

### error

Will display an error by being highlighted in red:
`console.error('error message')` => will log _error message_ in red in the console

### warn

Will display a warning by being highlighted in yellow:
`console.warn('warning message')` => will log _warining message_ in yellow in the console

### clear

Will clear the console:
`console.clear()` => will display _console was cleared_

### time, timeEnd

Will display the amount of time the code took to run, they should use the same label:

```javascript
console.time('timer')
const functionName = () => functionBody...
functionName()
console.timeEnd('timer')
```

=> will display the time it took for the code to execute in seconds

### group, groupEnd

Will display the content in a separate block and which will be indented, they should use the same label:

```javascript
console.group('group1');
console.warn('warning');
console.error('error');
console.log('I belong to a group');
console.groupEnd('group1');
console.log('I dont belong to any group');
```

### styling the logs

Use CSS and the **%c** parameter to style the logs:

```javascript
const spacing = '10px';
const styles = `padding: ${spacing}; background-color: white; color: red; font-style: italic; border: 1px solid black; font-size: 2em;`;
const styledLog = 'I am a styled log';
console.log(`%c${styledLog}`, styles);
```

=> will display the message with some styling

---

## Heap / Stack

---

The **Heap** is the memory allocated by the browser to our code. **Long living memory**  
The **Stack** is the execution order of all the functions of the code.
**Short living memory**  
Each function will be executed one by one, hence will be placed on top of the stack. Once a function is done executing, it will be removed from the stack. The entire code can be imagined as wrapped in an **anonymous** function, this one will be at the base of the stack.

---

## Primitive vs Reference values

---

**Primitive** values: string, booleans, number, undefined, null, symbol

**Reference** values: the address to the reference...

---

## Operators

---

`let variable1 = 10`  
`let variable2 = 5`

`variable1 = variable1 + variable2`  
**is equivalent to**  
`variable1 += variable2`

The same goes for the other math operators:

```javascript
variable1 -= variable2;
variable1 *= variable2;
variable1 /= variable2;
```

`variable1 + 1` and `variable2 - 1`  
**are equivalent to**  
`variable1++` and `varible2--`

== will check the value  
=== will check the value **and** the type

### Operator precedence

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

In general, math operators have precedence on the logic operators. e.g : **+** has precedence over **&&**.

**Note**
`console.log(variable1++)` will return the result before the operation  
`console.log(++variable1)` will return the result after the operation

### Falsy values

the number `0`  
the BigInt `0n`  
the keyword `undefined`  
the keyword `null`  
the number `NaN`  
the boolean `false`  
the empty string `“”` (equivalent to `` or ‘’)

### Truthy values

Every other values

---

## Spread

---

Will make a copy of an array(like the slice() method) or an object.  
`const array = [1,2,3]`  
`copiedArray = [...array]` => new array  
`const person = {name: 'Max', age: 45}`  
`copiedPerson = {...copiedPerson}` => new object

---

## Rest

---

Merges arguments into an array.

```javascript
const toArray = (...arguments) => {
  return arguments;
};

toArray(1, 2, 3, 4); // output [1,2,3,4]
```

---

## && ||

---

### && Operator

&& will check if all values are truthy. It will return the first falsy value or the last value if there is no falsy value:  
`console.log(1 && false && {})` // false as it is the first falsy value  
`console.log([] && 'true' && 1)` // 1 as there are no falsy value

### || Operator

|| will check if at least one the values are truthy. It will return the first truthy value or the last one if they are all falthy:  
`console.log(undefined || false || On)` // On as all the values are falthy  
`console.log(NaN || 66 || '')` // 66 as it is the first truthy value

---

## Decimal Base Exponents

---

Shorthand syntax for numbers:
`1e0` = 1  
`1e1` = 10  
`1e3` = 1000  
etc...

---

## Variables

---

- Global scope
- Local scope
- Shadowing
- Strict mode

**let** => variable, can be reassigned  
**const** => constant, cannot be reassigned

**float** => decimal number  
**integer** => non decimal number

Variables need to be declared before they are used

### Global scope

Any variable that is declared on the **main** scope, outside functions, if statements etc...It will be available everywhere in the code.  
The global scope cannot access local variables/constants.

### Local scope

A variable has a local scope if it is declared inside a function, loop, if statement. It will only be available inside that _scope_.  
The local scope can access global variables/constants.

### Shadowing

A variable can be redeclared (same name) inside a function, loop, if... Javascript will then override the global variable and use the newly created one but only inside the local scope

### Strict mode

The strict mode `'use strict'` is used to avoid errors in a file. It mainly concerns the ES5 syntax. Use it at the beginning of a file or function to enable it. The browser will then know it will not allow certain syntax errors like forgetting to use the _var_ keyword: varName = 'value' => the var keyword is missing

---

## Functions

---

- Overview
- Method
- Naming
- Function expression
- Function declaration / function statement
- Anonymous function
- Arrow function
- Bind
- Default parameter
- Return keyword
- pure function
- factory function
- closure
- recursion

### Overview

The file will be read from top to bottom before being executed, it will then run the desired functions. So a **function** can be declared at the bottom of a file and be called before its declaration (only in the case of a **function declaration**).
But it is a good practice to declare the function before calling it, it makes the code easier to read.
The function body (inside the curly braces) takes **expressions**  
alert(result);
No need for ';' after a **statement** or **function**

Adding parenthese right after the function name will result in the function being executed right away, when the function is read.

### Method

A **method** is a function attached to an object.

### Naming

`functionNameHandler(){}`  
The word _Handler_ can be used to name any function that is associated with an event (handler)

### Function expression

A **function expression** is a function that is declared in a variable:  
`const functionVariable = function(){}`

### Function declaration / function statement

A **function declaration** is a function declared on the global scope:  
`function functionName(){}`

### Anonymous function

A function that has no name: `function(){}`  
It can be used when the function is only used in one place, e.g: event.
`button.addEventListener('click', function(){ // code to execute})`
or when in a function expression:
`const functionVariable = function(){}`

### Arrow function

Basic syntax: `() => {}`
No need for the _function_ keyword anymore.

### Bind

Used to **prepare** a function. It allows to **pass arguments** to a function **without executing** it straight away, _to be called functions_, function that you don't call yourself but are called by another function/event (callback) :
As a callback :  
`function1(callBackFunction.bind(this, bindParam))`  
Declaring the **function1**

```javascript
function1(callBack, otherParam){
  callBack(otherParam)
}
```

Callback function:

```javascript
const callBackFunction = (bindParam, otherParam){
  console.log(`${bindParam} ${otherParam}`)
}
```

### Default parameter

A function can take default parameter in case an argument is not provided and therefore becomes undefined:  
`functionName(param1, param2 = DEFAULT_VALUE){}`

### Rest operator

Using the three dot notation (...), it will concatenate arguments into a single array:
`const functionName = (...argumentName) => {}`
`functionName(1,2,3,4,5)` // argumentName = [1,2,3,4,5]

### Callback functions

A **callback** is a function that is used as an argument by another function. It is not directly called by the developper, it is a _to be called function_. e.g: `btn.addEventListener('click', callBackFunction)`

### Return keyword

The **return** keyword will end the function. Nothing will be executed after a return statement

### Pure function

A function is considered **pure** when it returns the same output with the same input and that it doesn't affect any other values outside the function.

### Factory function

A function that returns another function.Useful when a function is called several times in an application... **// TO COMPLETE**

### Closure

Every functions are closures. They **close** around the variables present inside them... **// TO COMPLETE**  
Functions can refer to "external" variables (i.e. in a different lexical environment) when they execute - even if those variables weren't used outside of the function.

### Recursion

A function calls itself

---

## Loops

---

- for
- for of
- for in
- while
- do while
- break/continue
- labeled statement

### for

Most common loop in every programming language:

```javascript
for (let i = 0; i < limit; i++) {
  // execute code on each loop
}
```

### for of

Specific to arrays:

```javascript
const list = [item1, item2, item3];
for (const item of list) {
  // execute code on each item
}
```

### for in

Specific to objects:

```javascript
const object = { key1: value, key2: value, key3: value };
for (key in object) {
  // execute code on key:value pair
  // e.g: console.log(key) // will output the keys
  // e.g: console.log(object[key]) // will output the keys values
}
```

### while

Will continue to execute as long as a condition is true, but will check the condition before executing the code:

```javascript
let condition = true;
while (condition) {
  // execute code
  condition = false;
}
```

### do while

Same as **while** but will execute the code **then** check for the condition:

```javascript
let condition = true;
do {
  // execute code
} while (condition);
```

### break/continue

The keyword **break** will stop the loop to which it belongs, the **continue** keyword will skip the loop to which it belongs but will continue after

### labeled statement

Loops can be labeled :

```javascript
outerLoop: for (let i = 0; i < 2; i++) {
  // execute code here
}
```

---

## if statement

---

If statements can run a certain code if a condition is met. Only one block can run in an if statement. None will run except the **else** block if the conditions aren't met.

```javascript
if (conditionIsTrue) {
  // code to run
} else if (conditionToCheck) {
  // code to run
} else {
  // code to run if all the conditions fail
}
```

### Coercion

If the value given as a condition isn't a boolean, the if statement will **coerce** the value by temporarily converting it to **truthy** or **falsy**.

### Optional Chaining

Avoid using the **if** statement by using the question mark operator to check for undefined or null:  
`console.log(user?.hobbies?.summer)` // will check if the user exists, then if the hobbies property exists. If not it will not return anything but also not throw an error.

---

## Operators tricks: !! && || ??

---

### double bang !!

The double exclamation marks/!! will coerce a value into a real boolean.  
`const name = 'table'` => name is truthy.  
`!!name` => name is true.

### Setting a default value to a variable using ||

`const name = someInput || defaultValue` => if someInput is undefined then the defaultValue will be assigned.

### Set a value with &&

`const name = condition && valueToAssign` => if condition is true then the valueToAssign will be assigned.

### Set a value with ??

Will check for nullish values:  
Instead of using **if**, use the double question mark operator(??) to assign a value:

```javascript
const darkModePreference1 = true;
const darkModePreference2 = false;
const darkModePreference3 = undefined;
const darkModePreference4 = null;

const getUserDarkModePreference = (darkModePreference) => {
  return darkModePreference ?? true;
};
```

---

## DOM

---

- window
- document
- DOM
- Nodes
- Elements
- Attributes
- Properties
- Querying the DOM
- Traversing the DOM
- Styling
- Create element
- Clone element
- importNode()
- Remove element
- data-\*
- position
- element height
- element width
- scroll
- scrollIntoView()
- scroll animation
- template tag
- Add script tag

### window

window object that represents the tab of the browser

### document

document object. Part of the **window** object

### DOM

**Document Object Model**
Javascript is a **hosted language**, the browser exposes this DOM API to be used by Javascript.  
The browser _reflects_ the HTML code and each tag becomes an **element node** or **element**. Each text also becomes a node or **text node**

### Nodes

Objects that make up the DOM

### Elements

Elements are **one type of nodes**. e.g: `<div>, <main>, <ul>`

### Attributes

Attributes placed in the HTML code on element tags. e.g: id, class, value...
`element.id = someId` // will add an id attribute to the element

### Properties

Properties reflecting the attributes on the DOM objects : id="" = element.id

### Querying the DOM

### Traversing the DOM

Child : direct child
Descendant: any child node/element that comes after

- **firstChild**
- **firstElementChild**
- **childNodes**
- **children**
- **querySelector()**
- **lastChild**
- **lastElementChild**

Parent: direct parent
Ancestor: any parent node/element that comes before

- **parentNode**: will include the textNodes
- **parentElementNode**: will target only the HTML element
- **closest**(): selects the nearest ancestor element

Siblings: any element that is on the same level

- **previousSibling**
- **previousElementSibling**
- **nextSibling**
- **nextElementSibling**

### Styling

- style attribute
- className
- classList

### Style attribute

Adds the **style** attribute to an element, then add a CSS like property.  
`element.style.backgroundColor = '#ccc'`

### className

Adds a class name.  
`element.className = 'className'`

### classList

Property that has methods that make it easier to manage classes.  
`element.classList.add = 'className'` : will add a class  
`element.classList.remove = 'className'` : will remove a class  
`element.classList.toggle = 'className'` : will toggle a class

### Create element

- **createElement()**: will create a new elementby giving it a tag name (li, div, p, section). Can be **only** called on the document object : e.g: `document.createElement('main')`
- **innerHTML** : will add a new content and replace any existing content
- **appendChild()**: will add an element inside the targeted(parent) element.`element.appendChild(childName)`
- **append()**: will add elements or text. `element.append(element,'some text'...)`
- **textContent**: will add text to the element. `element.textContent = 'text to display'`
- **inserAdjacentElement()**: will add an element to the desired position. `element.inserAdjacentElement('position', objectElement)` | afterbegin,afterend, beforebegin, beforeend

### Replace element

- **replaceWith()**
- **replaceChild()**

### Clone element

- **cloneNode(true | false => default)**: will clone an element with or without all of its content.

### importNode()

Will create a copy of an element. Needs to be inserted into the document using **append**() or **appendChild**(). The second argument will import or not all of the original node's content.
`element.importNode(nodeToCopy, true | false(default))`

### Remove element

- **remove()**: removes an element
- **removeChild()**: removes an element
  `element.remove()`

### data-\*

### position

Indicates the positon relative to the **document** (not the window) from :

- top: `element.offsetTop`
- bottom: `element.offsetBottom`
- left: `element.offsetLeft`
- right: `element.offsetRight`

### element height

`innerHeight` // best to avoid as it doesn't include scrollbars  
`element.clientHeight` // will exclude borders, will include scrollbars  
`element.offsetHeight` // will include borders

### element width

`element.clientWidth` // will exclude borders  
`element.offsetWidth` // will include borders

### scroll

**scrollTo(valueX, valueY)** Scroll to a desired value (x or y):  
`element.scrollTo(0, 50)` // will scroll vertically to 50px from the top of the element  
`element.scrollTo(50, 0)` // will scroll horizonrally to 50px from the left of the element

**scrollBy(valueX, valueY)** Scroll by a desired value relatively to the element's position

### scrollIntoView()

Will make an element visible into the view.
`element.scrollIntoView()`

### scroll animation

Object that adds animation to the scrolling (Chrome and Firefox only)
`{behavior: 'auto(default) | smooth'}`

`element.scrollTo({top: 0, behavior: 'auto(default) | smooth'})`  
`element.scrollIntoView({behavior: 'auto(default) | smooth'})`

### template tag

Will create a **template** tag that can hold some HTML code that won't be displayed by default.  
`<template id='some-id'><h2>Some heading</h2><p>Some text</p></template>`

Accessing the template in the JS file:

```javascript
const templateElement = document.getElementById('some-id');
const templateBody = document.importNode(templateElement.content, true);
templateBody.textContent = 'some text';
templateElement.append(templateBody);
```

### Add script tag

Script tags can be added like any other tag.
`const script = document.createELement('script');`  
`script.src = './assets/js/main.js';`  
`script.defer = true;`  
`document.head.append('script')`  
This will insert a script inside the head tag

---

## Arrays

---

- Square brakets notation
- Array()
- Array.of()
- Array.from()
- push()
- unshift()
- pop()
- shift()
- [index]
- splice()
- slice()
- concat()
- indexOf()
- find()
- findIndex()
- includes()
- forEach()
- map()
- sort()
- reverse()
- filter()
- reduce()
- split()
- Spread operator
- Array destructuring
- Object to array

### Square brackets notation

Most common way to create an array:
`const newArray = [1, 2, 'string', {key: value}]`

### Array()

Same logic as the square brackets notation:
`const newArray = Array(1, 2, 'string', {key: value})`

### Array.of()

### Array.from()

Will take an Array-like object (e.g: nodeList) and will transform it into a real array with all the available methods associated with arrays

### push()

Will add a new element to the end of an array. Will **return** the length of the array after adding the element:
`customArray.push(value)`

### unshift()

Will add a new element at the beginning of an array. Will **return** the length of the array after adding the element:
`customArray.unshift(value)`

### pop()

Will remove the last element of an array. Will **return** the removed element:
`customArray.pop()`

### shift()

Will remove the first element of an array. Will **return** the removed element:
`customArray.shift(value)`

### [index]

Will insert an element at the desired index, but will replace any existing value at that same index

### splice()

Will remove, add or replace an element depending on the parameters passed to it:  
The first parameter is the index the method should work on.  
The second parameter tells how many elements should be deleted:  
`array.splice(0,0)` : won't do anything  
`array.splice(0,1)` : will delete the element at index 0  
`array.splice(-1,0)` : will remove the last element  
`array.splice(0,1,5)` : will replace the element at index 0 with the number 5
Will **return** the removed element(s)  
If using negative indexes, it will remove the element from the end of the array

### slice()

**Returns** a brand new array.  
Copies an array that will not be affected if the original array is modified.  
`const copyArray = originalArray.slice()`  
Returns a chunk of an array with parameters:  
`const newArray = originalArray(0,2)` : will return an array starting at the first index to the second index(not included), so 2 values will be returned in the new array

### concat()

Will return a new array by adding the values from an array to the end of an existing array:  
`const array1 = [1,2,3,4]`
`const array2 = array1.concat([5,6,7,8])`

### indexOf()

Will look for a certain element in an array.  
Will return the index of the element if it exists, otherwise will return **-1** if it doesn't find anything.  
`const indexOfElementToFind = array.indexOf(element)`  
If specifying an index, it will start looking from that index  
`const indexOfElementToFind = array.indexOf(element, 3)`, will start to look from index 3

### lastIndexOf()

Will look for the last occurence of a certain element in an array.  
Will return the index of the element if it exists, otherwise
will return **-1** if it doesn't find anything.
`const elementToFind = array.lastIndexOf(element)`

### find()

Will return an element if it exists in an array
Can take as parameters every items in the array, the index of each element and the entire array.
`const returnElement = arr.find(item => item === elementToFind)`
`array.find((item, index, entireArray) => {})`

### findIndex()

Will return the index of a desired value
`const elementIndex = arr.findIndex(item => item === desiredElement)`

### includes()

Will return a boolean depending if the array has the desired element or not
`const hasElement = arr.includes(elementToLookFor)`

### forEach()

Will perform an action with all elements in an array. Will **not** return a new array.
`arr.forEach((item, index, entireArray) =>{ // code to perform with each item})`

### map()

Will return a new array.
`const newArray = arr.map((item, indx, entireArray) => { // code to perform on each element})`

### sort()

Will sort an array. It will convert all values into strings then sort them alphabetically.

```javascript
const sortedPrices = prices.sort((a, b) => {
  if (a > b) {
    return 1;
  } else if (a === b) {
    return 0;
  } else {
    return -1;
  }
});
```

### reverse()

Will reverse the element in the array
`arr.reverse()`

### filter()

Will return a new array with the values corresponding to the filtering method
`arr.filter(item, idx, entireArray => { // condition to return})`

### reduce()

Will return a new array. Will iterate on the array and do actions with all the elements combined. **It takes 2 parameters**: a **function** like all the other array methods and an **initial value**.
`numbers.reduce((prevValue, currentValue, idx, entireArray) => { // e.g: prevValue + currentValue}, initialValue)`

### split()

Will transform a string into an array of strings by specifying which character it should target to separate the data.
`const string = 'some;random;string;'`
`const splitString = string.split(';') // ['some','random','string']`

### join()

Will take values from an array and merge them into a string
`const arr = ['firstName','lastName']`
`arr.join() // 'firstName, lastName'`
It will separate the values by a **colon ','** if no other separator is specified

### spread operator

Will make a copy of an array(like the slice() method) or an object.  
`const array = [1,2,3]`  
`copiedArray = [...array]` => new array  
Good way to concatenate arrays:
`const array1 = [1,2,3]`
`const array2 = [4,5,6]`
`const newArray = [...array1,...array2]` // newArray = [1,2,3,4,5,6]

### Array destructuring

Will split up the values of an array into variables
`const arr = ['parker','peter', 15, 'NY City']`
`const [lastName, firstName, ...otherInfo] = arr // lastName, firstName, [15, 'NY City']`  
The const can have any name we want.

### Converting an object to an array

Will output an array with values depending on the method used:

```javascript
const objectName = { key1: 'value1', key2: 'value2' };
Object.key(objectName); // ['key1','key2']
Object.values(objectName); // ['value1','value2']
Object.entries(objectName); // [['key1','value1'], ['key2','value2']]
```

---

## Sets

---

Useful for storing unique values like 'ids'.
`cont ids = new Set([1,3,4,88])`
`ids.has('88')` // returns true
`ids.add(999)` // will add 999 to the end of the array
`ids.delete(4)` // will delete 4 from the array

---

## Maps

---

Can be used to add info to an object without changing the original declaration

---

## Objects

---

- Add
- Update
- Delete
- Spread Operator
- Object assign()
- Object destructuring
- Checking property existence
- this

```javascript
const objectName = {
  key: value, // key:value pair, also called 'property' or 'field'
};
```

```javascript
const objectName = {
  name : 'name', // string
  age: 32, // number
  isAdmin: true // boolean
  greet: () => {} // method
}
```

### Add

On new property:
Dot notation :`objectName.propertyToAdd = value`
Brackets notation:`objectName['propertyToAdd'] = value` // allows variables to be added to the object

### Update

On existing property:
Dot notation :`objectName.existingPropertyToUpdate = value`
Brackets notation:`objectName['existin gPropertyToUpdate'] = value`

### Delete

On existing property:
Dot notation :`objectName.existingPropertyToDelete = value`
Brackets notation:`objectName['existingPropertyToDelete'] = value`

### Spread Operator

Can be used to make a copy of a object.  
`const person1 = {name: 'value', age : 34}`
`const person2 = {...person1}`

### Object assign()

Will assign properties of an object to a new object.  
`const person1 = {key1: 'value', key2: 'value'}`  
`const person2 = Object.assign({}, person1)`

### Object destructuring

Will pull out a **key: value** pair and store it in a **variable**
`const person1 = {key1: 'value1', key2: 'value1' }`
`const {key1} = person1` // the name of the const must be equal to an existing property of the object
`console.log(key1)` // value1

Assign a new name to the key:
`const person1 = {key1: 'value', key2: 'value' }`
`const {key1 : new Name} = person1`

Assign the rest of the key:value pairs to a variable by using the **rest** parameter :
`const person1 = {key1: 'value', key2: 'value' }`
`const {key1 : new Name, ...otherKeys} = person1`

### Checking property existence

Checking if it exists:
`if(keyName in objectName){}`
`if(objectName.keyName === undefined){}`

Checking if it doesn't exist:
`if(!(keyName in objectName)){}`

### this

Will represent the object that calls the method in which the **this** keyword is present.  
Inside a constructor, **this** will refer to the object newly created.

```javascript
class Component {
  constructor() {
    this.render();
  }
  render() {}
}
```

### set

Will set a property

```javascript
const object = {
  key: value,
  set setKey(valueToSet) {
    this._setKey = valueToSet;
  },
};

object.setKey = value;
```

### get

Will get a property. Can be used as a readonly property if no setter is used.

```javascript
const object = {
  key: value,
  set setKey(valueToSet) {
    this._setKey = value;
  },
  get setKey() {
    return this._setKey;
  },
};

object.setKey; // 'value'
```

---

## Classes

---

- Overview
- Creating a class
- Constructor method
- Inheritance: Extending Classes
- super()
- static property
- setter
- getter
- private property
- getOwnPropertyDescriptor()
- getOwnPropertyDescriptors()
- defineProperty()

### Overview

Classes can act as **blueprints** of objects. They make it easier to create similar objects.
Calling a class is called **Instantiating** a Class : `const variable = new ClassName()`.  
They don't replace Objects, but Objects are built based on Classes

### Creating the Class:

```javascript
class ClassName {
  // has to be kebab case
  field1 = 'Default value'; // fields can have default values
  field2;
  field3;
}
```

A Class has **fields**. They will become **properties** of the new Object created.

### Constructor method

Used to build the object with the Class by passing parameters to define the fields:

```javascript
class ClassName {
  // the fields can be declared directly in the constructor() method
  field1 = 'Default value'; // optional
  field2; // optional
  field3; // optional

  constructor(param1, param2, param3) {
    this.field1 = param1;
    this.field2 = param2;
    this.field3 = param3;
  }
}

const newObject = new ClassName(arg1, arg2, arg3);
```

### Inheritance: Extending Classes

Extending Classes allows some Classes to **extend** another Class which becomes the **parent** Class. The child Class has then access to the parent Class properties and methods.

```javascript
class ParentClass {
  field1 = 'Default value';
  method1() {
    console.log(this.field1);
  }
}

class ChildClass extends ParentClass {
  render() {
    return this.method1();
  }
}
```

### super()

Extends the constructor of the **parent** class in the **child** class.

### static property

A static property allows the class to be called as such, without having to define it in a variable:

```javascript
class App {
  static init() {
    console.log('initializing...');
  }
}

App.init();
```

### setter

Will set a property and will allow to perform some checks on the value passed and also set a default value if necessary.

### getter

Will get a property from that same class.

### private property

A property can be made **private** (i.e: will not be accessible outside the class) by using the **hash/#** symbol.

### getOwnPropertyDescriptor()

Will list all the properties of a property object

### getOwnPropertyDescriptors()

Will list all the properties of all the properties of an object

### defineProperty()

Will set the properties of a property in an object

---

## Events

---

- events list
- event object
- events in HTML
- on() methods
- eventListeners
- removing eventListeners
- preventDefault()
- capturing
- bubbling
- propagation
- triggering event programmatically
- this

### events list

- click
- mouseenter
- mouseleave
- dblclick
- scroll
- submit // on forms only

### event object

Every event comes with some data attached.
Some of the properties are :

- target : the element the event happened on
- currentTarget: the element on which the listener is set
- clientX / clientY : the position of the event relative to the window in the case of a click
- offsetX / offsetY: the position relative to the element
- button: left button of the mouse = 0
- target.closest(element) : will target the closest element to the one the event is happening on

### events in HTML

In the HTML code:
`<button onClick(someFunction)></button>` // This is a **bad practice**, do not mix JS and HTML.

### on() methods

- onClick() method:
  Downside: can only call one function

### eventListeners

Set an eventListener to an element of the DOM:
`button.addEventListener('click', someFunction)`

### removing eventListeners

Create a removeEventListener() and pass in the exact same arguments: event type and function.
`button.removeEventListener('click', someFunction)`

### preventDefault()

Cancels the default behavior of an event by the browser/host
`button.addEventListener('submit', (event) => {event.preventDefault()})` // will not reload the page which is the default behavior.
Some other function can then be called

### capturing

Will go from outside to inside

### bubbling

Will go from inside to outside

### propagation

An event will first occur on the element it is called upon but will then propagate to its parent elements by default(third argument = false):
`button.addEventListener('submit', (event) => {event.preventDefault()}, false)` // bubbling
If set to **true** then the event on the parents will be called first
`button.addEventListener('submit', (event) => {event.preventDefault()}, true)` // capturing
`event.stopPropagation()` // will cancel the bubbling

### triggering event programmatically

Use the built in methods:

- element.click()
- element.scroll()

### this

On an arrow function, **this** will refer to the window element
On a function declaration, **this** will refer to the element that has the listener

### drag and drop

---

## setTimeout()

---

Will fire up a function after a desired time
`setTimeout(someFunction, timeInMilliseconds)`
Clearing the setTimeout method:  
The setTimeout returns an _id_ when called. That id can be used to target that setTimeout and cancel it:  
`const setTimeoutId = setTimeout(someFunction, time)`
`clearTimeout(setTimeoutId)`

---

## setInterval()

---

Will fire up a function at a predefined interval:
`setInterval(someFunction, timeInMillisecond)`
The setInterval returns an _id_ when called. That id can be used to target that setTimeout and cancel it:
`const setIntervalId = setInterval(someFunction, time)`
`clearInterval(setIntervalId)` // clearTimeout would also work...

---

## location

---

Allows the user to navigate  
`location.href = 'someUrl'`  
`location.host` // will return the url  
`location.origin` // will return the url with the protocol used  
`location.pathname` // will return the entire url

---

## history

---

Gives access to the history of the browser:  
`history.back()`  
`history.forward()`  
`history.length()`  
`history.go(number)`

---

## navigator

---

`navigator.geolocation.getCurrentPosition(data => functionToRunWithTheDataReturned)`

---

## date

---

The Date object will return the current date.

`new Date()` // outputs the current date  
`new Date().getSeconds()` // will return the current seconds
`new Date().getMinutes()` // will return the current minutes
`new Date().getHours()` // will return the current hours
`new Date().getMonth()` // will return the current month in number
`new Date().getDate()` // will return the day of the week in number
`new Date().getTime()` // will return the current time in milliseconds (i.e: 1625736763145)
`new Date().getFullYear()` // will return the current year

---

## error

---

`new Error('message')`
will also show where the error comes from

**object.constructor.methods/property**

---

## RegEx

---

Regular Expression. Can be used to check the validity of an input value.  
Checking for email syntax:  
`/^\S+@\S+\.\S+$/` // will check for any characters, the _@_ symbol, any character, the _dot_ symbol and any character after that.

---

## Event Loop

---

The Event Loop is not part of the Javascript Engine(e.g: V8, Spidermonkey), but is part of the host environment(browser...).
The Event Loop will run and check to see if the call stack is empty and will then take some code from the message queue and place it in the call stack.

---

## Promise

---

- Overview
- Resolving - then()
- Error handling - catch()
- Promise.race()
- Promise.All()
- Promise.allSettled()

### Overview

Promises are objects which "wrap" (asynchronous) code to make working with it easier.
Declaring a new promise:

### Resolving - then()

The **then** will be used to run some code if the promise is **resolved**.

```javascript
const somePromise = new Promise((resolve, reject) => {
  console.log('whatever...');
});

somePromise().then(someFunction());
```

### Error handling - catch()

The **catch** keyword will be used if the promise is **rejected**.

```javascript
somePromise()
  .then(data=>{
    console.log(data)
  })
  .catch(err){
    console.log(err)
}
```

### finally()

The **finally** keyword can be used to run any code once the promise has settled, wheither it was resolved or rejected.

```javascript
somePromise()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  })
  .finally(() => {
    // clean up code here
  });
```

### Promise.race()

Will return the result of whichever promise resolves first. The other ones won't be cancelled but their result will be ignored.

```javascript
Promise.race([promise1(), promise2()]).then((dataOfTheFirstResolvedPromise) => {
  console.log(dataOfTheFirstResolvedPromise);
});
```

### Promise.all()

Will return either an array of all the promises resolved(the order of the result will match the order of the promises) or a result of one of the promises that failed.
If none rejected:

```javascript
Promise.all([promise1(), promise2()]).then((dataOfAllThePromise) => {
  console.log(dataOfAllThePromise); // data will be of an array [resultOfPromise1, resultOfPromise2]
});
```

If one promise is rejected:

```javascript
Promise.all([promise1(), promise2()]).then((dataOfTheFailedPromise) => {
  console.log(dataOfTheFailedPromise); // will return the data of the failed promise
});
```

### Promise.allResolved()

Will return an array of objects [{status: '', value:''}] of all the promises results, resolved or rejected. A rejected promise will not cancel the other ones.

```javascript
Promise.allResolved([promise1(), promise2()]).then((dataOfAllThePromises) => {
  console.log(dataOfAllThePromises); // will return the data of the failed promise
});
```

---

## Async / await

---

Transforms the promise. It protects us from the **callback hell** where too many promises are nested into each other.
It also makes the code more readable and makes it look like synchronous code as the lines after an await labeled function won't be ran until the promise as returned a value.

Call **async** on the function that returns the promise, then **await** on the promise itself.

```javascript
async functionRetuningThePromise = () =>{
  await pr = promiseFunction()
}
```

### Handling errors

```javascript
async functionRetuningThePromise = () =>{
  try{
    await pr = promiseFunction()
  }catch(err){
    console.log(err)
  }
}
```

---

## JSON

---

- parse()
- stringify()
- xhr.responseType()

**Javascript Object Notation**
Double quotes only around the keys.

```JSON
{
    "name": "Max",
    "age": 30,
    "hobbies": [
        { "id": "h1", "title": "Sports" },
        { "id": "h2", "title": "Cooking" }
    ],
    "isInstructor": true
}
```

### parse()

Transforms a JSON string into an object
`JSON.parse(JSON Object)`

### stringify()

Transforms a vale into a JOSN string
`JSON.stringify({key: value})`

### xhr.responseType

Will transform the xhr response into the desired type:
`xhr.responseType = 'json'` // will transform the response if the type is JSON

---

## Http request

---

### XMLHttpRequest object

Creating a XMLHttpRequest object:
`const xhr = new XMLHttpRequest()`

### confituring the request

`xhr.open('Http method','endPoint','data')` // will accept some arguments to configure the request.  
Http methods:

- GET
- POST
- DELETE
- PATCH
- PUT

### Get

`xhr.send('GET','url')`

### Post

`xhr.send('POST','url','dataToSend')`

### fetch()

The **fetch API** is a built in API in the browser as opposed to AXIO which can be used through a CDN.

#### GET

```javascript
fetch('URL')
  .then(response => {
    return response.json(); // will map the response to a json object that will return another promise
  })
  .then(modifiedResponse => {
    console.log(modifiedResponse),
  })
  .catch(err => {
    console.log('fetch error:', err)
  })

```

#### POST

Sending JSON data:

```javascript
fetch('postURL', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json', // type of data we're sending
    Accept: 'application/json', // type of data we expect to get back
  },
  body: JSON.stringify({
    data: 'DATA TO SEND',
  }),
})
  .then((postResponse) => console.log(postResponse))
  .catch((postError) => console.log(postError));
```

---

## Modules

---

- HTML markup
- export
- default export
- import
- multi export
- rename imports
- dynamic imports

### HTML markup

Add `type="module"` to the script tag of the js file:  
`<script src="assets/scripts/app.js" defer type="module"></script>`

### export

Exporting a class, function, variable...  
`export nameOfTheObjectToExport`  
The **export** keyword makes the class/variable/function available to the rest of the application

### default export

`export default {}` // will not require a name and no curly braces in the import file
`import CustomName, {otherNamedImport} from '/path.js'` // curly braces are for the named exports

### import

`import { objectNameToImport } from '/path.js'`

### multi export

`import { objectNameToImport, secondObjectName } from '/path.js'`
**or**
`import * customName from '/path.js'`

### rename imports

`import { objectNameToImport as alias } from '/path.js'` // alias will be used in the file where the import is made

### dynamic imports

```javascript
import('./path.js')
  .then((module) => {
    // code to run...
  })
  .catch((err) => {
    // code to run if the module was not imported
  });
```

### globalThis

**globalThis** replaces the **this** when using imports.  
It is available in the browser and server side JS.  
It holds the Window object on the browser side.

---

## File Reader

---

Reads a file from the `input="file"` input.

```javascript
const reader = new FileReader();
reader.onload = () => {
  result = reader.result; // will be of type string
};
reader.readAsDataURL([fileToRead]);
```

**onload** and **loadend** are basically the same but loadend holds more informations
