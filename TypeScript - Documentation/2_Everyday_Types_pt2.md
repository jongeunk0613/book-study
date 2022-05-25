# Everyday Types

## Type Aliases
Instead of repeating the same custom types, name them through `type alias`; refer to a type by a single name and use it more than once.  

<img width="418" alt="image" src="https://user-images.githubusercontent.com/43084680/170033332-a39a0712-0d77-4d13-88c8-a18631678b29.png">
Since they are only aliases, a type aliases cannot have different "versions" of the same type.  
<br/>

## Interfaces
Another way to name an object type is an `interface declaration`.  

<img width="413" alt="image" src="https://user-images.githubusercontent.com/43084680/170033416-4e7e8432-0aea-4710-9393-6353f97bc81d.png" />
The example above works just as if an anonymous object type was used.<br/>
In this case, TypeScript is only concerned with the *structure* of the value passed to `printCoord` - it only cares that it has the expected properties &rarr; `structually typed type system`.  
<br/>

## Interface vs Type

<table>
	<tr>
		<th> Adding new fields to an existing interface<br/>Declaration merging possible </th>
		<th> Type cannot be changed after being created<br/>No declaraction merging </th>
	</tr>
	<tr>
		<td><img src="https://user-images.githubusercontent.com/43084680/170036275-fe7bcc27-fbee-4cdf-8be4-54eda4a52dd0.png"></td>
		<td><img src="https://user-images.githubusercontent.com/43084680/170036545-d555d330-c238-4f83-96de-5268711018b8.png"></td>
	</tr>
</table>
<br/>

<table>
	<tr>
		<th> Extending an interface </th>
		<th> Extending a type via intersections </th>
	</tr>
	<tr>
		<td><img src="https://user-images.githubusercontent.com/43084680/170035134-ccf0a0b1-e840-40c3-8f98-512666fe502a.png"></td>
		<td><img src="https://user-images.githubusercontent.com/43084680/170035012-bc593640-6207-48ad-bd04-33ec0834ddd4.png"></td>
	</tr>
</table>
<br/>

<table>
	<tr>
		<th> Only used to declare the shapes of objects<br/>No renaming primitives </th>
		<th> Can rename primitives </th>
	</tr>
	<tr>
		<td><img src="https://user-images.githubusercontent.com/43084680/170037124-52866af3-21d1-4e06-842f-ddbc9f85f5a7.png"></td>
		<td><img src="https://user-images.githubusercontent.com/43084680/170037286-7129b25f-97ae-44c7-8623-6055d6884edc.png"></td>
	</tr>
</table>
<br/>

<table>
	<tr>
		<th> Always appear in their original form in error messages<br/>but only when they are used by name </th>
		<th> The name of the type is not mentioned </th>
	</tr>
	<tr>
		<td><img src="https://user-images.githubusercontent.com/43084680/170038461-d9409f92-cfe7-4c09-ae98-834ed97e6b37.png"><img src="https://user-images.githubusercontent.com/43084680/170038612-58b604f8-65b1-48a0-b6ae-5474932c7a24.png"></td>
		<td><img src="https://user-images.githubusercontent.com/43084680/170042226-afeb8177-88dc-4e16-83f0-5f3c938f2e86.png"><img src="https://user-images.githubusercontent.com/43084680/170042344-322eb5d9-f655-4cab-88f7-c71ca29533d3.png"></td>
	</tr>
</table>
<br/>

## Type Assertions
Use type assertions to `specify and certify` that a value is of a certain specific type.  
<img width="577" alt="image" src="https://user-images.githubusercontent.com/43084680/170042867-7a17f873-014c-44b3-bf0e-18a82bdba499.png">
<br/>

Because type assertioes are `removed at compile-time`, there is `no runtime checking` associated with a type assertion.  
There won't be an exception or null generated if the type assertion is wrong.  
<img width="292" alt="image" src="https://user-images.githubusercontent.com/43084680/170043543-4021a00c-a6be-4c16-9add-814e37230c01.png">  
In JavaScript:  
<img width="211" alt="image" src="https://user-images.githubusercontent.com/43084680/170043900-15b7bd55-863f-41e3-995c-42b666d052ed.png">
<br/>

TypeScript only allows type assertions which convert to a *more specific or less specific* version of a type.  
**Impossible** coercions are prevented.  
<img width="914" alt="image" src="https://user-images.githubusercontent.com/43084680/170044233-70463deb-7998-4d05-b9e5-954c359246c3.png">
<br/>

This `prevents complex coercions` that might be valid.  
In that case, use two assertions as follow:  
<img width="281" alt="image" src="https://user-images.githubusercontent.com/43084680/170044761-7d477190-e8db-4b65-be5a-0da5c9e512bb.png">
<br/>

## Literal Types
In addition to the general types `string` and `number`, we can refer to *specific* strings and numbers in type positions.  
Usually, literal types are used on `const` variable declaration; `const` variables' values doesn't change.  
<img width="463" alt="image" src="https://user-images.githubusercontent.com/43084680/170045271-2678c32c-5fe3-4cd7-9aa3-2d5442938a4a.png">
<img width="480" alt="image" src="https://user-images.githubusercontent.com/43084680/170045448-8e027e2f-5df2-46cc-83e2-5f69a4e3cfc6.png">
<br/>

By `combining literals into unions`, a function that `only accepts certain set of known values` can be created.  
<img width="980" alt="image" src="https://user-images.githubusercontent.com/43084680/170047966-8a2cbaff-0c7c-47e9-b4dd-7fbb918f44d4.png"><br/>
<img width="396" alt="image" src="https://user-images.githubusercontent.com/43084680/170048126-a90d012d-85b2-4b39-a479-b0f68617a5b5.png"><br/>
<img width="815" alt="image" src="https://user-images.githubusercontent.com/43084680/170048397-5028575e-b792-4bdb-be15-726511c8817a.png">
<br/>
> `Boolean` literals are also a kind of literal type.  
> It is an alias for the union `true | false`.  

## Literal Inference
TypeScript assumes that the properties of an object might change values later.  
Therefore, instead of infering the type as a single literal type, if inferes its general type based on the literal.  
<img width="406" alt="image" src="https://user-images.githubusercontent.com/43084680/170049198-eeb0e752-c8e5-40cb-9af8-604530c4fbe9.png"><br/>
<img width="769" alt="image" src="https://user-images.githubusercontent.com/43084680/170049834-4b53f709-9e2f-4107-bf16-d31d97fd5d9e.png">
<br/>

To solve the case of `req.method`:
1. Change the inferences by adding a type assertion in either location  
<img width="505" alt="image" src="https://user-images.githubusercontent.com/43084680/170050099-b0a459dd-3d75-4a4a-96ba-262148af06d5.png"><br/>
2. Use `as const` to convert the entire object to be type literals  
<img width="493" alt="image" src="https://user-images.githubusercontent.com/43084680/170050344-522d30fc-98e5-499f-ac11-9d21bc7069e3.png"><br/>

## `null` and `undefined`
`null` refers to `absent`, and `undefined` refers to `uninitialized`.  
They behave differently depending on the `strictNullChecks` option.  
<br/>

### `strictNullChecks` off
Values that might be `null` or `undefined` can still be accessed normally and be assigned to a property of any type.  
No null checks will be done.  
<img width="361" alt="image" src="https://user-images.githubusercontent.com/43084680/170051374-9f802dfe-07b3-413d-ae38-7ab408927e78.png">
<br/>

### `strictNullChecks` on
When a value is `null` or `undefined`, testing for those values before using methods or properties on that value will be neccessary.  
Just like checking for `undefined` before using an optional property, *narrowing* can be used to check for values that might be `null`.  
<img width="505" alt="image" src="https://user-images.githubusercontent.com/43084680/170051517-cb2456a6-2688-4093-951e-366a68ccac5c.png"><br/>
<img width="472" alt="image" src="https://user-images.githubusercontent.com/43084680/170051963-dd4d5f30-fbca-404c-b74c-2bffe7cc4820.png"><br/>
<img width="388" alt="image" src="https://user-images.githubusercontent.com/43084680/170052301-95bf71e6-b374-461e-ab01-b25ffda79023.png">
<br/>

### Non-null Assertion Operator (Postfix `!`)
Removes `null` and `undefined` from a type without doing any explicit checking &rarr; type assertion that the value isn't `null` or `undefined`.  
<img width="339" alt="image" src="https://user-images.githubusercontent.com/43084680/170053522-44e7beb4-1fdd-4dad-bc1d-418a40b2a0b5.png">

Just like other type assertions, this `doesn't change the runtime behavior` of your code, so it's important to only use `!` when you know that the value *can't* be `null` or `undefined`.  
<br/>

> `?` vs `!`
> `?` checks whether the value is defined and it not, returns `undefined`.
> `!` assumes that the value is surely not `null` nor `undefined`.
<img width="500" alt="image" src="https://user-images.githubusercontent.com/43084680/170054332-708eeb73-c292-4026-88b6-a517f4b3b289.png"><br/>
<img width="335" alt="image" src="https://user-images.githubusercontent.com/43084680/170054535-c5747b59-9baa-4021-acf6-8c463baa7169.png">
<br/>

## Enums
Enums are a feature added to JavaScript by TypeScript which allows for describing a value which could be `one of a set of possible named constants`.  
Unlike most TypeScript features, this is `not a type-level addition` to JavaScript but something `added to the language and runtime`.  
<img width="386" alt="image" src="https://user-images.githubusercontent.com/43084680/170055944-acf81cef-dd03-4909-bd5e-b73fe9ac69f4.png">
<br/>

## Less Common Primitives
## bigint
`BigInt` is used for very large integers.  
Included after ES2020.  
<img width="336" alt="image" src="https://user-images.githubusercontent.com/43084680/170056424-95a93018-b617-49f7-9799-5a6dbdf498c4.png">
<br/>

## symbol
`Symbol` is used to create a globally unique reference via the function `Symbol()`
<img width="864" alt="image" src="https://user-images.githubusercontent.com/43084680/170056738-c09a8123-631e-4e14-9b67-e29debcd6a50.png">






