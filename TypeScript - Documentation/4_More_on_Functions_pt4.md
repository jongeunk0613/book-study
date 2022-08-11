# Other Types to Know About

### `void`
Used when a function **does not return a value**.  
It's the inferred type when a function doesn't have any **`return` statements**,  
or doesn't return any **explicit value** from those return statements.  

```JS
function noop() {
  return; // void
}
```
<br/>

> `void` is not the same as `undefined`.
<br/>

### `object`
Refers to any value that **isn't a primitive**.  
It's different from the empty object type `{ }` and from the global type `Object`.  
It's very likely that `Object` will not be used.  
> `object` is not `Object`. **Always** use `object`!
<br/>

In JavaScript function values are objects:
- they have properties
- have `Object.prototype` in their prototype chain
- are `instanceOf Object`
- can call `Object.keys` on them

```JS
console.log(add.length) // 2
console.log(add.prototype)  // 
console.log(add instanceof Object)  // true
console.log(Object.keys(add))   // []
```

#### `object` vs `Object` vs `{ }`

### `unknown`
