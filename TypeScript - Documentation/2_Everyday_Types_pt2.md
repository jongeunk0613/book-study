# Everyday Types

## Type Aliases
Instead of repeating the same custom types, name them through `type alias`; refer to a type by a single name and use it more than once.  

```JS
type Point = {
  x: number;
  y: number;
};

// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100});
```
Since they are only aliases, a type aliases cannot have different "versions" of the same type.  
<br/>

## Interfaces
Another way to name an object type is an `interface declaration`.  

```JS
interface Point = {
  x: number;
  y: number;
};

// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100});
```
The example above works just as if an anonymous object type was used.  
In this case, TypeScript is only concerned with the *structure* of the value passed to `printCoord` - it only cares that it has the expected properties &rarr; `structually typed type system`.  
<br/>

## Interface vs Type
Main difference: type cannot be re-opened to add new properties vs interface is always extendable.

<table>
	<tr>
		<th>Extending an interface</th>
		<th>Extending a type via intersections</th>
 	</tr>
 	<tr>
  		<td>
        <pre lang="javascript">
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear() 
bear.name
bear.honey
</pre>
      </td>
   		<td>
        <pre lang="javascript">
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: boolean 
}

const bear = getBear();
bear.name;
bear.honey;
      </pre>
    </td>
 	</tr>
</table>
<br/>

<table>
	<tr>
		<th>Adding new fields to an existing interface<br/>Declaration merging possible</th>
		<th>A type cannot be changed after being created<br/>No declaration merging</th>
 	</tr>
 	<tr>
  		<td>
        <pre lang="javascript">
// An interface can be re-opened
// and new values added:

interface Mammal {
    genus: string
}

interface Mammal {
    breed?: string
}

const animal: Mammal = {
    genus: "1234",
    // Fails because breed has to be a string
    breed: 1
}
</pre>
      </td>
   		<td>
        <pre lang="javascript">
type Reptile = {
    genus: string
}

// You cannot add new variables in the same way
type Reptile = {
    breed?: string
}
      </pre>
    </td>
 	</tr>
</table>
Interfaces can add properties to existing interface declarations while types can't.  
Types will considered these kind of actions as declaring duplicate types.  
<br/><br/>

<table>
	<tr>
		<th>Only used to declare the shapes of objects<br/> No renaming primitives</th>
		<th>Can rename primitives</th>
 	</tr>
 	<tr>
  		<td>
        <pre lang="javascript">
interface AnObject1 {
    value: string
}

// This isn't feasible with interfaces
interface X extends string {

}
</pre>
      </td>
   		<td>
        <pre lang="javascript">
type AnObject2 = {
    value: string
}

// Using type we can create custom names
// for existing primitives:

type SanitizedString = string
type EvenNumber = number
      </pre>
    </td>
 	</tr>
</table>
When trying to rename primitives with interfaces, error occurs; with types, primitives can be renamed.
<br/><br/>

<table>
	<tr>
		<th>Always appear in their original form in error messages<br/>only when they are used by name</th>
		<th></th>
 	</tr>
 	<tr>
  		<td>
        <pre lang="javascript">
// Compiler error messages will always use 
// the name of an interface:

interface Mammal {
    name: string
}

function echoMammal(m: Mammal) {
    console.log(m.name)
}

// e.g. The error below will always use the name Mammal 
// to refer to the type which is expected:
echoMammal({ name: 12343 })

// The type of `m` here is the exact same as mammal,
// but as it's not been directly named, TypeScript
// won't mention it in the error messaging

function echoAnimal(m: { name: string }) {
    console.log(m.name)
}

echoAnimal({ name: 12345 })
</pre>
      </td>
   		<td>
        <pre lang="javascript">
// Compiler error messages will always use 
// the name of an interface:

type Mammal = {
    name: string
}

function echoMammal(m: Mammal) {
    console.log(m.name)
}

// e.g. The error below will always use the name Mammal 
// to refer to the type which is expected:
echoMammal({ name: 12343 })

// The type of `m` here is the exact same as mammal,
// but as it's not been directly named, TypeScript
// won't mention it in the error messaging

function echoAnimal(m: { name: string }) {
    console.log(m.name)
}

echoAnimal({ name: 12345 })
      </pre>
    </td>
 	</tr>
</table>
With interface, the property error of `name` is shown as `Mammal.name`; with type, it is simply shown as `name`.
<br/><br/>
  
## Type Assertions
Use type assertions to specify and certify that a value is of a certain specific type.  
```JS
// Both are the same
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
// Don't use it inside .tsx files
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```
<br/>
Because type assertioes are removed at compile-time, there is no runtime checking associated with a type assertion.  
There won't be an exception or null generated if the type assertion is wrong.  
```JS
function toUpper(n) {
    return (n as string).toUpperCase();
}

console.log(toUpper(123));
```
<br/>
TypeScript only allows type assertions which convert to a *more specific or less specific* version of a type.  
**Impossible** coercions are prevented.  
```JS
const x = "hello" as number;
//Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
```
<br/>
This prevents complex coercions that might be valid.  
In that case, use two assertions as follow:  
```JS
const a = (expr as any) as T;
```
<br/>

## Literal Types
In addition to the general types `string` and `number`, we can refer to *specific* strings and numbers in type positions.  
Usually, literal types are used on `const` variable declaration; `const` variables' values doesn't change.  
```JS
let changingString = "Hello World";
changingString = "Olá Mundo";
// Because `changingString` can represent any possible string, that
// is how TypeScript describes it in the type system
changingString;
// ^? let changingString: string

const constantString = "Hello World";
// Because `constantString` can only represent 1 possible string, it
// has a literal type representation
constantString;
// ^? const constantString: "Hello World"
```
<br/>

By combining literals into unions, a function that only accepts certain set of known values can be created.  
```JS
// @errors: 2345
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
```
```JS
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}
```
```JS
// @errors: 2345
interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic");
```
<br/>
`Boolean` literals are also a kind of literal type.  
It is an alias for the union `true | false`.  

## Literal Inference
TypeScript assumes that the properties of an object might change values later.  
Therefore, instead of infering the type as a single literal type, if inferes its general type based on the literal.  
```JS
const obj = { counter: 0 };
if (someCondition) {
  obj.counter = 1;
}
// object properties can be read and written, therefore, instead of 0, it has the type of number.
```

```
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
// req.method has the type of string instead of GET because other methods such as POST, GUESS can happen
```

In the latter, case:
1. change the inference by adding a type assertion in either location
```JS
// Change 1:
const req = { url: "https://example.com", method: "GET" as "GET" };
// Change 2
handleRequest(req.url, req.method as "GET");
```
2. use `as const` to convert the entire object to be type literals
```JS
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```
<br/>

## `null` and `undefined`
`null` refers to `absent`, and `undefined` refers to `uninitialized`.  
They behave differently depending on the `strictNullChecks` option.  
<br/>

### `strictNullChecks` off
Values that might be `null` or `undefined` can still be accessed normally and be assigned to a property of any type.  
No null checks will be done.  
<br/>

### `strictNullChecks` on
When a value is `null` or `undefined`, testing for those values before using methods or properties on that value will be neccessary.  
Just like checking for `undefined` before using an optional property, *narrowing* can be used to check for values that might be `null`.  
```JS
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```
<br/>

### Non-null Assertion Operator (Postfix `!`)
Removes `null` and `undefined` from a type without doing any explicit checking &rarr; type assertion that the value isn't `null` or `undefined`.  
```JS
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```
Just like other type assertions, this doesn't change the runtime behavior of your code, so it's important to only use `!` when you know that the value *can't* be `null` or `undefined`.  
<br/>

## Enums






