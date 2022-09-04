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

When using generic type variables, the compiler will make sure that you use any  
generically types paramters in the body of the function correctly.  
We must treat these parameters as if they could be any and all types.  

```javascript
function loggingIdentity<Type>(arg: Type): Type {
  console.log(arg.length);
  // Property 'length' does not exist on type 'Type'.
  return arg;
```

If a `number` type argument is passed, it won't have a `.length` member,  
so an error is caused.  

If this function works on arrays of `Type` rather than `Type`,  
the `.length` member will be always available, and the error will not be caused.  

```javascript
function loggingIdentity<Type>(arg: Type[]): Type[] {
  console.log(arg.length);
  return arg;
}

function loggingIdentity<Type>(arg: Array<Type>): Array<Type> {
  console.log(arg.length);
  return arg;
}
```

The type of `loggingIdentity` is read as "the generic function `loggingIdentity` takes a type parameter `Type`,  
and an argument `arg` which is an array of `Type`s, and returns an array of `Type`s."  

<br/>

# Generic Types

Type of generic functions and how to create generic interfaces.  

The type of generic functions is like those of non-generic functions:  
the type parameters listed first, similar to function declarations.  

```javascript
function identity<Type>(arg: Type): Type {
  return arg;
}

let myIdentity: <Type>(arg: Type) => Type = identity;
```

A different name for the generic type parameter can be used as long as  
the number of type variables and how the type variables are used line up.  

```javascript
let myIdentity2: <Input>(arg: Input) => Input = indetity;
```

The generic type can also be written as a call signature of an object literal type.  
```javascript
let myIdentity: { <Type>(arg: Type): Type } = identity;
```

Using with the interface:  
```javascript
interface GenericIdentityFn {
    <Type>(arg: Type): Type;
}

function identity<Type>(arg:Type): Type {
    return arg;
}

let myIdentity: GenericIdentityFn = identity;
```

It's allso possible to make the generic parameter to be a parameter of the whole interface.  
This way, the type parameter is visible to all the other members of the interface.  

```javascript
interface GenericIdentityFn<Type>{
    (arg: Type): Type;
}

function identity<Type>(arg:Type): Type {
    return arg;
}

let myIdentity: GenericIdentityFn = identity;
// Generic type 'GenericIdentityFn<Type>' requires 1 type argument(s).
let myIdentity2: GenericIdentityFn<number> = identity
```

In this case, we have a non-generic function signature that is part of a generic type.  
Now, we will need to specifty the corresponding type argument in order to use `GenericIdentityFn` to  
effectively lock in what the underlying call signatrue will use.  

Type parameter directly on the call signature vs type parameter on the interface itself  



