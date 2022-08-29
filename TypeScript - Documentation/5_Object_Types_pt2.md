# Generic Object Types

Use *type parameter* to create generic types.  
```JS
interface Box<Type> {
    contents: Type;
}

```
&rarr; A `Box` of `Type` is something whose `contents` have type `Type`.  

`Box` is like a template for a real type ; `Type` is a placeholder that will get replaced with some other type.  

```JS

let box: Box<string> = {contents: "hello"};
box.contents;   // (property) Box<string>.contents: string

```

`Box` is reusable.  `Type` can be substituted with anything and for a new type, a new type of `Box` is not needed.  

```JS
interface Box<Type> {
    contents: Type;
}

interface Apple {
    count: number
}

// Same as '{contents: Apple}'
type AppleBox = Box<Apple>;
```

We can avoid overloads entirely by using generic functions.  
```JS
interface NumberBox {
  contents: number;
}
 
interface StringBox {
  contents: string;
}
 
interface BooleanBox {
  contents: boolean;
}

function setContents2(box: StringBox, newContents: string): void;
function setContents2(box: NumberBox, newContents: number): void;
function setContents2(box: BooleanBox, newContents: boolean): void;
function setContents2(box: { contents: any }, newContents: any) {
  box.contents = newContents;
}

/* - - - - - - - - */

interface Box<Type> {
    contents: Type;
}

function setContents<Type> (box: Box<Type>, newContents: Type) {
    box.contents = newContents;
}
``` 

Type aliases can also be generic.  
```JS
type Box<Type> = {
  contents: Type;
}
```

Since type aliases can describe more than just object types,  
it can be used to write other kinds of generic helper types.  

```JS
type OrNull<Type> = Type | null;

type OneOrMany<Type> = Type | Type[];

type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;
// type OneOrManyOrNull<Type> = OneOrMany<Type> | null

type OneOrManyOrNullStrings = OneOrManyOrNull<string>;
// type OneOrManyOrNullStrings = OneOrMany<string> | null
```

<br/>

# The `Array` Type

`Array` is a common generic type we use : `number[]` or `string[]` is a shorthand for `Array<number>` and `Array<string>`.  

```JS
function doSomething(value: Array<string>) {
    console.log(value);
}

let myArray: string[] = ["hello", "world"];

doSomething(myArray);
doSomething(new Array("hello", "world"));
```

```JS
interface Array<Type> {
  /**
   * Gets or sets the length of the array.
   */
  length: number;
 
  /**
   * Removes the last element from an array and returns it.
   */
  pop(): Type | undefined;
 
  /**
   * Appends new elements to an array, and returns the new length of the array.
   */
  push(...items: Type[]): number;
 
  // ...
}
```

<br/>

# The `ReadonlyArray` Type

The `ReadonlyArray` is a special type that describes arrays that **shouldn't be changed**.  

```JS
function doStuff(values: ReadonlyArray<string>) {
    const copy = values.slice();
    console.log(`The first value is ${values[0]}`);

    values.push("hello");
    // Property 'push' does ot exist on type 'readonly string[]'
}
```

It's like the `readonly` modifier for properties &rarr; a tool used for intent.  
Function returns `ReadonlyArray` &rarr; not meant to change the contents at all  
Function consumes `ReadonlyArray` &rarr; any array passed into the function will not be changed  

Unlike `Array`, there isn't a `ReadonlyArray` constructor.  

```JS
new ReadonlyArray("red", "green", "blue");
// 'ReadonlyArray' only refers to a type, but is being used as a value here
```

Instead, assign a regular `Array` to a `ReadonlyArray`.  
```JS
const roArray: ReadonlyArray<string> = ["red", "green", "blue"]
```

TypeScript also provides a shorthand syntax for `ReadonlyArray<Type` &rarr; `readonly Type[]`.  
```JS
function doStuff(values: readonly string[]) {
    const copy = values.slice();
    console.log(`The first value is ${values[0]}`);

    values.push("hello!");
    // Property 'push' does not exist on type 'readonly string[]`
}
```

Unlike `readonly` property modifier, assignability isn't bidirectional between regular `Array`s and `ReadonlyArray`s.  
```JS
let x: readonly string[] = [];
let y: string[] = [];

x = y;
y = x;
// The type 'readonly string[]' is 'readonly' and cannot be assigned to the mutable type 'string[]'
```

<br/>

# Tuple Types

Another sort of `Array` type that know exactly **how many elements** it contains,  
and exactly **which types** it contains **at specific positions**.  

```JS
type StringNumberPair = [string, number];
```

Like `ReadonlyArray`, it has no representation at runtime,  but is significant to TypeScript.  
To TypeScript, `StringNumberPair` describes array whose `0` index contains a `string` and whose `1` index contains a `number`.  

```JS
function doSometing(pair: [string, number]){
    const a = pair[0];
    // const a: string
    const b = pair[1];
    // const b: number
}

doSometing(["hello", 42]);
```

If we try to index past the number of elements, we'll get an error.  
```JS
function doSometing(pair: [string, number]){
    const a = pair[0];
    // const a: string
    const b = pair[1];
    // const b: number

    const c = pair[2];
    // Tuple type '[string, number'] of length '2' has no element at index '2'.
}

doSometing(["hello", 42]);
```

Destructuring tuples is also possible using JavaScript's array destructuring.  
```JS
function doSometing(stringHash: [string, number]){
    const [inputString, hash] = stringHash;
    console.log(inputString);
    // const inputString: string
    console.log(hash);
    // const hash: number
}
```

Simple tuple types are equivalent to types which are versions of `Array`s that  
**declare properties for specific indexes**, and that **declare `length` with a numeric literal type**.  

```JS
interface StringNumberPair {
    length: 2;
    0: string;
    1: number;

    // Other 'Array<string | number'> members...
    slice(start?: number, end?: number): Array<string | number>;
}
```

Tuples can have optional properties by writing out a question mark.  
Optional tuple elements can only come at the end, and also affect the type of `length`.  

```JS
type Either2dOr3d = [number, number, number?];

function setCoordinate(coord: Either2dOr3d) {
    const [x, y, z] = coord;
    // const z: number | undefined
    console.log(`Provided coordinates had ${coord.length} dimensions`);
    // (property) length: 2 | 3
}
```

Tuples can also have rest elements, which have to be an array/tuple type.  
A tuple with a rest element has no set "length" - it only has a set of well-known elements in different positions.  

```JS
type StringNumberBooleans = [string, number, ...boolean[]];
type StringBooleansNumber = [string, ...boolean[], number];
type BooleansStringNumber = [...boolean[], string, number];

function getLength(snbs: StringBooleansNumber) {
    console.log(snbs.length);
    // (property) length: number
}

const a: StringNumberBooleans = ["hello", 1];
const b: StringNumberBooleans = ["beautiful", 2, true];
const c: StringNumberBooleans = ["world", 3, true, false, true, false, true];
```

It is useful when using with parameter lists, like rest parameters and arguments.  

```JS
function readButtonInput(...args: [string, number, ...boolean[]]) {
    const [name, version, ...input] = args;
    // ...
}

// The top is basically equivalent to :
function readButtonInput(name: string, version: number, ...input: boolean[]) {
    // ...
}
```

<br/>

# `readonly` Tuple Types

Just like with array shorthand syntax, declare a `readonly` tuple by writing the `readonly` modifier in front.  

```JS
function doSomething(pair: readonly [string, number]) {
    pair[0] = "hello!";
    // Cannot assign to '0' because it is a read-only property
}
```

Tuples tend to be created and left un-modified in most codes, so annotating types as `readonly` tuple when possible is a good default.  
&rarr; array literals with `const` assertions are inferred as `readonly` tuple types.  

```JS
let point = [3, 4] as const;
// let point: readonly [3, 4]

function distanceFromOrigin([x, y]: [number, number]) {
    return Math.sqrt(x ** 2 + y ** 2);
}

distanceFromOrigin(point);
// Argument of type 'readonly [3, 4]' is not assignable to parameter of type '[number, number]'.
//   The type 'readonly [3, 4]' is 'readonly' and cannot be assigned to the mutable type '[number, number]'.
```

Even though `distanceFromOrigin` never modifies its elements, expects a mutable tuple.  
`point`'s type (`readonly [3,4]`) is not compatible with `[number, number]`,  
since that type can't guarantee `point`'s elements won't be mutated. 









