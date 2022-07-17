# Generic Functions

**Generics** are used to describe a correspondence between two values.  
- relating the type of the input to the type of the output
- relating the types of two inputs in some way

```JS
function firstElement<Type>(arr: Type[]): Type | undefined {
  // arr: Type[]
  return arr[0];
}
```

In this case, using the generic `Type` parameter, 
a link is created between the input of the function and the output.  
```JS
// s : string | undefined
const s = firstElement(["a", "b", "c"]);
// n : number | undefined
const n = firstElement([1, 2, 3]);
// u : undefined
const u = firstElement([]);
```
