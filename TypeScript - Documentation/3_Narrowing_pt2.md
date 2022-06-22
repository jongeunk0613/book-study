# Narrowing

## Control Flow Analysis
The analysis of code based on reachability is called **control flow analysis**.  
<img width="446" alt="image" src="https://user-images.githubusercontent.com/43084680/174823195-cfaaa2e1-7c6e-4cf7-a024-0ba917acd433.png"><br/>
The type `number` is removed from `padding` after the `if` block,  
because TypeScript knows that it is unreachable in the case where `padding` is a `number`.  
<br/>

TypeScript uses this flow analysis **to narrow types** as it encoutners type guards and assignments.  
<img width="416" alt="image" src="https://user-images.githubusercontent.com/43084680/174824867-249ef3bb-7f4a-4051-ad89-d2a393e957de.png"><br/>
<br/>

## Using Type predicates
By using a function whose return type is a **type predicate**, we can define a user-defined type guard.  
<img width="371" alt="image" src="https://user-images.githubusercontent.com/43084680/175066237-303de3d1-ded5-49c2-b592-37bd3e5792e7.png"><br/>
A **type predicate** takes the form `parameterName is Type`, where `paraterName` must be the name of a parameter from the current function signature.  
<br/>

Using the `isFish`, TypeScript can *narrow* a variable to a that specific type if the original type is compatible.  
<img width="388" alt="image" src="https://user-images.githubusercontent.com/43084680/175068275-92a21e02-08bf-48f2-b275-a75edfac221e.png"><br/>
TypeScript not only knows that `pet` is a `Fish` in the `if` branch.  
It also knows that in the `else` branch, you must have a `Bird`.  
<br/>

`isFish` can also used to filter an array of `Fish | Bird` and obtain an array of `Fish`.  
<img width="485" alt="image" src="https://user-images.githubusercontent.com/43084680/175070125-5d313a60-f127-4e1d-835d-cde701aa26d8.png"><br/>
<br/>

## Discriminated Unions

## The `never` type
TypeScript uses a `never` type to represent a state which shouldn't exist.  
<img width="405" alt="image" src="https://user-images.githubusercontent.com/43084680/175071372-e7e0d526-45a6-48d0-899c-4a17f547c7fc.png"><br/>
<br/>

## Exhaustiveness Checking
The `never` type is **assinable to every type**. But, **no type is assignable** to `never`.  
You can use narrowing and rely on `never` turning up to do exhaustive checking in a switch statement.  
<br/>

<img width="422" alt="image" src="https://user-images.githubusercontent.com/43084680/175073048-aa82152a-0337-489f-ac61-28a04f956b93.png"><br/>
The `default` in the `getArea` function tries to assign the shape to `never`.  
This will raise only when every possible case has not been handled.  
<br/>

Adding a new member to the `Shape` union, will cause a TypeScript error.  
<img width="502" alt="image" src="https://user-images.githubusercontent.com/43084680/175073527-07c0487a-e3a7-4d04-9e80-50af342227d9.png"><br/>





