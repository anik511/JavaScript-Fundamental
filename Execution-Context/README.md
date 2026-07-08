# JavaScript Execution Context

### What is Execution Context?

> An **Execution Context** is the environment in which a piece of JavaScript code is evaluated and executed. Think of it as a "box" or "container" that holds everything JavaScript needs to run your code — which variables exist, what their values are, what `this` refers to, and where to look up things that aren't found locally.
 <!-- >- Execution Context হলো এমন একটি পরিবেশ যেখানে JavaScript কোড রান হয়। এটাকে একটা "বাক্স" এর মত ভাবা যায় যেখানে কোড চালানোর জন্য প্রয়োজনীয় সবকিছু (ভেরিয়েবল, তাদের ভ্যালু, this ইত্যাদি) থাকে। -->

> Every time JavaScript runs code, it does so **inside** an execution context. Nothing runs outside of one.

## Types of Execution Context

There are three types:

1. **Global Execution Context (GEC)**
   > Created by default when your script first runs. There is only **one** global execution context per program. It represents everything *not* inside a function.
   > - It creates a **global object** (`window` in browsers, `global` in Node.js).
   > - It sets `this` to point to that global object (in non-module code).

2. **Function Execution Context (FEC)**
   > Created **every time a function is called** (not when it's defined). Each function call gets its **own brand-new** execution context. Call a function three times → three separate execution contexts are created and destroyed.

3. **Eval Execution Context**
   > Created inside the `eval()` function. This is rarely used and generally avoided, so we won't focus on it.

```js
// Global Execution Context is created here automatically
let name = "Sam";

function greet() {
  // A new Function Execution Context is created every time greet() is called
  let message = "Hello";
  console.log(message + " " + name); // Expected output: Hello Sam
}

greet(); // FEC #1 created, runs, then destroyed
greet(); // FEC #2 created (a fresh one), runs, then destroyed
```

## The Two Phases of an Execution Context

> This is the **most important** idea in this whole topic. Every execution context is created in **two phases**:

### Phase 1 — Creation Phase (also called "Memory Creation Phase")

> Before a single line of your code actually *runs*, JavaScript scans through the code and sets up memory:
> - Variables declared with `var` are put into memory and initialized to `undefined`.
> - Variables declared with `let` and `const` are put into memory but left **uninitialized** (this is the **Temporal Dead Zone / TDZ**).
> - Function declarations are stored **entirely** in memory (the whole function).
> - `this` is determined.

> This phase is *why hoisting exists*. Hoisting isn't the code physically moving — it's just JavaScript setting up memory in this creation phase before running anything.

### Phase 2 — Execution Phase

> Now JavaScript runs your code **line by line**, top to bottom:
> - Assignments actually happen (the `undefined` placeholders get their real values).
> - Functions get called (which each spin up their own new execution context).

### Seeing both phases in action

```js
console.log(a);      // Expected output: undefined  (var was set up in creation phase)
console.log(greet);  // Expected output: [Function: greet]  (whole function stored in creation phase)
// console.log(b);   // ReferenceError: Cannot access 'b' before initialization (let is in TDZ)

var a = 10;
let b = 20;

function greet() {
  return "Hi";
}

console.log(a);      // Expected output: 10  (now the execution phase has assigned it)
```

> Notice `a` is `undefined` (not an error) but `b` throws. That's the difference between `var` (initialized to `undefined` in the creation phase) and `let`/`const` (left in the TDZ). This directly connects back to what we saw in the **Variables and Constants** topic.

## The Call Stack

> JavaScript is **single-threaded** — it can only do one thing at a time. To keep track of which execution context is currently running, it uses a structure called the **Call Stack**.

> - When the script starts, the **Global Execution Context** is pushed onto the stack.
> - Every time a function is called, its **Function Execution Context** is pushed on **top**.
> - When a function finishes (returns), its context is **popped off** the stack.
> - The context on top of the stack is the one currently running.

> A stack is **LIFO** — *Last In, First Out*. The last context pushed is the first one removed.

```js
function first() {
  console.log("Inside first");
  second();               // second() is called from inside first()
  console.log("Back in first");
}

function second() {
  console.log("Inside second");
}

first();

/*
Expected output:
  Inside first
  Inside second
  Back in first
*/
```

**How the Call Stack changes during the run above:**

```
Step 1:  [ Global ]                          ← script starts
Step 2:  [ Global, first ]                    ← first() called
Step 3:  [ Global, first, second ]            ← second() called from inside first()
Step 4:  [ Global, first ]                    ← second() finished, popped off
Step 5:  [ Global ]                           ← first() finished, popped off
Step 6:  [ ]                                   ← script ends, stack empty
```

> ***Note: If functions keep calling each other endlessly (infinite recursion), the stack keeps growing until it overflows — this is the famous `Maximum call stack size exceeded` error.***

```js
function loopForever() {
  loopForever(); // calls itself with no stopping condition
}
loopForever(); // RangeError: Maximum call stack size exceeded
```

## Putting It All Together

> Here's the full lifecycle in one mental model:
> 1. Script runs → **Global Execution Context** is created (creation phase, then execution phase) and pushed onto the call stack.
> 2. A function is called → a **new Function Execution Context** is created (its own creation phase, then execution phase) and pushed on top.
> 3. The function finishes → its context is popped off.
> 4. Repeat until the call stack is empty and the program ends.

## Questions we might have in our mind?

  ### 1. Is a new execution context created when I *define* a function, or when I *call* it?
   > When you **call** it. Defining a function just stores it in memory. The execution context is created only at the moment of invocation — which is why calling the same function 100 times creates 100 separate (short-lived) execution contexts.

  ### 2. How is Execution Context related to Hoisting?
   > Hoisting is simply a **side effect of the creation phase**. Because `var` declarations and function declarations are set up in memory *before* the code runs line by line, they appear to be "moved to the top." Nothing actually moves — the creation phase just registers them first. We'll cover this in detail in the **Hoisting** topic.

  ### 3. How is Execution Context related to Scope?
   > Each execution context has its own environment where its variables live. When code inside one context needs a variable it can't find locally, it looks "outward" to the context it was defined in — this chain of lookups is the **Scope Chain**. So execution context is the foundation that scope is built on. We'll cover this in the **Scope** topic.

  ### 4. What does `this` refer to inside an execution context?
   > It depends on *how* the context was created:
   > - In the **Global Execution Context** (non-module), `this` is the global object (`window` / `global`).
   > - Inside a regular **function call**, `this` depends on how the function was invoked (this has its own dedicated topic).

  ### 5. What is the Temporal Dead Zone (TDZ) again?
   > It's the period between when a `let`/`const` variable is set up in the creation phase and when it's actually assigned a value in the execution phase. During that window, accessing the variable throws a `ReferenceError`. (First introduced in the **Variables and Constants** topic.)

  > ***Note: Now that we understand Execution Context, we're ready to properly understand "[Hoisting](Hoisting)" and then "[Scope](Scope)".***
