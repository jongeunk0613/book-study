# Object Types

### Property Modifiers

Properties in an object can specify the **type**, whether the property is **optional**, and whether the property **can be written** to.  
<br/>

### Optional Properties
Mark *optional* properties by adding a question mark `?` to the end of their names.  

```JS
interface PaintOptions {
    shape: Shape;
    xPos?: number;
    yPos?: number;
}

function paintShape(opts: PaintOptions) {
    // ...
}

const shape = getShape();
paintShape({ shape });
paintShape({ shape, xPos: 100 });
paintShape({ shape, yPos: 100 });
paintShape({ shape, xPos: 100, yPos: 100 });
```

<br/>

What optionality really says is that *if* the property *is set*, it better have a specific type.  

Optional properties can be read.  
But with `strictNullChecks`, TypeScript tell us they're potentially `undefined`.  

```JS
function paintShape(opts: PaintOptions) {
    // (property) PaintOptions.xPos?: number | undefined
    let xPos = opts.xPos;
    let yPos = opts.yPos;
    // ....
}
```

<br/>

In JavaScript, when we access an optional property that is not set, we get the value `undefined`.  
We can just handle `undefined` specially.  
```JS
function paintShape(opts: PaintOptions) {
    // let xPos: number 
    let xPos = opts.xPos === undefined ? 0 : opts.xPos;
    let yPos = opts.yPos === undefined ? 0 : opts.yPos;
    // ....
}
```
<br/>

This pattern of setting defaults for unspecified values is so common that JavaScript has syntax to support it.  

```JS
function paintShape({shape, xPos = 0, yPos = 0}: PaintOptions ){
    console.log("x coordinate at ", xPos);
    console.log("y coordinate at ", yPos);
    // ....
}
```

With this syntax, default values are provided for `xPos` and `yPos`.  
They will be present within the body of `paintShape`, but optional for any callers to `paintShape`.  
<br/>

> Note: there is currently no way to place type annonations within destructuring patterns, because the following syntax already means smoething different in JavaScript. 

```JS
function draw({shape: Shape, xPos: number = 100 /*...*/ }) {
    render(shape);
    // Cannot find name 'shape'. Did you mean 'Shape'?
    render(xPos);
    // Cannot find name 'xPos'.
}
```

In an object destructuring pattern, `shape: Shape` means "grab the property `shape` and redefine it locally as a variable named `Shape`".  
Likewise `xPost: number` creates a variable named `number` whose value is based on the parameter's `xPos`.  
<br/>

### `readonly` Properties
`readonly` properties don't have a different behavior at runtime,  
but it can't be written to during type-checking.  

```JS
interface SomeType {
    readonly prop: string;
}

function doSomething(obj: SomeType) {
    // Can be read
    console.log(`prop has the value '${obj.prop}'.`);

    // But it can't be written
    obj.prop = "hello";
    // Cannot assign to 'prop' because it is a read-only property.
}
```

Using `readonly` modifier **doesn't neccesarily imply** that a value is **totally immutable**; that its internal contents can't be changed.  
It just means the property itself **can't be re-written to**.  

```JS
interface Home {
    readonly resident: {name: string; age: number};
}

function visitForBirthday(home: Home) {
    // Can be read and updated
    console.log(`Happy birthday ${home.resident.name}!`);
    home.resident.age++;
}

function evict(home: Home) {
    // Can't be written to the 'resident' property itself on a 'Home'
    home.resident = {
        // Cannot assign to 'resident'' because it is a read-only property. 
        name: "Victor the Evictor",
        age: 42
    }
}
```

TypeScript doesn't check whether properties on two types are `readonly` when  
checking whether those types are compatible,  
so `readonly` properties can also change via aliasing.  

```JS
interface Person {
    name: string;
    age: number;
}

interface ReadonlyPerson {
    readonly name: string;
    readonly age: number;
}

let writablePerson: Person = {
    name: "Person McPersonface",
    age: 42,
}

// works
let readonlyPerson: ReadonlyPerson = writablePerson;

console.log(readonlyPerson.age);    // prints '42'
writablePerson.age++;
console.log(readonlyPerson.age);    // prints '43'
```

### Index Signatures

Used when the **names of a type's properties** is **not known** ahead of time,  
but the **shape of the values are known**.  

In this case, an index signature can be used to describe the types of possible values.  

```JS
interface StringArray {
    [index: number]: string;
}

cont myArray: StringArray = getStringArray();
const secondItem = myArray[1];
// const secondItem: string
```

The index signature of `StringArray` states that when a `StringArray` is indexed with a `number`, it will return a `string`.  

An index signature property type must be either 'string' or 'number'. 

<hr/>

#### Possible to support both types of indexers

It is possible to support both types of indexers,  
but the type returned from a numeric indexer must be a subtype of the type returned from the string indexer.  
This is because when indexing with a `number`, JavaScript will actually convert that to a `string` before indexing into an object.  
ex: indexing with `100` (number) is the same thing as indexing with `"100"` (string).  

```JS
interface Animal {
    name: string;
}

interface Dog extends Animal {
    breed: string;
}

// Error: indexing with a numeric string might get you a completely separate type of Animal
interface NotOkay {
    // 'number' index type 'Animal' is not assignable to 'string' index type 'Dog'.
    [x: number]: Animal;
    [x: string]: Dog;
}

// Possible because Dog is a subtype of Animal
interface Okay {
    [x: number]: Dog,
    [x: string]: Animal
}
```
<hr/>

String signatures are a powerful way to describe the "dictionary" pattern.  
But they also enforce that all properties match their return type.  
This is because string index declares that `obj.property` is also available as `obj["property"]`.  

```JS
interface NumberDictioanry {
    [index: string]: number;

    length: number; // ok
    name: string;
    // Property 'name' of type 'string' is not assignable to 'string' index type 'number'
}
```

However, properties of different types are acceptable if the singature is a union of the property types.  
```JS
interface NumberOrStringDictionary {
    [index: string] : number | string;
    length: number;
    name: string;
}
```

Index signatures can be made `readonly` in order to prevent assignment to their indices.  
```JS
interface ReadonlyStringArray {
    readonly [index:number]: string;
}

let myArray: ReadonlyStringArray = {0: "object0", 1: "object1"};
console.log(myArray[0]);
myArray[2] = "Mallory"
// Index signature in type 'ReadonlyStringArray' only permits reading.
```

You can't set myArray[2] because the index signature is `readonly`.  
<br/>

### Extending Types

By using the `extends` keyword, we can avoid writting the same properties over again.  
It copies members from other named types, and add the declared new members to the new type.  
It is useful for not repeating the similar types and for signaling intent that several different declarations of the same property might be related.  

```JS
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```

<br/>
`interface`s can also extend from multiple types.  

```JS
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

interface ColorfulCircle extends Colorful, Circle {}

const cc: ColorfulCircle = {
  color: "red",
  radius: 42,
}
```
<br/>

### Intersection Types

**Intersection Types** is used to combine existing object types by using the `&` operator.  
```JS
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;
// Has all members of Colorful and Circle

function draw(circle: Colorful & Circle) {
  console.log(`Color was ${circle.color}`);
  console.log(`Radius was ${circle.radius}`)
}

// okay
draw({color: "blue", radius: 42});

// oops
draw({color: "red", raidus: 42});

// Argument of type '{ color: string; raidus: number; }' is not
// assignable to parameter of type 'Colorful & Circle'.
// Object literal may only specify known properties, but 'raidus'
// does not exist in type 'Colorful & Cirlce'. Did you mean to write 'radius'?
```
<br/>

### Interfaces vs. Intersections

They are similar but different.  
Similarity  
- `interfaces` &rarr; uses `extends` to extend from other types  
- `intersection` &rarr; can do something similar with `&` and name the result with a type alias  

Difference  
- Difference in how conflicts are handled

<br/>
<hr/>

### Intersections vs Extends

#### `Extends` can be used only with types with statically known members

```JS
type SimpleType = {x: string};
interface SimpleInterface extends SimpleType {};    // ok

type ComplexType = {x: string} | {x: number};
interface ComplexInterface extends ComplexType {};
// An interface can only extend an object type or intersection of object types with statically known members.
```

`Intersection` is a general type operation that can be performed on any two types and will give a result.  
`Extend` is limited to interfaces and interface-like types (and classes).  

<br/>

#### `Extends` can't create fields with the same name as in the parent with wider type

```JS
interface Parent {
    x: string | number;
}

interface Child1 extends Parent {
    x: string;  // ok
}

interface Child2 extends Parent {
    x: string | number | boolean;   // error
}
// Interface 'Child2' incorrectly extends interface 'Parent'.
//   Types of property 'x' are incompatible.
    // Type 'string | number | boolean' is not assignable to type 'string | number'.
    //   Type 'boolean' is not assignable to type 'string | number'.
```

On the other hand, type `intersection` does not complain.  

```JS
interface Parent {
    x: string | number;
}

type IntersectedChild = Parent & {x: string | boolean };    // ok
// IntersectedChild will have property 'x' that is an intersection 
// of 'string | number' and 'string | boolean', that is a 'string'.
type IntersectedChildX = IntersectedChild['x']; // string
```
<br/>

#### Members with same property key

`extend` throws an error when the derriving interface declares a property  
with the same key as the one in the derived interface.  

```JS
interface NumberToStringConverter {
    convert: (value: number) => string;
}

interface BidirectionalStringNumberConverter extends NumberToStringConverter {
    convert: (value: string) => number;
}

// Interface 'BidirectionalStringNumberConverter' incorrectly extends interface 'NumberToStringConverter'.
//   Types of property 'convert' are incompatible.
//     Type '(value: string) => number' is not assignable to type '(value: number) => string'.
//       Types of parameters 'value' and 'value' are incompatible.
//         Type 'number' is not assignable to type 'string'.
```

With `intersection`, no error is given and both properties can be used.  
```JS
interface NumberToStringConverter {
    convert: (value: number) => string;
}

type BidirectionalStringNumberConverter = NumberToStringConverter & {
    convert: (value: string) => number;
}

const converter: BidirectionalStringNumberConverter = {
    convert: (value: string | number) => {
        return (typeof value === 'string' ? Number(value) : String(value)) as string & number;  // type assertion needed
    }
}

const s: string = converter.convert(0);
const n: number = converter.convert('a');

```

<hr/>
<br/>




