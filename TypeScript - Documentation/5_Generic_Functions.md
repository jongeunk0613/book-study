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
// function firstElement<string>(arr: string[]): string | undefined
const s = firstElement(["a", "b", "c"]);
// n : number | undefined
// function firstElement<number>(arr: number[]): number | undefined
const n = firstElement([1, 2, 3]);
// u : undefined
// function firstElement<never>(arr: never[]): undefined
const u = firstElement([]);
```
<br/>

## Inference
The `Type` is inferred by TypeScript.  

```JS
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}

// (parameter) n : string
// const parsed : number[]
const parsed = map(["1", "2", "3"], (n) => parseInt(n));
```
TypeScript could infer that `Input` was `string` and `Output` was `number[]`.  
<br/>

## Constraints
Constraints are used to `limit the kinds of types` a type parameter can accept.  

`longest` accepts two value and returns the longest.  
For this, we need a `length` property of type `number`.  
```JS
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

// const longestArray : number[]
const longestArray = longest([1, 2], [1, 2, 3]);
// const longestString : "jongeun" | "yejin"
const longestString = longest("jongeun", "yejin");
// Argument of type 'number[]' is not assignable to parameter of type '"hello"'.
const longestValue = longest([1, 2, 3], "hello");
// Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
// numbers don't have length property
const notOk = longest(10, 100);
```

Because we constrained `Type` to `{length: number}`, we were allowed to access the property `.length`.  
Otherwise, we wouldn't be able because the values might have been some other type without a length property.  

The types were inferred based on the arguments.  
Generics is about relating two or more values with the same types.  
<br/>

## Working with Constrained Values
Some common error:  
```JS
function minimumLength<Type extends { length: number }> (
  obj: Type, 
  minimum: number,
): Type {
  if (obj.length >= minimum) {
    return obj;
  } else {
    return { length: minimum };
    // Type '{ length: number; }' is not assignable to type 'Type'.
    // '{ length: number; }' is assignable to the constraint of type 'Type',
    // but 'Type' could be instantiated with a different subtype of constraint '{ length: number; }'.
  }
}
```
