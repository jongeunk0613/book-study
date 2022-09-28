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

### Conditional Type Constraints

