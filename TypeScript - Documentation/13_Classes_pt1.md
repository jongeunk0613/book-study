# Class Members

The most basic class is an empty one.  
```ts
class Point {}
```

<br/>

# Fields

A field declaration creates a `public writeable porperty` on a class.  

```ts
class Point {
  x: number;
  y: number;
}

const pt = new Point();
pt.x = 0;   // (property) Point.x: number
pt.y = 0;   // (property) Point.y: number
```

<br/> 

The type annotation is optional, but will be an implicit `any` if not specified.  


```ts
class Point {
  x;
  y;
}

const pt = new Point();
pt.x = 0;   // (property) Point.x: any
pt.y = 0;   // (property) Point.y: any
```
<br/>

Fields can also have **initializers**.  
They are run automatically when the class is instantiated.  

```ts
class Point {
  x = 0;
  y = 0;
}
 
const pt = new Point();
console.log(`${pt.x}, ${pt.y}`);    // 0, 0
```

<br/>

The initializer of a class property will be used to infer its type.  

```ts
class Point {
  x = 0;    // number
  y = 0;    // number
}
 
const pt = new Point();
pt.x = "0";
// Type 'string' is not assignable to type 'number'.
```

<br/>

### `--strictPropertyInitialization`

This settings controls whether class fields need to be initialized int he constructor.  

```ts
class BadGreeter {
  name: string;
  // Property 'name' has no initializer and is not definitely assigned in the constructor.
}
```
<br/>

```ts
class GoodGreeter {
  name: string;
 
  constructor() {
    this.name = "hello";
  }
}
```

The fields need to be initialized in the constructor itself.  
TypeScript does not analyze methods you invoke from the constructor to detect initializations,  
because a derived class might override those methods and failt to initialize the members.  

To express that the field will be initialized through means other than the constructor,  
a **definite assignment assertion operator** `!` can be used.  

```ts
class OKGreeter {
  // Not initialized, but no error
  name!: string;
}
```

<br/>

# Readonly

Fields prefixed with the `readonly` modifier prevents assignments to the field outside of the constructor.  

```ts
class Greeter {
  readonly name: string = "world";

  constructor(otherName?: string) {
    if (otherName !== undefined) {
      this.name = otherName;
    }
  }

  err() {
    this.name = "not ok";
    // Cannot assign to 'name' because it is a read-only property
  }
}

const g = new Greeter();
g.name = "also not ok";
// Cannot assign to 'name' because it is a read-only property
```

<br/>

# Constructors

Constructors are similar to functions.  
You can add parameters with type annonations, default values, and overloads.  

```ts
class Point {
  x: number;
  y: number;
  
  // Normal signature with defaults
  constructor (x = 0, y = 0) {
    this.x = x;
    this.y = y;
  }
}
```

```ts
Class Point {
  // Overloads
  constructor(x: number, y: string);
  constructor(s: string);
  constructor(xs: any, y?: any) {
  // TBD
  }
}
```

<br/>

Differences between class constructor signatures and function signatures:
1 - Constructors can't have type parameters - these belong on the outer class declaration  
```ts
class Greeter {
  readonly name: string = "world";

  constructor<Type>(otherName?: string) {   // Type parameters cannot appear on a constructor declaration.
    if (otherName !== undefined) {
      this.name = otherName;
    }
  }
}
```
2 - Constructor can't have return type annotations - the class instance type is always what's returned  
```ts
class Greeter {
  readonly name: string = "world";

  constructor(otherName?: string) : none {    // Type annotation cannot appear on a constructor declaration.
    if (otherName !== undefined) {
      this.name = otherName;
    }
  }
}
```

<br/>

### Super Calls

Just as in JavaScript, if you have a base class, you'll need to class `super()`  
in your constructor body before using any `this.` members.  

```ts
class Base {
  k = 4;
}
 
class Derived extends Base {
  constructor() {
    // Prints a wrong value in ES5; throws exception in ES6
    console.log(this.k);
    // 'super' must be called before accessing 'this' in the constructor of a derived class.
    super();
  }
}
```

Forgetting to call `super` is an easy mistake to make in JavaScript,  
but TypeScript will tell you when it's necessary.  

<br/>

# Methods

A function property on a class is called **method**.  
Methods can use all the same type annotations as functions and constructors.  

```ts
class Point {
  x = 10;
  y = 10;
 
  scale(n: number): void {
    this.x *= n;
    this.y *= n;
  }
}
```

<br/>

Inside a method body, it is mandatory to access fields and other methods via `this.`.  

```ts
let x: number = 0;
 
class C {
  x: string = "hello";
 
  m() {
    // This is trying to modify 'x' from line 1, not the class property
    x = "world";
    // Type 'string' is not assignable to type 'number'.
  }
}
```

<br/>

# Getters / Setters

Classes can also have **accessors**.  

```ts
class C {
  _length = 0;
  get length() {
    return this._length;
  }
  set length(value) {
    this._length = value;
  }
}
```

TypeScript has some special inference rules for accessors:  
1. If `get` exists but no `set`, the property is automatically `readonly`  
2. If the type of the setter parameter is not specified, it is inferred from the return type of the getter
3. Getters and setters must have the same **Member Visibility**  
  - `public`, `private`, `protected`  


Since TypeScript 4.3, it is possible to have accessors with different types for getting and setting.  

```ts
class Thing {
  _size = 0;
 
  get size(): number {
    return this._size;
  }
 
  set size(value: string | number | boolean) {
    let num = Number(value);
 
    // Don't allow NaN, Infinity, etc
 
    if (!Number.isFinite(num)) {
      this._size = 0;
      return;
    }
 
    this._size = num;
  }
}
```

<br/>

# Index Signatures

Classes can declare index signatures.  
They work the same way as index signatures for other object types.  

```ts
class MyClass {
  [s: string]: boolean | ((s: string) => boolean);
 
  check(s: string) {
    return this[s] as boolean;
  }
}
```

It's not easy to usefully use index signature types because they have to capture the types of methods too.  
Generally, it's better to store indexed data in another place instead of on the class instance itself.  




