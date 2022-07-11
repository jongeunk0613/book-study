# More on Functions

### Function Type Expressions
`(paramName: paramType) => returnType`
&rarr; a function with one parameter, named `paramName`, of type `paramType`, with return type of `returnType`.  
If a parameter type isn't specified, it's implicitly `any`.  
<br/>
> parameter name is **required**.
> `(string) => void` means "a function with a parameter named `string` of type `any`
<br/>
<img width="332" alt="image" src="https://user-images.githubusercontent.com/43084680/177781133-2fc27011-f57e-4510-8be7-80efdf1c5af3.png"><br/>
<img width="310" alt="image" src="https://user-images.githubusercontent.com/43084680/177781408-ba2637e9-4227-4173-8a17-52b3e1dd62cf.png"><br/>
<br/>

### Call Signatures
Functions can be called and have properties at the same time.  
However, the function type expression syntax doesn't allow for declaring properties.  
Instead, we have to use `call signature` in an object type. 

<img width="424" alt="image" src="https://user-images.githubusercontent.com/43084680/177784006-7309190d-daf8-4cc2-98a7-a7c3ee8d12a6.png"><br/>
<br/>

Note! &rarr; use `:` between the parameter list and the return type rather than `=>`.  
<br/>

### Construct Signatures

JavaScript functions can also be invoked with the `new` operator &rarr; *constructor*.  
Created *construct signature* by adding the `new` keyword in ffront of a call signature.  
<img width="289" alt="image" src="https://user-images.githubusercontent.com/43084680/177786030-1c943a86-98d6-48c5-9e83-e300c891a95f.png"><br/>

The object `Date` can be called with or without `new`.  
Call and construct signatures can be combined in the same type arbitrarily.  
<img width="231" alt="image" src="https://user-images.githubusercontent.com/43084680/177786260-ac840b0e-0e78-4976-9a6c-bf54f280b9fb.png"><br/>

### Optional Parameters

Functions in JavaScript can have a varying number of arguments,  
and this can be modeled by defining a `optional parameter` using `?`.  
<img width="279" alt="image" src="https://user-images.githubusercontent.com/43084680/178284080-4d0e05ce-195e-4430-9f74-a7d8711fe621.png"><br/>
<br/>

The variable `l` will have the type `number | undefined` because unspecified parameters in JavaScript have the value `undefined`.  



