# TypeScript

- Types
- Class
- Interface

## Types

- string
- number
- boolean
- Array<Type> **or** type[]
- any
- tuple: different types in an array => [string, number, ...] 
- void
- never
- unknown

## Class

Will create a blueprint for an object. We don't need to declare the properties outside the constructor method thanks to TypeScript:

```javascript
export class ClassName{
  constructor(public protertyName: type, public protertyName: type){}
}
```

## Interface

Will type an object:

```javascript
export interface InterfaceName {
  protertyName: type;
  protertyName: type;
  methodName(parameter: type): returnedType;
}
```
