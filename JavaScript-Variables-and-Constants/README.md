# JavaScript Variables and Constants

### Variables 

> A variable serves as a storage container for holding data. For instance,
 <!-- >- ভেরিয়েবল গুলো ডেটা রাখার জন্য এক একটি পাত্রের মত কাজ করে। উদাহরণ স্বরূপ, -->
```js
let x = 5;
```
>Here, x is a variable. It's storing 5.

In JavaScript, we use keyword ***var***, ***let*** or ***const*** for declaring variables. Example,
```js
let a;
var b;
const c;
```
Here, a, b and c are variables.

## Var (Before ES6 (ES2015))

- var variables are ***function scoped (Before ES6 (ES2015))***.

**What does this mean?**

> - It means they are only available inside the function they're created in, or if not created inside a function, they are ***globally scoped*** and also part of ***Global Object***.
> - var can be re-declared any where in the code.

```js
var z = 9;
console.log(z); // Expected output: 9
console.log(this.z); // Expected output: 9, this.z works because by declaring a variable with var it became a property of global object.
var z = 5;
function aboutVar(){
  var x = 42;
  var z = 7; 
  console.log(x); // Expected output: 42
  console.log(z); // Expected output: 7
}
aboutVar();
console.log(z); // Expected output: 5
console.log(x); // Error: x is not defined
```

## let (ES6 (ES2015))

- let variables are ***block scoped (ES6 (ES2015))***.

>in JavaScript, the let keyword introduces block-scoped variables. This means that a variable declared using let cannot be re-declared and only accessible within the block of code (enclosed by curly braces **{ }**) in which it is defined.
```js
let x = 1;
let x = 1;// error: Identifier 'x' has already been declared 
```
```js
let z = 9;
if(true){
  let x = 42;
  console.log(x); // Expected output: 42
  console.log(z); // Expected output: 9
}
console.log(z); // Expected output: 9
console.log(x); // Error: x is not defined
 
```

> ***Note: It is recommended we use let instead of var.***

## const(Constants) (ES6 (ES2015))

- Like let declarations, const declarations can only be accessed within the block they were declared.
> The value of a variable declared with const remains the same within its scope. It cannot be updated or re-declared. So if we declare a variable with const, we can neither do this:

```js
const greeting = "say Hi";
greeting = "say Hello instead";// error: Assignment to constant variable. 
```
```js
const greeting = "say Hi";
const greeting = "say Hello instead";// error: Identifier 'greeting' has already been declared 
```
```js
const z = 9;
if(true){
  const x = 42;
  console.log(x); // Expected output: 42
  console.log(z); // Expected output: 9
}
console.log(z); // Expected output: 9
console.log(x); // Error: x is not defined
```

> ***Note: let and const are not part of global object.***

## Questions we might have in our mind?

 ### 1. Why let and const are not part of global object?
  - Historical Context:
     >In JavaScript, var declarations are hoisted to the global scope (if declared outside any function) and become part of the global object (e.g., window in browsers or global in Node.js). This caused issues in larger        codebases, as it increased the risk of name collisions and unintended overwrites.
     
     >To address these problems, let and const were introduced in ES6 (2015) with block scoping and other features that prevent polluting the global object.
  - Block Scope:
    > Variables declared with let and const are block-scoped, meaning they are confined to the block ({}) in which they are defined.
    
    > The global object is a single, shared namespace. If let and const were added to it, they would violate their block-scoping nature.
    ```js
    let a = 10;
    console.log(window.a); // undefined (not part of the global object)
    
    var b = 20;
    console.log(window.b); // 20 (added to the global object)
    ```
   - Temporal Dead Zone (TDZ)
     >let and const have a feature called the ***Temporal Dead Zone (TDZ)***. They are not accessible before their declaration, even if they are part of the same scope.
     
     >If let and const were added to the global object, it would conflict with the TDZ concept, as global variables in the global object are accessible immediately.

    ```js
    console.log(x); // ReferenceError: Cannot access 'x' before initialization
    let x = 5;
    
    // If `let x` were part of the global object, you'd expect `window.x` to exist.
    console.log(window.x); // undefined

    ```
   - Avoiding Global Pollution:
     
     - One of the key motivations behind let and const was to reduce global namespace pollution.
     - Adding them to the global object would contradict this goal, as it would:
     - Increase the risk of overwriting existing global properties.
     - Make debugging and maintenance harder in large projects.
   - Summary of Key Differences
     
| Feature        | 	var                                | 	let / const |
| -------------- | ----------------------------------- | ----------------------------- |
| Scope          | Function or global                  | Block                         |
| Global Object  | Part of the global object           | Not part of the global object |
| Hoisting       | Hoisted, initialized to undefined   | Hoisted but in TDZ            |
| Re-declaration | Allowed                             | Not allowed                   |

> ***Note: By keeping let and const out of the global object, JavaScript enforces cleaner, safer, and more predictable coding practices.😊***

  ### 2. What is ES6 (ES2015)?
   > ES6, also known as ECMAScript 2015 or ES2015, is a major update to the JavaScript language specification. It introduced a wide range of new features, syntax improvements, and APIs that significantly enhanced the 
     capabilities of JavaScript for both browser and server-side development. ES6 was a significant milestone in the evolution of JavaScript, and many of its features have become fundamental to modern JavaScript 
     programming. [Batter explanation is here](https://codeburst.io/javascript-wtf-is-es6-es8-es-2017-ecmascript-dca859e4821c)
  ### 3. What is function scope?
  ### 4. What is Global scope?
  ### 5. What is Block scope?

  > ***Note: Scope will be explained in scope section. Before learning scope we need to know about "[Execution Context](Execution-Context)".***
