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
    // Type '{ length: number; }' is not assignable to type 'Type'.
    // '{ length: number; }' is assignable to the constraint of type 'Type',
    // but 'Type' could be instantiated with a different subtype of constraint '{ length: number; }'.
    return { length: minimum };
  }
}
```

The function promises to return the *same* kind of object as was passed in,  
not just *some* object matching the constraint.  
```JS
// 'arr' gets value { length: 6}
const arr = minimumLength([1, 2, 3], 6);
// and crashes here because arrays have 
// a 'slice' method, but not the returned object!
console.log(arr.slice(0));
```
<br/>

## Specifying Type Arguments
TypeScript cannot infer the intended type arguments always; there are cases where it can't.  

EX: a function to combine two arrays  
```JS
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
```

This would cause an error when calling the function with mismatched arrays:  
```JS
const arr = combine([1, 2, 3], ["hello"]);
// Type 'string' is not assignable to type 'number'.
```

You can solve by manually specifying `Type`.
```JS
const arr = combine<string | number>([1, 2, 3], ["hello"]);
```
<br/>

## Guidelines for Writing Good Generic Functions
### Push Type Parameters Down
```JS
function firstElement1<Type>(arr: Type[]) {
  return arr[0];
}

function firstElement2<Type extends any[]>(arr: Type) {
  return arr[0];
}

// a: number (good)
const a = firstElement1([1, 2, 3]);
// b: any (bad)
const b = firstElement2([1, 2, 3]);
```
`firstElement1`'s inferred return type is `Type`.  
`firstElement2`'s inferred return type is `any`.  
This is because TypeScript has to resolve the `arr[0]` expression using the constraint type,  
rather tahn "waiting" to resolve the element during a call.  

> **RULE**: When possible, use the type parameter itself rather than constraining it. 
<br/>

### Use Fewer Type Parameters
```JS
function filter1<Type>(arr: Type[], func: (arg: Type) => boolean): Type[] {
  return arr.filter(func);
}

function filter2<Type, Func extends (arg: Type) => boolean> (
  arr: Type[],
  func: Func
): Type[] {
  return arr.filter(func);
}
```

`Func` doesn't relate two values.  
> **RULE**: Always use as few type parameters as possible
<br/>

### Type Parameters Should Appear Twice
```JS
function greet<Str extends string>(s: Str) {
  console.log("Hello, " + s);
}

greet("World");
```

Sometimes we forget that a function might not need to be generic.  
This could be simply be written as follow:  
```JS
function greet(s: string) {
  console.log("Hello, " + s);
}
```
