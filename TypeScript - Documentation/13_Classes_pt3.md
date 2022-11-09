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








