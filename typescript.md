# TypeScript

- Types
- Class
- Interface

## Types

- propertyName : string => string
- propertyName : boolean => boolean
- propertyName : InterfaceName => interface
- propertyName : InterfaceName[] => array of interface
- void
- never

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
}
```
