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
Most of the time, more complex structures are dealt instead of simple types.  
This type can consist of unions of other multiple types.  
The common property with literal types that exists in every type in the union is called `discriminated union`, since it narrows out the members of the union.  

Take the example of the `Shape` interface, where `circle` types keep track of their radiuses and `square` types keep track of their side lengths.  
<img width="245" alt="image" src="https://user-images.githubusercontent.com/43084680/176204416-d027001d-6ddf-4db9-8878-f9aeabcd2397.png"><br/>
<br/>

By using literal types `"circle"` and `"square"`, we can avoid misspelling issues.  
<img width="892" alt="image" src="https://user-images.githubusercontent.com/43084680/176204862-3326fa24-1417-4ad0-994c-cdd98828b99a.png"><br/>
<br/>

Let's create a `getArea` function that calculates the area depending on the shape type.  
```JS
function getArea(shape: Shape) {
  return Math.PI * shape.radius ** 2;
Object is possibly 'undefined'.
}
```
<br/>

Under `strictNullChecks` this gives us an error because `radius` might not be defined.  
```JS
function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius ** 2;
Object is possibly 'undefined'.
  }
}
```
<br/>

Even though we have done type check it still gives us an error.  
This is a situation where we know more about our values than the type checker does.  

```JS
function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius! ** 2;
  }
}
```
<br/>
We could use the non-null assertion `!` but this is error-prone.  
Also, if `strictNullChecks` is off, then we can assess does properties because optional properties are assumed to always be present.  

We can do better by telling the checker more specifically which properties are needed in which types.  
```JS
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;
```
This way we tell the type checker that `radius` is required for type `circle` and `sideLength` for type `square`.  
<br/>

```
function getArea(shape: Shape) {
  return Math.PI * shape.radius ** 2;
Property 'radius' does not exist on type 'Shape'.
  Property 'radius' does not exist on type 'Square'.
}
```
Instead of simply telling us that `radius` is undefined, it now tells us that shape might be `Square` and `Square` does not have the property `radius`.  

```JS
function getArea(shape: Shape) {
  return Math.PI * shape.radius ** 2;
Property 'radius' does not exist on type 'Shape'.
  Property 'radius' does not exist on type 'Square'.
}
```

Now the `getArea` function does not have any errors occurring.  
Here, the `radius` property is called the *discriminant* property.  
Checking whether the `kind` property is `"circle"` got rid of every type in `Shape` that didn't have a `kind` property with the type `"circle"`.  
Now, no non-null assertion `!` is needed.  
```JS
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
                        
(parameter) shape: Circle
    case "square":
      return shape.sideLength ** 2;
              
(parameter) shape: Square
  }
}
```


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





