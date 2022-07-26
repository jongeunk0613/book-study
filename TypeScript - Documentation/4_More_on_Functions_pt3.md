# Function Overloads

`Overload Signatures` is used to specify a function that can be **called in different ways** with a **variety of argument counts and types**.  
This is done by writing some number of function signatures followed by the body of the function.  
```JS
function makeDate(timestamp: number): Date;               // Overload signature
function makeDate(m:number, d:number, y:number): Date;    // Overload signature
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {   // Implementation signature
    if (d !== undefined && y !== undefined) {
        return new Date(y, mOrTimestamp, d);
    } else {
        return new Date(mOrTimestamp);
    }
}
const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
const d3 = makeDate(1, 3);
// No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments. 
```
The `implementation signature` has to have a compatible signature and can't be called directly.  
<br/>

### Overload Signatures and the Implementation Signature
Some common errors:
```JS
function fn(x: string): void;
function fn() {
    console.log("DO SOMETHING");
}
fn();
// Expected 1 arguments, but got 0
```
> The signature of the **implementation** is not visible from the outside.  
> When writing an overload function, you hsould always have *two or more* signatures above the implementation of the function. 

The implementation signature must also be **compatible** with the overload signatures.  
The following causes errors because the **implementation signature doesn't match the overloads** in a correct way:
```JS
function fn(x: boolean): void;
// ARgument type isn't right
function fn(x: string): void;
// This overload signature is not compatible with its implementation siganture.
function fn(x: boolean) {}
```
```JS
function fn(x: string): string;
// Return type isn't right
function fn(x: number): boolean;
// This overload signature is not compatible with its implementation signature
function fn(x: string | number) {
    return "oops";
}
```
<br/>

### Writing Good Overloads
> Always prefer parameters with union types instead of overloads when possible.  

Function that returns the length of a string or an array:  
```JS
function len(s: string): number;
function len(arr: any[]): number;
function len(x: any) {
    return x.length;
}
```
This function can be invoked with string or arrays.  
Howevver, it can't be invoked with a value that might be a string **or** an array.  
TypeScript, can only resolve a function call to a single overload.  
```JS
len("");        // OK
len([0]);       // OK
len(Math.random() > 0.5 ? "hello" : [0]);
// No overload matches this call.
//   Overload 1 of 2, '(s: string): number', gave the following error.
//     Argument of type 'number[] | "hello"' is not assignable to parameter of type 'string'.
//       Type 'number[]' is not assignable to type 'string'.
//   Overload 2 of 2, '(arr: any[]): number', gave the following error.
//     Argument of type 'number[] | "hello"' is not assignable to parameter of type 'any[]'.
//       Type 'string' is not assignable to type 'any[]'
```
Because both overloads have the `same argument count and same return type`,  
it can instead be written to a non-overloaded version of the function:  
```JS
function len(x: any[] | string) {
    return x.length;
}
```
<br/>

### Declaring `this` in a function
```JS
const user = {
    id: 123,

    admin: false,
    becomeAdmin: function () {
        this.admin = true;
    }
}
```
TypeScript, understands that `user.becomeAdmin` has a corresponding `this` which is the outer object `user`.  
But this is not all the cases.  
Since **JavaScript specification** states that you **cannot have a parameter called `this`**,  
TypeScript uses that **syntax space** to let you **declare the type for `this`** in the function body. 
```JS
interface DB {
    filterUsers(filter: (this: User) => boolean): User[];
}

const db = getDB();
const admins = db.filterUsers(function (this: User) {
    return this.admin;
});
```
This pattern is common with callback-style APIs &rarr; when another object typically controls when the function is called.  
> Note! use `function` and not arrow functions
```JS
interface DB {
    filterUsers(filter: (this: User) => boolean): User[];
}

const db = getDB();
const admins = db.filterUsers(() => this.admin);
// The containing arrow function captures the global value of 'this'.
// Element implicitly has an 'any' type because type 'typeof globalThis' has no index signature.
```
