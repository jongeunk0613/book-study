# Everyday Types

## Type Aliases
Instead of repeating the same custom types, name them through `type alias`; refer to a type by a single name and use it more than once.  

```
type Point = {
  x: number;
  y: number;
};

// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100});
```
