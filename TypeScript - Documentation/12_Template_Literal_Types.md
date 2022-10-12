# 



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




