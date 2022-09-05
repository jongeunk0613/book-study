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

