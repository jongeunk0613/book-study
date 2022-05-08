# The Basics
The only way in pure JavaScript to tell what some function does with a particular value is to `call it and see what happens` &rarr; **dynamic typing**.  
Using **static type system**, we can `make predictions` about what code is expected `before it runs`. 
<br/>

## Static type-checking
**Static type systems** help us `find bugs before running the code` by `describing the shapes and behaviors of` what our `values` will be when the code is run.  
```TypeScript
const message = "hello!";

message();
/* This expression is not callable.
  Type 'String' has no call signatures. */
```
<br/>







### References
- [typescriptlang - basic types](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
