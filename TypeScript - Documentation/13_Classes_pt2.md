# Class Heritage

Classes in JavaScript can inherit from base classes  

## `implements` Clauses

`implements` clause is used to check that a class satisfies a particular `interface`.  
An error will occur if a class sails to correctly implement it.  

```ts
interface Pingable {
  ping(): void;
}

class Sonar implements Pingable {
  ping() {
    console.log("ping!");
  }
}

class Ball implements Pingable {
// Class 'Ball' incorrectly implements interface 'Pingable'
//  Property 'ping' is missing in type 'Ball' but required in type 'Pingable'
  pong() {
    console.log("pong!");
  }
}
```

Classes may also implement multiple interfaces, e.g. `class C implements A, B {`.  

```ts
interface Pingable {
  ping(): void;
}

interface Pongable {
  pong(): void;
}

class Sound implements Pingable, Pongable {
  ping() {
    console.log("ping");
  }

  pong() {
    console.log("pong");
  }
}
```

<br/>

### Cautions

`implements` clause only checks that the class can be treated as the interface type.  
It doesn't change the type of the class or its methods at all.  

A common source of error is to assume that an `implements` clause will change the class type.  

```ts
interface Checkable {
  check(name: string): boolean;
}

class NameChecker implements Checkable {
  check(s) {    // Parameter 's' implicitly has an 'any' type
    // No error here
    return s.toLowerCase() === "ok";
    // (parameter) s: any
  }
}
```

<br/>

Similarly, implementing an interface with an optional property doesn't create that property.  

```ts
interface A {
  x: number;
  y?: number;
}

class C implements A {
  x = 0;
}

const c = new C();
c.y = 10;
```

<br/>

## `extends` Clauses

Classes may `extend` from a base class.  
A derived class has all the properties and methods of its base class,  
and also define additional members.  

```ts
class Animal {
  move() {
    console.log("Moving along!");
  }
}

class Dog extends Animal {
  woof(times: number) {
    for (let i = 0; i < times; i++) {
      console.log("woof!");
    }
  }
}

const d = new Dog();
d.move();   // Base class method
d.woof(3);  // Derived class method
```

<br/>

### Overriding Methods

A derived class can override a base class field or property.  
Base class methods can be accessed by using `super.`  

```ts
class Base {
  greet() {
    console.log("Hello World!");
  }
}

class Derived extends Base {
  greet(name?: string) {
    if (name === undefined) {
      super.greet();
    } else {
      console.log(`Hello, ${name.toUpperCase()}`);
    }
  }
}

const d = new Derived();
d.greet();
d.greet("reader");
```

It's important that a derived class follow its base class contract.  
It is very common and always legal to refer to a derived class instance through a base class reference.  

```ts
// Alias the derived instance through a base class reference
const b: Base = d;
b.greet();    // No problem
```

What if `Derived` didn't follow `Base`'s contract?

```ts
class Base {
  greet() {
    console.log("Hello World!");
  }
}

class Derived extends Base {
  // Make the parameter required
  greet(name: string) {
  // Property 'greet' in type 'Derived' is not assignable to the same property in base type 'Base'.
  //   Type '(name: string) => void' is not assignable to type '() => void'.
    console.log(`Hello, ${name.toUpperCase()}`);
  }
}
```

If we compile this code, the sample code will crash.  

```ts
const b: Base = new Derived();
b.greet();    // Crashes because "name" will be undefined
```

<br/>

### Type-only Field Declaractions

When `target >= ES2022` or `useDefineForClassFields` is `true`,  
class fields are initialized after the parent class constructor completes,  
overwriting any value set by the parent class.  

This can be a problem when you only want to re-declare a more accurate type for an inherited field.  
To handle these cases, you can write `declare` to indicate to TypeScript that  
there should be no runtime effect for this field declaration.  

```ts
interface Animal {
  dateOfBirth: any;
}

interface Dog extends Animal {
  breed: any;
}

class AnimalHouse {
  resident: Animal;
  constructor(animal: Animal) {
    this.resident = animal;
  }
}

class DogHouse extends AnimalHouse {
  // Does not emit JavaScript code,
  // only ensures the types are correct
  declare resident: Dog;
  constructor(dog: Dog) {
    super(dog);
  }
}
```

<br/>

### Initialization Order

THe order that JavaScript classes initialize can be surprising in some cases.  

```ts
class Base {
  name = "base";
  constructor() {
    console.log("My name is " + this.name);
  }
}

class Derived extends Base {
  name = "derived";
}

const d = new Derived();
```

The order of class initialization:  
- The base class fields are initialized
- The base class constructor runs
- THe derived class field are initialized
- The derived class constructor runs

<br/>

### Inheriting Built-in Types

> NOTE: if you don't plant to inherit from built-in types like `Array`, `Error`, `Map`, etc. 
> or your compilation target is explicitly set to ES6/ES2015 or above, this section is not important

In ES2015, constructors which return an object implicitly substitute the value of `this` for any callers of `super(...)`.  
It is necessary for generated constructor code to capture any potential return value of `super(...)` and replace it with `this`.  

As a result, subclassing `Error`, `Array`, and others may no longer work as expected.  
This is due to the fact that constructor functions for `Error`, `Array`, and the like use ECMAScript 6's `new.target` to adjust the prototype chain;  
however, there is no way to ensure a value for `new.target` when invoking a constructor in ECMAScript 5.  
Other downlevel compilers generally have the same limitation by default.  

For example, subclass like this:  

```ts
class MsgError extends Error {
  constructor (m: string) {
    super(m);
  }

  sayHello() {
    return "hello" + this.message;
  }
}

const msgerror = new MsgError("world");
console.log(msgerror.sayHello());
// target => ES5
// [ERR]: "Executed JavaScript Failed:" 
// [ERR]: msgerror.sayHello is not a function 
```

you may find that:  
- methods may be `undefined` on objects returned by constructing these subclasses, so calling `sayHello` will result in an error.  
- `instanceof` will be broken between instances of the subclass or their instances, so `(new MsgError()) instanceof MsgError` will return `false`.  

As a recommendation, you can manually adjust the prototype immediately after any `super(...)` calls.  

```ts
class MsgError extends Error {
  constructor(m: string) {
    super(m);
    
    // Set the property explicitly
    Object.setPrototypeOf(this, MsgError.prototype);
  }
  
  sayHello() {
    return "hello " + this.message;
  }
}

const msgerror = new MsgError("world");
console.log(msgerror.sayHello());   // "hello world"
```

However, any subclass of `MsgError` will have to manually set the prototype as well.  
For runtimes that don't support `Object.setPrototypeOf`, you may instead be able to use `__proto__`.  

<br/>

# Member Visibility

With TypeScript, you can control whether certain methods or properties are visible to code outside the class.  

### `public`

It's the default visibility of class members.  
A `public` member can be accessed anywhere.  

```ts
class Greeter {
  public greet() {
    console.log("hi!");
  }
}

const g = new Greeter();
g.greet();
```

Because `public` is the default visibility modifier, you don't need to write it on a class member.  
But might choose to do so for style/readability reasons.  

<br/>

### `protected`

`protected` members are only visible to subclasses of the class they're declared in.  

```ts
class Greeter {
  public greet() {
    console.log("hi!");
  }
  protected getName() {
    return "hi";
  }
}

class SpecialGreeter extends Greeter {
  public howdy() {
    // OK to access protected member here
    console.log("Howdy, " + this.getName());
  }
}

const g = new SpecialGreeter();
g.greet();    // OK
g.getName();
// Proterty 'getName' is protected and only accessible within class 'Greeter' and its subclasses
```

<br/>

### Exposure of protected members

Derived classes need to follow their base class contracts,  
but may choose to expose a subtype of base class with more capabilities.  
ex: making `protected` members `public`  

```ts
class Base {
  protected m = 10;
}

class Derived extends Base {
  // No modifier, so default is 'public'
  m = 15;
}

const d = new Derived();
console.log(d.m);   // OK
```

Note that `Derived` was already able to freely read and write `m`,  
so this doesn't meaningfully alter the "security" of this situation.  

The main thing to note here is that in the derived class,  
wee need to be careful to repeat the `protected` modifier if this exposure isn't intentional.  

<br/>

### Cross-hierarchy protected access

Different OOP languages disagree about whether it's legal to access a `protected` member through a base class reference.  

```ts
class Base {
  protected x: number = 1;
}

class Derived1 extends Base {
  protected x: number = 5;
}

class Derived2 extends Base {
  f1(other: Derived2) {
    other.x = 10;
  }

  f2(other: Base) {
    other.x = 10;
    // Property 'x' is protected and only accessible through an instance of class 'Derived2'. 
    // This is an instance of class 'Base'.
  }
}
```

TypeScript treats this as illegal,  
because accessing `x` in `Derived2` should only be legal from `Derived2`'s subclasses,  
and `Derived1` isn't one of them.  
Likely, accessing it through a base class reference should be illegal too.  

<br/>

### `private`

`private` is like `protected`, but doesn't allow access to the member even from subclasses.  

```ts
class Base {
  private x = 0;
}

const b = new Base();
// Can't access from outside the class
console.log(b.x);
// Property 'x' is private and only accessible within Class 'Base'

class Derived extends Base {
  showX() {
    // Can't access in subclasses
    console.log(this.x);
    // Property 'x' is private and only accessible within Class 'Base'
  }
}
```

Because `private` members aren't visible to derived classes, a derived class can't increase its visibility.  

```ts
class Base {
  private x = 0;
}

class Derived extends Base {
  // Class 'Derived' incorrectly extends base class 'Base'.
  //  Property 'x' is private in type 'Base' but not in type 'Derived'
  x = 1;
}
```

<br/>

### Cross-instance private access

Different OOP languages disagree about whether different instances of the same class may access each other's `private` members.  
TypeScript allows cross-instance `private` access.  

```ts
class A {
  private x = 10;

  public sameAs(other: A) {
    // No error
    return other.x === this.x;
  }
}
```

<br/>

### Caveats

Like other aspects of TypeScript's type system,  
`private` and `protected` are only enforced during type checking.  
Meaning, javaScript runtime constructs like `in` or simple property lookup  
can still access a `private` or `protected` member.  

```ts
class MySafe {
  private secretKey = 12345;
}


// In a JavaScript file...
const s = new MySafe();
console.log(s.secretKey);
```

`private` also allows access using bracket notion during type checking.  
This makes `private` declared fields potentially easier to access for things like unit tests,  
with the drawback that these fields are **soft private** and don't strictly enforce privacy.  

```ts
class MySafe {
  private secretKey = 12345;
}

const s = new MySafe();

// Not allowed during type checking
console.log(s.secretKey);
// Property 'secretKey' is private and only accessible within class 'MySafe'.

// ok
console.log(s["secretKey"]);
```

<br/>

Unlike TypeScript's `private`, JavaScript's private fields `#` remain private after compilation  
and do not provide the previously mentioned escape hatches like bracket notation access, making them **hard private**  

```ts
class Dog {
  #barkAmount = 0;
  personality = "happy";

  constructor() {}
}
```

When compiling to ES2021 or less, TypeScript will use WeakMaps in place of `#`  

```ts
"use strict";
var _Dog_barkAmount;
class Dog {
    constructor() {
        _Dog_barkAmount.set(this, 0);
        this.personality = "happy";
    }
}
_Dog_barkAmount = new WeakMap();
```







