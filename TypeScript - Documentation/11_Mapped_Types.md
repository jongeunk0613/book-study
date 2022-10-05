# Mapped Types

A mapped type is a generic type which uses a union of **PropertyKeys** (frequently created via a `keyof`)  
to iterate through keys to create a type.  

```javascript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};
```

They are build on the syntax for **index signatures**,  
which are used to declare the types of properties which have not been declared ahead of time.  

```javascript
type OnlyBoolsAndHorses = {
  [key: string]: boolean | Horse;
};
 
const conforms: OnlyBoolsAndHorses = {
  del: true,
  rodney: false,
};
```

ex: `OptionsFlags` will take all the properties from the type `Type` and change their values to be a `boolean`.  

```javascript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type FeatureFlags = {
  darkMode: () => void;
  newUserProfile: () => void;
};
 
type FeatureOptions = OptionsFlags<FeatureFlags>;      
// type FeatureOptions = {
//     darkMode: boolean;
//     newUserProfile: boolean;
// }
```

<br/>

## Mapping Modifiers

There are two modifiers which can be applied during mapping: `readonly` for mutability and `?` for optionality.  
These modifiers can be added or removed by prefixing with `-` or `+`. If a prefix is not added, then `+` is assumed.  

```javascript
// Removes 'readonly' attributes from a type's properties
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property];
};
 
type LockedAccount = {
  readonly id: string;
  readonly name: string;
};
 
type UnlockedAccount = CreateMutable<LockedAccount>;        
// type UnlockedAccount = {
//     id: string;
//     name: string;
// }
```

```javascript
// Removes 'optional' attributes from a type's properties
type Concrete<Type> = {
  [Property in keyof Type]-?: Type[Property];
};
 
type MaybeUser = {
  id: string;
  name?: string;
  age?: number;
};
 
type User = Concrete<MaybeUser>;   
// type User = {
//     id: string;
//     name: string;
//     age: number;
// }
```

<br/>

## Key Remapping via `as`

In TypeScript 4.1 and onwards, you can re-map keys in mapped types with an `as` clause in a mapped type.  

```javascript
type MappedTypeWithNewProperties<Type> = {
    [Properties in keyof Type as NewKeyType]: Type[Properties]
}
```

With template literal types, new property names from prior ones can be created.  

```javascript
type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};
 
interface Person {
    name: string;
    age: number;
    location: string;
}
 
type LazyPerson = Getters<Person>;
// type LazyPerson = {
//     getName: () => string;
//     getAge: () => number;
//     getLocation: () => string;
// }
```

Also, keys can be filtered out by producing `never` via a conditional type.  

```javascript
// Remove the 'kind' property
type RemoveKindField<Type> = {
    [Property in keyof Type as Exclude<Property, "kind">]: Type[Property]
};
 
interface Circle {
    kind: "circle";
    radius: number;
}
 
type KindlessCircle = RemoveKindField<Circle>;        
// type KindlessCircle = {
//     radius: number;
// }
```

Arbitrary unions can also be mapped; not just unions of `string | number | symbol`, but unions of any type.  

```javascript
type EventConfig<Events extends { kind: string }> = {
    [E in Events as E["kind"]]: (event: E) => void;
}
 
type SquareEvent = { kind: "square", x: number, y: number };
type CircleEvent = { kind: "circle", radius: number };
 
type Config = EventConfig<SquareEvent | CircleEvent>    
// type Config = {
//     square: (event: SquareEvent) => void;
//     circle: (event: CircleEvent) => void;
// }
```

<br/>

## Further Exploration











