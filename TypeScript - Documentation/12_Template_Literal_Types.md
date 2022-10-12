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


## Inference with Template Literals



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




