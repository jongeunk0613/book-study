# Other Types to Know About

### `void`
Used when a function **does not return a value**.  
It's the inferred type when a function doesn't have any **`return` statements**,  
or doesn't return any **explicit value** from those return statements.  

```JS
function noop() {
  return; // void
}
```
<br/>

> `void` is not the same as `undefined`.
<br/>

### `object`
Refers to any value that **isn't a primitive**.  
It's different from the empty object type `{ }` and from the global type `Object`.  
It's very likely that `Object` will not be used.  
> `object` is not `Object`. **Always** use `object`!
<br/>

In JavaScript function values are objects:
- they have properties
- have `Object.prototype` in their prototype chain
- are `instanceOf Object`
- can call `Object.keys` on them

```JS
console.log(add.length) // 2
console.log(add.prototype)  // 
console.log(add instanceof Object)  // true
console.log(Object.keys(add))   // []
```

#### `object` vs `Object` vs `{ }`
asdasdasd

### `unknown`
Represents *any* value.  
It's similar to `any`, but safer because it's not legal to do anything with an `unknown` value.  

```JS
function f1(a: any) {
    a.b(); // OK
}

function f2(a: unknown) {
    a.b();
    // Object is of type 'unknown'
}
```

Useful when describing functions that **accept any value** without using `any` in the function body,  
or functions that **returns a value of unknown type**.  
```JS
function safeParse(s: string): unknown {
    return JSON.parse(s);
}

console.log(safeParse('{}'));       // { }
console.log(safeParse('true'));     // true
console.log(safeParse('"foo"'));    // "foo"
console.log(safeParse('[1, 5, "false"]'))   // [1, 5, "false"]
console.log(safeParse('null'));     // null
```

<br/>

### `never`
Represents values which are *never* observed.  
In a return type, it means that the function **throws an exception** or **terminates execution** of the program.  
```JS
function fail(msg: string): never {
    throw new Error(msg);
}
```
Also appears when TypeScript determines **there's nothing left in a union**.  
```JS
function fn(x: string | number) {
    if (typeof x === "string") {
        // do something
    } else if (typeof x === "number") {
        // do something else
    } else {
        x; // type 'never'!
    }
}
```
<br/>

### `Function`
Describes properties like `bind`, `call`, `apply`, and others present on all function values in JavaScript.  
The properties of type `Function` can be called and returns `any`.  
This is an **untyped function call**, and it is best avoided because of the unsafe `any` return type. 
If needed to accept an arbitrary function but don't intend to call it,  
the type `() => void` is generally safer.  

```JS
// function doSomething(f: Function) : any
function doSomething(f: Function) {
    return f(1, 2, 3);
}
```
<br/>

### `Rest Parameters and Arguments`




