# Conditional Types

It is similar to **conditonal expressions** (`condition ? trueExpression : falseExpression`) in JavaScript.  

```javascript
SomeType extends OtherType ? TrueType : FalseType;
```

It helps describe the relation between the types of inputs and outputs.  

```javascript
interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}
 
type Example1 = Dog extends Animal ? number : string;
// type Example1 = number
 
type Example2 = RegExp extends Animal ? number : string;
// type Example2 = string
```

<br/>
They are very useful specially when used with generics.  

ex: `createLabel`

```javascript
interface IdLabel {
  id: number /* some fields */;
}
interface NameLabel {
  name: string /* other fields */;
}
 
function createLabel(id: number): IdLabel;
function createLabel(name: string): NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel {
  throw "unimplemented";
}
```

This kind of code have 2 inconveniences:  
1. If APIs are made in this way, they become cumbersome.  
2. Various overloads are needed. If `createLabel` handles additional types, more overloads will be needed.  

<br/>

If we write this with conditional types, we can simplify our overloads to a single function with no overloads.  

```js
interface IdLabel {
  id: number /* some fields */;
}
interface NameLabel {
  name: string /* other fields */;
}
 
type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel;
```

```javascript
function createLabel<T extends number | string>(idOrName: T): NameOrId<T> {
  throw "unimplemented";
}
 
let a = createLabel("typescript");
// let a: NameLabel
 
let b = createLabel(2.8);
// let b: IdLabel
 
let c = createLabel(Math.random() ? "hello" : 42);
// let c: NameLabel | IdLabel
```

<br/>

## Conditional Type Constraints

The following raises an error because `T` isn't known to have a property called `message`.  

```javascript
type MessageOf<T> = T["message"];
// Type '"message"' cannot be used to index type 'T'.
```

By constraining `T`, no error will occur.  

```javascript
type MessageOf<T extends { message: unknown }> = T["message"];
 
interface Email {
  message: string;
}
 
type EmailMessageContents = MessageOf<Email>;
// type EmailMessageContents = string
```

<br/>
What if we want to make `MessageOf` take any type and default to `never` if a message property isn't available?  
We can do this by moving the constraint out and introducing a conditional type.  

```javascript
type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;
 
interface Email {
  message: string;
}
 
interface Dog {
  bark(): void;
}
 
type EmailMessageContents = MessageOf<Email>;
// type EmailMessageContents = string
 
type DogMessageContents = MessageOf<Dog>;
// type DogMessageContents = never
```

Within the true branch, TypeScript knows that `T` will have a `message` property.  

<br/>
Another example: `Flatten` type that flattens array types to their element types, but leaves them alone otherwise.  

```javascript
type Flatten<T> = T extends any[] ? T[number] : T;
 
// Extracts out the element type.
type Str = Flatten<string[]>;
// type Str = string
 
// Leaves the type alone.
type Num = Flatten<number>;
// type Num = number
```

When `Flatten` is given an array type, it uses an indexed access with `number` to fetch out `string[]`'s element type.  
Otherwise, it just returns the type it was given.  

<br/>

## Inferring Within Conditional Types


