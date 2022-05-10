# The Basics
The only way in pure JavaScript to tell what some function does with a particular value is to `call it and see what happens` &rarr; **dynamic typing**.  
Using **static type system**, we can `make predictions` about what code is expected `before it runs`.  
<br/>

## Static type-checking
**Static type systems** help us `find bugs before running the code` by `describing the shapes and behaviors of` what our `values` will be when the code is run.  
```TypeScript
const message = "hello!";

message();
/* This expression is not callable.
  Type 'String' has no call signatures. */
```
<br/>

## Non-exception Failures
TypeScript catches potential bugs, even those that are "valid" Javascript that don't immediately throw an error.  
Bugs such as typos, uncalled functions, or basic logic errors.  

### accessing a property that doesn't exist
```JavaScript
const user = {
  name: "Daniel",
  age: 26,
};

user.location;
// JavaScript -> returns undefined
// TypeScript -> Property 'location' does not exist on type '{ name: string; age: number; }'.

```

### uncalled functions
``` JavaScript
function flipCoin() {
  // Meant to be Math.random()
  return Math.random < 0.5;
  // Operator '<' cannot be applied to types '() => number' and 'number'.
}
```

### basic logic errors
```JavaScript
const value = Math.random() < 0.5 ? "a" : "b";
if (value !== "a") {
  // ...
} else if (value === "b") {
  // This condition will always return 'false' since the types '"a"' and '"b"' have no overlap.
  // Oops, unreachable
}
```
<br/>

## Types for Tooling
TypeScript can also prevent us from making mistakes by suggesitng which properties we might want to use.  
An editor that supports TypeScript can deliver "quick fixes" to automatically fix errors, refactoring to easily re-organize code, and useful navigation features for jumping to definitions of a variable, or finding all references to a given variable. 
![image](https://user-images.githubusercontent.com/43084680/167619861-7354d2df-519d-4052-a08b-dc963a7fc90c.png)
<br/>

## tsc, the TypeScript compiler
Install TypeScript with the following command:
```console
>> npm install -g typescript
```

Create a simple ts file
```JavaScript
// hello.ts
console.log("Hello World!");
```

Type-check the file:
```console
>> tsc hello.ts
// hello.ts   hello.js
```

`tsc` compiles or transforms the TypeScript file into a plain JavaScript file.  
If there is an error, an error will show on the command line.  
```JavaScript
// hello.ts
function greet(person,date) {
  console.log(`Hello ${person}, today is ${date}!`);
}

greet("Brendan");
```
```console
>> tsc hello.ts
Expected 2 arguments, but got 1.
```
<br/>

## Emitting with Errors




### References
- [typescriptlang - basic types](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
