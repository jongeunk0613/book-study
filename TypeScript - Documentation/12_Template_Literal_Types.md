# Template Literal Types

Template literal types build on string literal types.  
They have the ability to expand into many strings via unions.  

They have the same syntax as template literal strings in JavaScript,  
but are used in type positions.  
<br/>

When used with concrete literal types, a template literal produces a new string literal type by concatenating the contents.  

```ts
type World = "world";
 
type Greeting = `hello ${World}`;
// type Greeting = "hello world"
```

When a union is used in the interpolated position,  
the type is the set of every possible string literal that could be represented by each union member.  

```ts
type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
 
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
// type AllLocaleIDs = "welcome_email_id" | "email_heading_id" | "footer_title_id" | "footer_sendoff_id"
```

For each interpolated position in the template literal, the unions are cross multiplied.  

```ts
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
type Lang = "en" | "ja" | "pt";
 
type LocaleMessageIDs = `${Lang}_${AllLocaleIDs}`;
// type LocaleMessageIDs = "en_welcome_email_id" | "en_email_heading_id" | "en_footer_title_id" | 
//                         "en_footer_sendoff_id" | "ja_welcome_email_id" | "ja_email_heading_id" | 
//                         "ja_footer_title_id" | "ja_footer_sendoff_id" | "pt_welcome_email_id" | 
//                         "pt_email_heading_id" | "pt_footer_title_id" | "pt_footer_sendoff_id"
```
<br/>

## String Unions in Types

The power in template literals comes when defining a new string based on information inside a type.

ex: 
- function `makeWatchedObject` adds a new function called `on()` to the passed object.  
- function `on` expects two arguments: an `eventName` (string) and a `callBack` (function).  
- `eventName` should be of the form `attributeInThePassedObject` + `"Changed"`.  
- function `callback`, when called should be passed a value of the type associated with the name `attributeInThePassedObject`.  

<br/>

```ts
const passedObject = {
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26,
};
```

- `eventName` for the property `firstName` will be `firstNameChanged`.  
- when calling the callback for `firstNameChanged`, a `string` will be passed.  
- when calling the callback for `ageChanged`, a `number` will be passed.  

The naive function signature of `on()` might be: `on(eventName: string, callBack: (newValue: any) => void)`.  

However some type constraints is missing and this can be solved with template literal types.  

<br/>
For example, it would be better to ensure that the set of eligible event names was constrained  <br/>
by the union of attribute names in the watched object with "Changed" added at the end.  

```ts
type PropEventSource<Type> = {
    on(eventName: `${string & keyof Type}Changed`, callback: (newValue: any) => void): void;
};
 
/// Create a "watched object" with an 'on' method
/// so that you can watch for changes to properties.
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
```

With this, we can build something that errors when given the wrong property:

```ts
const person = makeWatchedObject({
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26
});
 
person.on("firstNameChanged", () => {});
 
// Prevent easy human error (using the key instead of the event name)
person.on("firstName", () => {});
// Argument of type '"firstName"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.
 
// It's typo-resistant
person.on("frstNameChanged", () => {});
// Argument of type '"frstNameChanged"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.
```

<br/>

## Inference with Template Literals

Still some constraint is missing.  
Given a `firstNameChanged` event, the callback should expect to receive an argument of type `string`.  
Equally, the callback for a `ageChanged` event should receive a `number` argument.  
Instead of naivaly using `any` to type the callBack's argument, template literal types make it possible to  
ensure an attribute's data type will be the same type as that attribute's callback's first argument.  

<br/>

This is possible by using a function with a generic such that:  
- the literal used in the first argument is captured as a literal type
- that literal type can be validated as being in the union of valid attributes in the generic
- the type of the validated attribute can be looked up in the generic's structure using Indexed Access
- this typing information can then be applied to ensure the argument to the callback function is of the same type

```ts
type PropEventSource<Type> = {
    on<Key extends string & keyof Type>
        (eventName: `${Key}Changed`, callback: (newValue: Type[Key]) => void ): void;
};
 
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
 
const person = makeWatchedObject({
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26
});
 
person.on("firstNameChanged", newName => {      // (parameter) newName: string
    console.log(`new name is ${newName.toUpperCase()}`);
});
 
person.on("ageChanged", newAge => {       // (parameter) newAge: number
    if (newAge < 0) {
        console.warn("warning! negative age");
    }
})
```

Here, `on` is made into a generic method.  

When a user calls with the string `"firstNameChanged"`, TypeScript will try to infer the right type for `Key`.  
To do that, it will match `Key` against the content prior to `"Changed"` and infer the string `"firstName"`.  
Once TypeScript figures that out, the `on` method can fetch the type of `firstName` on the original object, which is `string` in this case.  
Similarly, when called with `"ageChanged"`, TypeScript finds the type for the property `age` which is `number`.  
 
Inference can be combined in different ways, often to deconstruct strings, and reconstruct them in different ways.  

<br/>

## Intrinsic String Manipulation Types

TypeScript includes a set of types which can be used in string manipulation.  
These types come built-in to the compiler for performance and can't be found in the `.d.ts` files included with TypeScript.  

<br/>

### `Uppercase<StringType>`

Converts each character in the string to the uppercase version.  

```ts
type Greeting = "Hello, world"
type ShoutyGreeting = Uppercase<Greeting>
// type ShoutyGreeting = "HELLO, WORLD"
 
type ASCIICacheKey<Str extends string> = `ID-${Uppercase<Str>}`
type MainID = ASCIICacheKey<"my_app">
// type MainID = "ID-MY_APP"
```

<br/>

### `Lowercase<StringType>`

Converts each character in the string to the lowercase equivalent.  

```ts
type Greeting = "Hello, world"
type QuietGreeting = Lowercase<Greeting>
// type QuietGreeting = "hello, world"
 
type ASCIICacheKey<Str extends string> = `id-${Lowercase<Str>}`
type MainID = ASCIICacheKey<"MY_APP">
// type MainID = "id-my_app"
```

<br/>

### `Capitalize<StringType>`

Converts the first character in the string to an uppercase equivalent.  

```ts
type LowercaseGreeting = "hello, world";
type Greeting = Capitalize<LowercaseGreeting>;
// type Greeting = "Hello, world"
```

<br/>

### `Uncapitalize<StringType>`

Converts the first character in the string to a lowercase equivalent.  

```ts
type UppercaseGreeting = "HELLO WORLD";
type UncomfortableGreeting = Uncapitalize<UppercaseGreeting>;
// type UncomfortableGreeting = "hELLO WORLD"
```




