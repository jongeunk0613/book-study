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
<img width="246" alt="image" src="https://user-images.githubusercontent.com/43084680/178488357-1a18a32a-4345-4611-b941-41822b5dc781.png"><br/>
<br/>

The variable `n` will have the type `number | undefined` because unspecified parameters in JavaScript have the value `undefined`.  
<img width="446" alt="image" src="https://user-images.githubusercontent.com/43084680/178488275-63af0e16-d152-4a68-b4d1-e04d02af7406.png"><br/>
<br/>

This can be solved by providing a `default` parameter.  
<img width="246" alt="image" src="https://user-images.githubusercontent.com/43084680/178489446-1141fcbe-b9bb-4e9e-a9dd-49bf5e339bcc.png"><br/>
Now `n` will have type `number` because any `undefined` argument will be replaced with the default value.  
`undefined` parameters are considered as `missing` arguments.  
<br/>

### Optional Parameters in Callbacks

We might write a callback function with optional parameters so that we could make calls with a variable number of arguments.  
<img width="718" alt="image" src="https://user-images.githubusercontent.com/43084680/178495957-1034b209-2feb-4399-ab29-a27de3af5cef.png"><br/>

But because the `index` parameter is optional, the callback can be called with a single argument.  
<img width="591" alt="image" src="https://user-images.githubusercontent.com/43084680/178500320-b0caf240-778e-4804-b9de-e96e97c87440.png"><br/>

Due to this, the following error is caused.  
<img width="638" alt="image" src="https://user-images.githubusercontent.com/43084680/178496585-b677ea8a-aaf0-43de-8b62-4c89abc0f01b.png">

In JavaScript, if you call a function with more arguments than there are parameters,  
the extra arguments are simply ignored.  
TypeScript behaves in the same way.  
<img width="977" alt="image" src="https://user-images.githubusercontent.com/43084680/178502164-e012f1cd-7be7-4fce-aaa1-c0ddd9ff196d.png"><br/>

Therefore, functions with fewer parameters (of the same types) can always take the place of functions with more parameters.  
<img width="857" alt="image" src="https://user-images.githubusercontent.com/43084680/178507790-961608b9-f687-4baf-8552-aa9dd128dda1.png"><br/>

> When writing a callback function, `never` write an optional parameter unless you intend to call the function `without passing` that argument.  





