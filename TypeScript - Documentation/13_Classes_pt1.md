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


