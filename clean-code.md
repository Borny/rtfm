# Clean Code

- Naming
- Comments
- Functions & Methods

## Naming

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
function isEmailValid(email: string): boolean{
    // code here
}
```

- Dividers / Block markers:  
```typescript
////////////
// CLASSES
////////////
class User{
    // code here
}
```

- Misleading comments:  
```typescript
// creates the user => wrong description
function updateUser(user: User): void{
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
const emailRegex = /\S+@\S+\.\S+/ 
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
}
```

## Functions & Methods

- Parameters

### Parameters

- 0 parameters : Best solution
- 1 parameter : Good solution
- 2 parameters : Acceptable
- 3 parameters : Avoid when possible
- >3 parameters : Avoid at all costs

Avoid **Output Parameters** : functions that will modified the value passed as a parameter:

```javascript
function createId(user){
    user.id = 'u1';
}

const user = { name: 'Tony'};
createId(user); // The createId function will modifiy the user object in an unexpected way

// Calling the function 'addId' makes more sense but still should be avoided
```