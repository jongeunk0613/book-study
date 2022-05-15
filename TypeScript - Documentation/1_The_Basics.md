# The Basics
The only way in pure JavaScript to tell what some function does with a particular value is to `call it and see what happens` &rarr; **dynamic typing**.  
Using **static type system**, we can `make predictions` about what code is expected `before it runs`.  
<br/>

## Static type-checking
**Static type systems** help us `find bugs before running the code` by `describing the shapes and behaviors of` what our `values` will be when the code is run.  
<img width="415" alt="image" src="https://user-images.githubusercontent.com/43084680/168466095-5a91f581-4fec-4171-9af5-3aeddf48f4b1.png">
<br/>

## Non-exception Failures
TypeScript catches potential bugs, even those that are "valid" Javascript that don't immediately throw an error.  
Bugs such as typos, uncalled functions, or basic logic errors.  

### accessing a property that doesn't exist
<img width="692" alt="image" src="https://user-images.githubusercontent.com/43084680/168466181-582f17ff-464d-4d5c-952c-04ac6e38f62c.png">

### uncalled functions
<img width="672" alt="image" src="https://user-images.githubusercontent.com/43084680/168466249-c9d82896-3686-4b10-b3e5-d0279706d356.png">

### basic logic errors
<img width="832" alt="image" src="https://user-images.githubusercontent.com/43084680/168466298-b5cd31fe-268c-4323-a313-c5096e0e6d18.png">
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
Even when `tsc` reports an error about the code, a js file will be outputed.  
This is due to one of TypeScript's core values : much of the time, _you_ will know bettern than TypeScript.  
By using the `noEmitOnError` compiler option, you can make TypeScript more strict and defensive against mistakes.  
This way, if an error occurs, there is no file outputed.  
```console
tsc --noEmitOnError hello.ts
```
<br/>

## Explicit Types
Type annotations can be added to describe what types of values a variable or functions can be used with.  
```TypeScript
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```
It is not mandatory to write explicit type annotations. In many cases, TypeScript can even just _infer_ the types if they are ommitted.
![image](https://user-images.githubusercontent.com/43084680/167634976-8c3dd6b1-451c-4c59-b08c-d80634243388.png)
<br/>

## Erased Types
Type annotations aren't part of JavaScript, therefore TypeScript cannot be run right away.  
Therefore, when TypeScript is compiled to JavaScript, any TypeScript-specific code, including type annotation, is stripped out.  

Check out the `greet` function after it is compiled:
```JavaScript
"use strict";
function greet(person, date) {
  console.log("Hello ".concat(person, ", today is ").concat(date.toDateString(), "!"));
}
greet("Maddison", new Date());
```
<br/>

## Downleveling
TypeScript has the ability to rewrite code from newer versions of ECMAScript to older ones. This process is called _downleveling_.  
By default, TypeScript targets ES3. The target version can be changed : `tsc --target es2015 hello.ts`.  
  
Template strings are a feature from ES6. Therefore, we can see that from the previous code, the template strings are converted to plain string concatenation.  
<br/>

## Strictness
By default, types are optional, inference takes the most lenient types, and there's no checking for potentially `null`/`undefined` values.  
However, several type-checking strictness flags can be set. The two biggest are `noImplicitAny` and `strictNullChecks`.  
<br/>

## `noImplicitAny`
Issues an error on any variable whose type is implicitly inferred as `any`.  
<img width="500" alt="image" src="https://user-images.githubusercontent.com/43084680/168467873-1ab69a1e-8c99-49ad-9593-5f9038b12174.png">
<br/>

## `strictNullChecks`
Makes handling `null` and `undefined` more explicit, and spares us from worrying about whether we forgot to handle `null` and `undefined`.  
<br/>
<img width="505" alt="image" src="https://user-images.githubusercontent.com/43084680/168466866-526a122f-6d32-4bfb-a7f2-81d969f24a11.png">
Here, the user filtered by find may be null or undefined. However, this is not considered in the code.  

By setting `strictNullChecks` to `true`, `null` and `undefined` have their own distinct types and you'll get a type error if you try to use them where a concrete value is expected.  
This is the code after setting the `StrickNullChecks` to `true`.  
<img width="512" alt="image" src="https://user-images.githubusercontent.com/43084680/168466956-af7a1ef9-b748-4f11-adb3-e4ec7acdd655.png">

Check the definition of `find` function. The following is a simplification of it.
```TypeScript
// When strictNullChecks: true
type Array = {
  find(predicate: (value: any, index: number) => boolean): S | undefined;
};

// When strictNullChecks: false the undefined is removed from the type system
// allowing you to write code which assumes it always found a result
type Array = {
  find(predicate: (value: any, index: number) => boolean): S;
};
```

### References
- [typescriptlang - basic types](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
- [typescriptlang - noImplicitAny](https://www.typescriptlang.org/tsconfig#noImplicitAny)
- [typescriptlang - strictNullChecks](https://www.typescriptlang.org/tsconfig#strictNullChecks)
