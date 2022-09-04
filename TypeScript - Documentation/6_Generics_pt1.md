# Generics

&rarr; to create a component that can work over a variety of types, rather than a single one.  
&rarr; reusability

<br/>

# Hello World of Generics

The identity function &rarr; returns back whatever is passed on 

```javascript
function identity(arg: any): any {
  return arg;
}
```

`any` is generic in that it accepts any and all types for `arg`.  
However, the information about what that type was when the function returns is lost.  
We need a connection between the received `arg` and the returned `arg`.  

For this, **type variable** is used; variable that works on types rather than values.  

```javascript
function identity<Type>(arg: Type): Type {
  return arg;
}
```

With `Type`, we can capture the type the user provides and use the information later.  
Now we can know that the same type is used for the argument and the return type.  

This version of the `identity` function is generic, since it works over a range of types.  

`Generic` functions can be called in the following two ways:  

1) Pass all the arguments including the type argument

```javascript
let output = identity<string>("myString");
// let output : string
```

2) Use type argument inference 
The compiler sets the value of `Type` automatically based on the type of the passed argument  

```javascript
let output = identity("myString")
// let output: string
```

Sometimes the compiler might not be able to infer the type.  
On those occasions, it is neccesarry to explicitly pass the type in the angle brackets (`<>`).  

<br/>

# Working with Generic Types Variables






