# Generic Object Types

Use *type parameter* to create generic types.  
```JS
interface Box<Type> {
    contents: Type;
}

```
&rarr; A `Box` of `Type` is something whose `contents` have type `Type`.  

`Box` is like a template for a real type ; `Type` is a placeholder that will get replaced with some other type.  

```JS

let box: Box<string> = {contents: "hello"};
box.contents;   // (property) Box<string>.contents: string

```

`Box` is reusable.  `Type` can be substituted with anything and for a new type, a new type of `Box` is not needed.  

```JS
interface Box<Type> {
    contents: Type;
}

interface Apple {
    count: number
}

// Same as '{contents: Apple}'
type AppleBox = Box<Apple>;
```

We can avoid overloads entirely by using generic functions.  
```JS
interface NumberBox {
  contents: number;
}
 
interface StringBox {
  contents: string;
}
 
interface BooleanBox {
  contents: boolean;
}

function setContents2(box: StringBox, newContents: string): void;
function setContents2(box: NumberBox, newContents: number): void;
function setContents2(box: BooleanBox, newContents: boolean): void;
function setContents2(box: { contents: any }, newContents: any) {
  box.contents = newContents;
}

/* - - - - - - - - */

interface Box<Type> {
    contents: Type;
}

function setContents<Type> (box: Box<Type>, newContents: Type) {
    box.contents = newContents;
}
``` 

Type aliases can also be generic.  
```JS
type Box<Type> = {
  contents: Type;
}
```

Since type aliases can describe more than just object types,  
it can be used to write other kinds of generic helper types.  

```JS
type OrNull<Type> = Type | null;

type OneOrMany<Type> = Type | Type[];

type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;
// type OneOrManyOrNull<Type> = OneOrMany<Type> | null

type OneOrManyOrNullStrings = OneOrManyOrNull<string>;
// type OneOrManyOrNullStrings = OneOrMany<string> | null
```

<br/>

# The `Array` Type





