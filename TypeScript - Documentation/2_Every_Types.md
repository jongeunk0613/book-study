# Everyday Types
<br/>

## The primitives: `string`, `number`, `boolean`

They have the same names when using the `typeof` operator on a value of those types.  
- `string` &rarr; string values like `"Hello, world"`
- `number` &rarr; numbers like `42`. All numbers, including integers and float - no `int` or `float` in JavaScript. 
- `boolean` &rarr; for the two values `true` and `false`

<p align="center">
<img style="display:inline" width="229" alt="image" src="https://user-images.githubusercontent.com/43084680/168470348-1d942113-26c8-4cd5-83c8-3110a4808522.png">
<img style="display:inline" width="193" alt="image" src="https://user-images.githubusercontent.com/43084680/168470358-dfcb9b89-9174-41b7-ae04-45a7e6af7c28.png">
<img style="display:inline" width="213" alt="image" src="https://user-images.githubusercontent.com/43084680/168470365-e0870fe7-dc8b-4f8b-982e-5945b14eaf01.png">
</p>


> The type names `String`, `Number`, `Boolean` (starting with capital letters) are legal, but refer to some special built-in types that will very rarely appear in your code. *Always use* `string`, `number`, or `boolean` for types. 
<br/>

### string vs String
In general, `string` denotes a primitive whereas `String` denotes an object.  
`Primitives` are values that `hold no properties`, whereas `objects` have the feature of `adding a property` to it.  
<br/>

## Arrays
Specify the type of an array using the syntax **type[]** &rarr; `number[]`, `string[]`   
It can also be written as **Array<type>** &rarr; `Array<number>`, `Array<string>`  

> Note that `number[]` and `[number]` are two different things &rarr; Arrays vs Typles
<br/>
  
## `any`
A special type in TypeScript used whenever `you don't want` a particular value `to cause typechecking errors`.  
When using type `any`, you can do much of `everything that's syntactically legal` (access any property, call it like a function, assign it to or from a value of any type, etc.  
It is useful when you don't want to write out a long type just to convince TypeScript that a particular line of code is okay.  
<img width="513" alt="image" src="https://user-images.githubusercontent.com/43084680/168473814-9e5aa4b9-13d2-48e6-95e0-3125e76a99da.png">
<br/>
  
## Type Annotations on Variables
### Explicit Annotation
Optionally add a type annotation to explicitly specify the type of a variable:
```TypeScript
  let myName: string = "Alice";
```
> TypeScript doesn't use "types on the left"-style declarations like `int x = 0;`  
> Type annotations will always go *after* the thing being typed.
<br/>
  
### Implicit Annotation
In most cases, type annotation is not needed; TypeScript tries to automatically *infer* the types.  
<img width="493" alt="image" src="https://user-images.githubusercontent.com/43084680/168476473-0ccef585-f37b-4a18-a662-856c1d58a4db.png">
<br/>
  
## Functions
TypeScript allows you to specify the `types of both the input and output values` of functions.  

### Parameter Type Annotations
Add type annotations `after each parameter` to declare what types of parameters the function accepts.  
With type annotations, arguments to that funtion will be checked.  
<img width="675" alt="image" src="https://user-images.githubusercontent.com/43084680/168610770-5e79fd9a-12bc-42be-8793-70805ab3017c.png">

> Even if you don't have type annotations on your parameters, TypeScript will `check` that you passed the `right number of arguments.`
<br/>
  
### Return Type Annotations
Add return type annotations `after the parameter list`.  
<img width="296" alt="image" src="https://user-images.githubusercontent.com/43084680/168611246-f3bc677f-d516-4ad9-b4d9-a80c81ed0cf1.png">

TypeScript `infers the function's return type` based on its `return` statements.  
Therefore, usually a return type annotation is not needed.  
Sometimes it is added for documentation purposes, to prevent accidental changes, or just for personal preference. 
<br/>
  
### Anonymous Functions
With anonymous functions, `contextual typing` occurs, which the `context` that the function occurred within informs what type it should have.  
<img width="832" alt="image" src="https://user-images.githubusercontent.com/43084680/168612933-ed1f929b-df1a-489d-8b6c-36803c8279d6.png">

Here, even though the parameter `s` didn't have a type annotation, TypeScript used the types of the `forEach` function, along with the inferred type of the array, to determine the type `s` will have.  
<br/>
  

