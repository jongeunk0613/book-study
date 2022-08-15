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

In JavaScript, function values are objects:
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

<br/>
<hr/>

#### `object` vs `Object` vs `{ }`
##### `Object`  
Contains methods such as `toString()` and `hasOwnProperty()` that is present in all JavaScript Objects.  
Any value (primitive, non-primitive) can be assigned to `Object` type.  
```JS
function foo(bar: Object) {
    console.log(bar);
}

foo([1, 2, 3]);     // ok
foo({a: 1, b: 2})   // ok
foo(123);           // ok
```
<br/>

##### `{ }`
An empty object.  
It is the same as `Object` in runtime but different in compile time.  
In compile time `{ }` doesn't have `Object`'s members and `Object` has more strict behavior.  
<br/>

##### `object`  
It is any **non-primitive** type.  
You can't assign to it any primitive type like `bool, number, string, symbol`.  
```JS
function foo(bar: object) {
    console.log(bar);
}

foo([1, 2, 3]);     // ok
foo({a: 1, b: 2})   // ok
foo(123);           // Argument of type 'number' is not assignable to parameter of type 'object'
```

<hr/>
<br/>

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

### Rest Parameters
It is also possible to describe a function that takes an *unbounded* number of arguments using **rest parameters**.  
A **rest parameter** appears after all other parameters, and uses the `...` syntax.  

```JS
function multiply(n: number, ...m: number[]){
    return m.map((x) => n*x);
}
// [10, 20, 30, 40]
const a = multiply(10, 1, 2, 3, 4);
```
In TypeScript, the type annotation on these parameters is implicitly `any[]` instead of `any`,  
and any type annotation given must be of the form `Array[T]` or `T[]` or a tuple type.  
```JS
// Rest parameter 'm' implicitly has an 'any[]' type, but a better type may be inferred from usage.
function multiply(n: number, ...m){
    return m.map((x) => n*x);
}

// A rest parameter must be of an array type
function multiply2(n: number, ...m: number){
    return m.map((x) => n*x);
}
```
<br/>

### Rest Arguments
Conversely, a **variable number of arguments** can be provided from an array using the spread syntax.  
For example, the `push` method of arrays takes any number of arguments.  
```JS
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
arr1.push(...arr2);
console.log(arr1);  // [1, 2, 3, 4, 5, 6]
```

In general, TypeScript does not assume that arrays are immutable.  
Which leads to some surprising behavior:  
```JS
// Inferred type is number[] -- "an array with zero or more numbers",
// not specifically two numbers
const args = [8, 5];
// Math.atan2(y: number, x: number): number
const angle = Math.atan2(...args);
// A spread argument must either have a tuple type or be passed to a rest parameter. 
```
In general a `const` context is the most straightforward solution:  
```JS
// Inferred as 2-length tuple
const args = [8, 5] as const;
const angle = Math.atan2(...args);
```
Usign rest arugments may require turning on **downlevelIteration** when targeting older runtimes.  
<br/>

<hr/>

#### `downlevelIteration`
In TypeScript, Downleveling means transpiling to an older version of JavaScript.  
This flag is to enable support for a more accurate implementation of how modern JavaScript iterate through new concepts in older JavaScript runtimes.  
  
ECMAScript6 added several new iteration primitives:  
- `for / of` loop  
- Array spread `[a, ...b]`  
- argument spread `fn(...args)`  
- `Symbol.iterator`  

`downlevelIteration` allows for these iteration primitives to be used more accurately in ES5 environments  
if a `Symbol.iterator` implementation is present.  

[downlevelIteration](https://www.typescriptlang.org/tsconfig#downlevelIteration)
<hr/>
<br/>

### Parameter Destructuring
Used to conveniently unpack objects provided as an argument into one or local variables in the function body.  
In JavaScript:  
```JS
function sum({a, b, c}) {
    console.log(a + b + c)
}

sum({a: 10, b: 3, c: 9});
```
The type annotation for the object goes after the destructuring syntax:  
```JS
function sum({a, b, c}: {a:number; b: number; c:number}) {
    console.log(a + b + c)
}

console.log(sum({a: 10, b: 3, c: 9}));
```
Or use a named type:  
```JS
type ABC = {a:number; b: number; c:number};
function sum({a, b, c}: ABC) {
    console.log(a + b + c)
}

console.log(sum({a: 10, b: 3, c: 9}));
```
<br/>

### Assignability of Functions
#### Return type `void`
Contextual typing with a return type of `void` does **not** force functions to **not** return something.  
Another way to say this is a contextual function type with a `void` return type (`type vf = () => void`),  
when implemented, can return *any* other value, but it will be ignored.  
```JS
type voidFunc = () => void;

const f1: voidFunc = () => {
    return true;
};

const f2: voidFunc = () => true;

const f3: voidFunc = function () {
    return true;
}
```
When the return values of these functions are assigned to another variable,  
it will retain the type of `void`:  
```JS
// const v1: void
const v1 = f1();    
const v2 = f2();
const v3 = f3();

console.log(v1, v2, v3);
// true, true, true
```
This behavior exists so that the following code is valid even thought  
`Array.prototype.push` returns a number and the `Array.prototype.forEach` method  
expects a function with a return type of `void`.  
```JS
const src = [1, 2, 3];
const dst = [0];

src.forEach((el) => dst.push(el));
console.log(dst) // [0, 1, 2, 3]
```
Another special case to be aware of:  
When a literal function definition has a `void` return type, that function must **not** return anything.  
```JS
// Type 'boolean' is not assignable to type 'void'
function f2(): void {
    return true;
}

const f3 = function(): void {
    return true;
}
```.
