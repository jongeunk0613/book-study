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
