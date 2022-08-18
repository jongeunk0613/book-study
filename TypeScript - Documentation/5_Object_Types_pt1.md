# Object Types

### Property Modifiers

Properties in an object can specify the **type**, whether the property is **optional**, and whether the property **can be written** to.  
<br/>

#### Optional Properties
Mark *optional* properties by adding a question mark `?` to the end of their names.  

```JS
interface PaintOptions {
    shape: Shape;
    xPos?: number;
    yPos?: number;
}

function paintShape(opts: PaintOptions) {
    // ...
}

const shape = getShape();
paintShape({ shape });
paintShape({ shape, xPos: 100 });
paintShape({ shape, yPos: 100 });
paintShape({ shape, xPos: 100, yPos: 100 });
```

<br/>

What optionality really says is that *if* the property *is set*, it better have a specific type.  

Optional properties can be read.  
But with `strictNullChecks`, TypeScript tell us they're potentially `undefined`.  

```JS
function paintShape(opts: PaintOptions) {
    // (property) PaintOptions.xPos?: number | undefined
    let xPos = opts.xPos;
    let yPos = opts.yPos;
    // ....
}
```

<br/>

In JavaScript, when we access an optional property that is not set, we get the value `undefined`.  
We can just handle `undefined` specially.  
```JS
function paintShape(opts: PaintOptions) {
    // let xPos: number 
    let xPos = opts.xPos === undefined ? 0 : opts.xPos;
    let yPos = opts.yPos === undefined ? 0 : opts.yPos;
    // ....
}
```
<br/>

This pattern of setting defaults for unspecified values is so common that JavaScript has syntax to support it.  

```JS
function paintShape({shape, xPos = 0, yPos = 0}: PaintOptions ){
    console.log("x coordinate at ", xPos);
    console.log("y coordinate at ", yPos);
    // ....
}
```

With this syntax, default values are provided for `xPos` and `yPos`.  
They will be present within the body of `paintShape`, but optional for any callers to `paintShape`.  
<br/>

> Note: there is currently no way to place type annonations within destructuring patterns, because the following syntax already means smoething different in JavaScript. 

```JS
function draw({shape: Shape, xPos: number = 100 /*...*/ }) {
    render(shape);
    // Cannot find name 'shape'. Did you mean 'Shape'?
    render(xPos);
    // Cannot find name 'xPos'.
}
```

In an object destructuring pattern, `shape: Shape` means "grab the property `shape` and redefine it locally as a variable named `Shape`".  
Likewise `xPost: number` creates a variable named `number` whose value is based on the parameter's `xPos`.  

