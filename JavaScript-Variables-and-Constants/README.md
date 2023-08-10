### JavaScript Variables and Constants

## Variables 

>A variable serves as a storage container for holding data. For instance,
```js
let x = 5;
```
>Here, x is a variable. It's storing 5.

In JavaScript, we use keyword ***var*** or ***let*** for declaring variables. Example,
```js
let a;
var b;
```
Here, a and b are variables.

## Var (Before ES6 (ES2015))

- var variables are ***function scoped (Before ES6 (ES2015))***.

**What does this mean?**

>It means they are only available inside the function they're created in, or if not created inside a function, they are globally scoped.
```js
var z = 9;
console.log(z); // Expected output: 9
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

***Note: It is recommended we use let instead of var.***

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