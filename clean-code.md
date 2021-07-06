# Clean Code

- Naming
- Comments
- Functions & Methods
- Control Structures & Errors

---

## Naming

---

- Variables, Constants and Properties
- Functions and Methods
- Classes

### Variables, Constants and Properties

Variables should be named after what they store:

- object, number or string => describe the value : `name | age | user | ...`
- boolean => is true or false : `isValid | isLogged | isLoading | ...`

### Functions and Methods

If the function performs an operation, the name should describe that operation (using a verb) : `getUser() | send() | validateEmail() | savePost() | ...`

If it computes a boolean, it should answer a true or false question: `isEmailValid() | isPaid() | ...`

### Classes

Should describe the Object that will be created by instantiating the Class.  
i.e: `User | Product | Post | Admin | ...`

## Comments

The main idea for comments is to **avoid them**.

- Bad comments
- Good comments

### Bad comments

- Redundant information:

```typescript
// Checks if the email is valid => useless comment
function isEmailValid(email: string): boolean {
  // code here
}
```

- Dividers / Block markers:

```typescript
////////////
// CLASSES
////////////
class User {
  // code here
}
```

- Misleading comments:

```typescript
// creates the user => wrong description
function updateUser(user: User): void {
  // code here
}
```

- Commented-out code:

```javascript
// function isUSerLogged(user){
//   return user.logged
// }
```

### Good comments

- Legal information:  
  `// Created by Tristan Deloris | March 2021`

- Explanations that can't be replaced by good naming:

```javascript
// accepts [text]@[text].[text], i.e: it requires an "@" and a "dot"
const emailRegex = /\S+@\S+\.\S+/;
```

- Warnings:

```javascript
// Only works on font-end javascript
localStorage.getItem('user');
```

- Todo Notes:

```javascript
const send = () => {
  // TO DO : create the send method
};
```

---

## Functions & Methods

---

- Parameters
- Levels of Abstraction
- Pure & Side Effects

### Parameters

- 0 parameters : Best solution
- 1 parameter : Good solution
- 2 parameters : Acceptable
- 3 parameters : Avoid when possible
- > 3 parameters : Avoid at all costs

Avoid **Output Parameters** : functions that will modified the value passed as a parameter:

```javascript
function createId(user) {
  user.id = 'u1';
}

const user = { name: 'Tony' };
createId(user); // The createId function will modifiy the user object in an unexpected way

// Calling the function 'addId' makes more sense but still should be avoided
```

### Levels of Abstraction

Functions should be small and do one thing. Outsource code that do other things into different functions.

```javascript
// AVOID :
function createUser(email, password) {
  if (!email || !email.includes('@') || !password || !pasword.trim() === '') {
    console.log('Invalid input');
    return;
  }

  const user = { email, password };

  db.insert(user);
}

// DO :
function createUser(email, password) {
  if (!inputIsValid(email, password)) {
    console.log('Invalid input');
    return;
  }
  saveUser(email, password);
}

function validateInput(email, password) {
  return !email || !email.includes('@') || !password || !pasword.trim() === '';
}

function saveUser(email, password) {
  const user = { email, password };

  db.insert(user);
}
```

### Pure & Side Effects

A **pure** function will always output the same value given the same output. It will also not have any side effects.

```javascript
// PURE, returns the same output
function createId(userName) {
  const id = `id_${userName}`;
  return id;
}

// UNPURE, doesn't return the same output
function createId(userName) {
  const id = `id_${userName}${Math.random()}`;
  return id;
}

// UNPURE, has a side effect
function createId(userName) {
  const id = `id_${userName}${Math.random()}`;
  saveUser(userName, id); // does something else than working on the input
  return id;
}
```

The **naming matters** as it should **imply** a side effect

---
## Control Structures & Errors
---

- General rules
- Guards and fast fail

### General rules
- Avoid deep nesting
- Prefer positive checks
- Using factory functions and polymorphism
- Utilize errors

### Guards and fast fail

Use a **guard** and fail fast to save unnecessary nesting.  
A **guard** will invert an if check and simply return if the condition is met:

```javascript 
// DON'T
function logUser(email, password){
  if(email.includes('@')){
    // do stuff
  }
}

// DO
function logUser(email, password){
  if(!email.includes('@')){ // this will fail fast if the condition isn't met
    return;
  }

  // do stuff
}
```

