# Indexed Access Types

Used to look up a specific property on another type.  

```js
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"];
// type Age = number
```

The indexing type is itself a type, so unions, keyof, or other types can be used entirely.  

```js
type Person = { age: number; name: string; alive: boolean };

type I1 = Person["age" | "name"];
// type I1 = string | number
 
type I2 = Person[keyof Person];
// type I2 = string | number | boolean
 
type AliveOrName = "alive" | "name";
type I3 = Person[AliveOrName];
// type I3 = string | boolean
```

If tried to index with a property that doesn't exit, an error will occur.  

```js
type Person = { age: number; name: string; alive: boolean };

type I1 = Person["alve"];
// Property 'alve' does not exist on type 'Person'.
```

Another example of indexing with an arbitrary type:  
Use `number` to get the type of an array’s elements.  
Combine this with `typeof` to conveniently capture the element type of an array literal.  

```js
const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
  { name: "Eve", age: 38 },
];
 
type Person = typeof MyArray[number];    
// type Person = {
//     name: string;
//     age: number;
// }

type Age = typeof MyArray[number]["age"];
// type Age = number

// Or
type Age2 = Person["age"];
// type Age2 = number
```

When indexing, only types can be used; `const` can't be used to make a variable reference.  

```js
type Person = { age: number; name: string; alive: boolean };

const key = "age";
type Age = Person[key];
// Type 'key' cannot be used as an index type.
// 'key' refers to a value, but is being used as a type here. Did you mean 'typeof key'?
```

However, type alias can be used for a similar style of refactor.  

```js
type Person = { age: number; name: string; alive: boolean };

type key = "age";
type Age = Person[key];
```


