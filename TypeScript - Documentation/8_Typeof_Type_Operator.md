# `Typeof` Type Operator

JavaScript already has a `typeof` operator that you can use in an **expression context**.  

```js
console.log(typeof "Hello World!");
// "string"
```

In TypeScript, the `typeof` operator can be used in a **type context** to refer to the type of a variable or property.  

```js
let s = "hello";
let n: typeof s;
// let n: string
```

The `typeof` can be used to conveniently express many patterns.  
EX: There's a predefined type ReturnType<T>, which takes a function type and produces its return type.

```js
type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>;
// type K = boolean
```

If we try to use ReturnType on a function name, an instructive error occurs.  

```js
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<f>;
// 'f' refers to a value, but is being used as a type here. Did you mean 'typeof f'?
```

This is because values and types aren't the same thing.  
To refer to the `f`'s type, `typeof` is used.  

```js
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;
    
// type P = {
//     x: number;
//     y: number;
// }
``
  
<br/>
  
## Limitations

