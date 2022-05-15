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
  

