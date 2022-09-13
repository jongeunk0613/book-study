# Generic Classes 

Similar to generic interfaces, generic classes  have a generic type parameter list in angle brackets (`<>`) following the name of the class.  

```javascript
class GenericNumber<NumType> {
  zeroValue: NumType;
  add: (x: NumType, y: NumType) => NumType;
}
 
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

It can be used with other types too.  

```javascript
let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function (x, y) {
  return x + y;
};
 
console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```

Just as with interface, putting the type parameter on the class itself  
lets us make sure all of the properties of the class are working with the same type. 

A class has two sides to its type: the static side and the instance side.  
Generic classes are **only generic over their instance side** rather than their static side.  
So when working with classes, static memebrs cannot use the class's type parameter.  

```javascript
class Box<Type> {
  static defaultValue: Type;
  // Static members cannot reference class type parameters.
}
```

At runtime, types are fully erased and then there will be only one `Box.defaultValue` property slot.  
This means that setting `Box<string>.defaultValue` (if possible) would also change Box<number>.defaultValue.  
Therefore, the static members of a generic class can never refer to the class’s type parameters.  

<br/>

# Generic Constraints

Used to write a generic function that works on a set of types with specific properties.  
```JS
  function loggingIdentity<Type>(arg: Type): Type {
    console.log(arg.length);
    // Property 'length' does not exist on type 'Type'.
    return arg;
  }
```
  
We want to constrain this function to work with any and all types that  
**also** have the `.length` property.  
For this, we must list our requirement as a constraint on what `Type` can be.  

How?  
First, create an interface that describes the constraint.  
Then, Use this interface and the `extends` keyword to denote our constraint.  

```JS
  interface Lengthwise {
    length: number
  }
  
  function logginIdentity<Type extends Lengthwise>(arg: Type): Type {
    console.log(arg.length); // Now we know it has a .length property. No more error is throwed
    return arg;
  }
```
  
Because the generic function is now constrained, it will no longer work over any and all types.  
```JS
  logginIdentity(3);
  // Argument of type 'number' is not assignable to parameter of type 'Lengthwise'.
  
  logginIdentity({ length: 10, value: 3});
```
<br/>
  
# Using Type Parameters in Generic Constraints
It is possible to declare a type parameter that is constrained by another type parameter.  

ex: we want to get a property from an object given its name.  
We want to ensure that we're not accidentally grabbing a propety that does not exist on the `obj`,  
so we palce a constraint between the two types.  

```JS
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

let x = {a: 1, b: 2, c: 3, d: 4};

getProperty(x, "a");
getProperty(x, "m");
// Argument of type "m" is not assignable to parameter of type "a" | "b" | "c" | "d"
```

<br/>
  
# Using Class Types in Generics

When creating factories in TypeScript using generics,  
it is necessary to refer to class types by their constructor functions.  
  
```js
function create<Type>(c: { new (): Type }): Type {
  return new c();
}
```
  
A more advanced example uses the prototype property to infer and constrain relationships  
between the constructor function and the instance side of class types.  

```JS
class BeeKeeper {
    hasMask: boolean = true;
}

class ZooKeeper {
    nametag: string = "Mikle";
}

class Animal {
    numLegs: number = 4;
}

class Bee extends Animal {
    keeper: BeeKeeper = new BeeKeeper();
}

class Lion extends Animal {
    keeper: ZooKeeper = new ZooKeeper();
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;
createInstance(Bee).keeper.hasMask;
```

This pattern is used to power the mixins design pattern.  

