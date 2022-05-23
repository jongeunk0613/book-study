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
