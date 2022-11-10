# Static Members

`static` members aren't associated with a particular instance of the class;  
they can be accessed through the class constructor object itself.  

```ts
class MyClass {
  static x = 0;
  static printX() {
    console.log(MyClass.x);
  }
}

console.log(MyClass.x);   // 0
MyClass.printX();         // 0
```

Static members can also use the name `public`, `protected`, and `private` visibility modifiers.  

```ts
class MyClass {
  private static x = 0;
}

console.log(MyClass.x);
// Property 'x' is private and only accessible within class 'MyClass'
```

Static members are also inherited:  

```ts
class Base {
  static getGreeting() {
    return "Hello World";
  }
}

class Derived extends Base {
  myGreeting = Derived.getGreeting();
}
```

<br/>

### Special Static Names

Properties from the `Function` prototype can't be overwritten, and even if you can, it's not safe.  
Classes are functions that can be invoked with `new`, therefore, certain `static` names can't be used.  
Properties names that can be defined as `static` members &rarr; `name`, `length`, `call`  

```ts
class S {
  static name = "S!";
  // Static property 'name' conflicts with built-in property 'Function.name' of constructor function 'S'
}
```

<br/>

### Why No Static Classes?

In TypeScript and JavaScript, there is no construct called `static class` like in C#.  

They exists only on those languages because they force all data and functions to be inside a class;  
because TypeScript doesn't have that restriction, there none on TypeScript (and JavaScript).  
A class with only a single instance is typically just represented as a normal object in JavaScript/TypeScript.  

There's no need of a "static class" syntax in TypeScript, because a regular object  
(or even top-level function) can do the same thing.  

```ts
// Unnecessary "static" class
class MyStaticClass {
  static doSomething() {}
}

// Preferred (alternative 1)
function doSomething() {}

// Preferred (alternative 2)
const MyHelperObject = {
  doSomething() {},
};
```

<br/>

# `static` Blocks in Classes

Static blocks allows writing a sequence of statements with their own scope  
that can access private fields within the containing class.  
Meaning, we can write initialization code with:  
- all the capabilities of writing statements
- no leakage of variables
- full access to our class's internals

```ts
class Foo {
  static #count = 0;

  get count() {
    return Foo.#count;
  }

  static {
    try {
      const lastInstances = loadLastInstances();
      Foo.#count += lastInstances.length;
    }
    catch {}
  }
}
```

<br/>

# Generic Classes

Classes can be generic just like interfaces.  
When a generic class is instantiated with `new`, its type parameters are inferred the same way as in a function call.  

```ts
class Box<Type> {
  contents: Type;
  constructor(value: Type) {
    this.contents = value;
  }
}

const b = new Box("hello!");
// const b: Box<string>
```

Classes can use generic constraints and defaults the same way as interfaces.  

<br/>

### Type Parameters in Static Members

This code is not legal.  

```ts
class Box<Type> {
  static defaultValue: Type;
  // Static members cannot reference class type parameters. 
}
```

Types are fully erased after compilation.  
Therefore, at runtime, there's only one `Box.defaultValue` property slot.  
This means that setting `Box<string>.defaultValue` (if possible) would also change `Box<number>.defaultValue`.  
The `static` members of a generic class can never refer to the class's type parameters. 

<br/>

# `this` at Runtime in Classes

TypeScript doesn't change the runtime behavior of JavaScript.  
And JavaScript is famous for having some peculiar runtime behaviors.  

JavaScript's handling of `this` is unusual.  

```ts
class MyClass {
  name = "MyClass";

  getName() {
    return this.name;
  }
}

const c = new MyClass();

const obj = {
  name: "obj",
  getName: c.getName,
};

// prints "obj", not "MyClass"
console.log(obj.getName());
```

The value of `this` inside a function depends on how the function is called.  
Because the function was called through the `obj` reference, its value of `this` was `obj` rather than the class instance.  

This is not what was intended and TypeScript provides some ways to mitigate or prevent this kind of error.  

<br/>

### Arrow Functions

To not loose the content of `this` when calling a function, use an arrow function property instead.  

```ts
class MyClass {
  name = "MyClass";
  getName = () => {
    return this.name;
  };
}

const c = new MyClass();
const g = c.getName;
console.log(g());   // "MyClass";
```

Some trade-offs:  
- `this` value is guaranteed to be correct at runtime, even for code not checked with TypeScript  
- Uses more memory, because each class instance will have its own copy of each function defined this way  
- You can't use `super.getName` in a derived class, because there's no entry in the prototype chain to fetch the base class method from  

<br/>

### `this` parameters

In a method or function definition, an initial parameter named `this` has special meaning in TypeScript.  
They are erased during compilation.  

```ts
function fn(this: SomeType, x: number) {
  /*...*/
}
```

```js
function fn(x) {
  /*... */
}
```

TypeScript checks that calling a function with a `this` parameter is done so with a correct context.  
Instead of using an arrow function, the `this` parameter can be added to method definitions  
to statically enforce that the methods are called correctly.  

```ts
class MyClass {
  name = "MyClass";
  
  getName (this: MyClass) {
    return this.name;
  }
}

const c = new MyClass();
c.getName();

const g = c.getName;
console.log(g());
// The 'this' context of type 'void' is not assignable to method's 'this' of type 'MyClass'
```

It has the opposite trade-offs of the arrow function approach:  
- JavaScript callers might still use the class method incorrectly without realizing it
- Only one function per class definition gets allocated, rather than one per class instance
- Base method definitions can still be called via `super`  

<br/>
