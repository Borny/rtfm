# Typescript

- About
- Install
- Compile
- Types
- Type Assignment
- Type Inference
- Object Types
- Array Types

## About

Typescript types are checked during compilation, Javascript types are during runtime.

## Install

Global install:

`npm i -g typescript`

## Compile

Compile a .ts file into a .js file with this command:

`tsc [nameOfFileToCompile]`

## Types

- string => all text values
- number => all numbers with no differenciations between integers or floats
- boolean => true or false only (no truthy or falsy values)
- object => any Javascript objects
- array => any Javascript arrays
- tuple (**Typescript only**)=> fixed-length array

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
const person: {name: string; age: number } = {
    name: 'Peter',
    age: 23
}
```

## Array Types

Array types are defined using the **[]** symbols:  

```typescript
const hobbies: string[] = ['Surfing', 'Coding'] // Typescript expects all values to be strings
const hobbies: string[] = ['Surfing', 78] // the compiler will throw an error as one of the value is not a string
```

## Tuple

A **Tuple** will defined a fixed-length array:  

```typescript
const hobbies: [string,number] = ['Spike', 30] // hobbies will accept only an array with two values, the first must be a string and the second a number
const hobbies: [string,number] = ['Spike'] // compile error as a value is missing
const hobbies: [string,number] = ['Spike', true] // compile error as the second value is not of type number
const hobbies: [string,number] = ['Spike', 20, 'Faye'] // compile error as the length of the array is 3 (expected 2) 
```
