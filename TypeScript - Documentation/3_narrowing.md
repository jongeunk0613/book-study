# Narrowing

`Narrowing` is the process of refining types to more specific types than declared.  
<br/>

## `typeof` type guards
`"string"`, `"number"`, `"bigint"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`.  

<img width="485" alt="image" src="https://user-images.githubusercontent.com/43084680/173069299-b50f15e7-3fd0-4b0c-a032-59815531d44a.png">

An `array` has the type of `"object"`.  
However, simply checking if `strs` is of `"object"` type is not enough since `null` also has the type of `"object"`.  
In the `printAll` function, it is only narrowed down to `string[] | null` instead of just `string[]`.  
<br/>

## Truthiness narrowing
In JavaScript, `if` statements don't expect their condition to always have the type `boolean`.  
<img width="426" alt="image" src="https://user-images.githubusercontent.com/43084680/173075401-361e588f-301b-42e5-b688-773f049f3bf9.png"><br/>
  
It first "coerce" the conditions to `boolean` and then choose their branches depending on whether the result is `true` or `false`.  
Values coerced to `false` : `0`, `NaN`, `""` (the empty string), `0n` (the `bigint` version of zero), `null`, `undefined`.  
Other values apart of the aboves are coerced to `true`.  
<br/>
You can explicitly coerce values to `Boolean` using the `Boolean` function or the double-Boolean negation.  
<img width="391" alt="image" src="https://user-images.githubusercontent.com/43084680/173076902-4ff03a13-71e3-4b31-8e70-6fa9b6ad1ab4.png"><br/>
! 두번 째 type boolean으로 나옴..  

By checking if a value is truthy, we can guard against values like `null` and `undefined`.  
<img width="392" alt="image" src="https://user-images.githubusercontent.com/43084680/173077434-fc6a779b-3291-42d7-8751-1629989e9815.png"><br/>
This prevents errors like : `TypeError: null is not iterable`.  

<br/>
However, truthiness checking on primitives can often be error prone.  
<img width="388" alt="image" src="https://user-images.githubusercontent.com/43084680/173079932-0e6780db-7864-41e5-93d8-37dc9213296a.png"><br/>
In this case, the empty string case is not handled correctly.  
When we choose to do **nothing** with a value, there's nothing TypeScript can do for us.  
`Linter` can help handle this kind of situations.  
<br/>

Boolean negations with `!` filter out from negated branches.  
<img width="342" alt="image" src="https://user-images.githubusercontent.com/43084680/173082872-39559b1c-8fcc-43c5-9885-a21c14f96e58.png"><br/>
<br/>

## Equality narrowing
`switch` statements and equality checks like `===`, `!==`, `==`, and `!=` can be used to narrow types.  
<img width="546" alt="image" src="https://user-images.githubusercontent.com/43084680/173083902-2d0e6041-0bb4-4e28-836e-0a039f1ac9c6.png"><br/>
Since `string` is the only common type that both `x` and `y` can have, TypeScript knows that `x` and `y` must be `string` in the first branch.  

Specific literal values can also be checked.  
<img width="472" alt="image" src="https://user-images.githubusercontent.com/43084680/173084590-3d6a08e9-e74e-4b2a-87c5-d63ff5dc6cdd.png"><br/>

Looser equality checks with `==` and `!=` also narrows correctly.  
`== null` checks both whether it's `null` or `undefined`.  
The same applies to `== undefined`.  
![image](https://user-images.githubusercontent.com/43084680/173084982-fec42200-f13f-4e65-a8f1-82d304b50204.png)<br/>






