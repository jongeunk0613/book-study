# Narrowing

## Control Flow Analysis
The analysis of code based on reachability is called **control flow analysis**.  
<img width="446" alt="image" src="https://user-images.githubusercontent.com/43084680/174823195-cfaaa2e1-7c6e-4cf7-a024-0ba917acd433.png"><br/>
The type `number` is removed from `padding` after the `if` block,  
because TypeScript knows that it is unreachable in the case where `padding` is a `number`.  
<br/>

TypeScript uses this flow analysis **to narrow types** as it encoutners type guards and assignments.  
<img width="416" alt="image" src="https://user-images.githubusercontent.com/43084680/174824867-249ef3bb-7f4a-4051-ad89-d2a393e957de.png"><br/>
<br/>

## Using Type predicates
