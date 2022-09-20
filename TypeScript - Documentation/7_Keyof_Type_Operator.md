# The `Keyof` Type Operator

It takes an object type and produces a string or numeric literal union of its keys.  

```js
type Point = { x: number; y: number };
type P = keyof Point;
// same as "x" | "y"
```

If the type has a string or number index signature,  
`keyof` will return those types instead.  

```js
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish;
// type A = number
     
type Mapish = { [k: string]: boolean };
type M = keyof Mapish;
// type M = string | number
```

In this example, `M` is `string | number` because JavaScript object keys are always  
coerced to a string, so `obj[0]` is always the same as `obj["0"]`.  

