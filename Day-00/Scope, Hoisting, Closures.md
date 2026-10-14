
## <font color="#c3d69b">1. Scope</font>

**Scope** is the set of rules that decides where a variable can be accessed. Whenever JS looks up a name, it asks: “Is this variable visible from here?”

#### <font color="#953734">1. Global scope</font>

It is anything declared outside all functions and blocks. It’s visible everywhere.

```js
const appName = "Demo";        // global

function show() {
  console.log(appName);        // works: can see global
}
```

#### <font color="#953734">2. Function Scope</font>

It is a variable declared inside a function that exists only inside it.

```js
function greet() {
  const msg = "Hello";
  console.log(msg);            // works
}
console.log(msg);              // ReferenceError: msg is not defined
```

#### <font color="#953734">3. Block Scope</font>

It applies to anything inside `{ }` (`if`, `for`, `while`, or a bare block). `let` and `const` respect it, but **`var` does not**.

```js
if (true) {
  let a = 1;
  const b = 2;
  var c = 3;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
console.log(c); // 3  (var ignores blocks, only respects functions)
```

This is the main reason modern code prefers `let` and `const` over `var`.

### <font color="#953734"><u>Lexical scope and the scope chain</font>
<font color="#953734"></u></font>

JS uses **lexical (static) scoping**: a function’s scope is decided by where it is _written_ in the code, not where it is _called_.

When you use a variable, JS looks in the current scope first, then the parent scope, then the grandparent, up to global. This is the **scope chain**.

```js
const x = "global";

function outer() {
  const y = "outer";

  function inner() {
    const z = "inner";
    console.log(x, y, z);   // finds z locally, y in outer, x in global
  }
  inner();
}
outer();
```

Lookup only goes **outward**, never inward. `outer` can’t see `z`.

### <font color="#953734"><u>Shadowing</u></font>

An inner variable with the same name hides the outer one:

```js
let name = "outer";

function test() {
  let name = "inner";       // shadows the outer `name`
  console.log(name);        // "inner"
}
test();
console.log(name);          // "outer" (unchanged)
```

# Hoisting in JavaScript

## The core idea

Hoisting is **not** JS physically moving your code to the top. That's a common but misleading explanation.

What actually happens is that before executing a scope, the engine does a **creation phase**: it scans the scope for declarations and registers those names in memory. Then the **execution phase** runs your code line by line.

So by the time line 1 runs, JS already _knows_ every variable and function declared in that scope. The difference is what state each one is in.

## What each declaration does during creation

|Declaration|Registered at creation?|Initial value|Usable before its line?|
|---|---|---|---|
|`function foo() {}`|Yes|The full function|Yes|
|`var x`|Yes|`undefined`|Yes (value is `undefined`)|
|`let x` / `const x`|Yes|_Uninitialized_|No (TDZ error)|
|`class A {}`|Yes|_Uninitialized_|No (TDZ error)|
|Function expression / arrow|Follows its `var`/`let`/`const`||No|

## 1. `var` hoisting

```js
console.log(a); // undefined
var a = 5;
console.log(a); // 5
```

The engine treats it like this:

```js
var a;              // creation phase: declared, set to undefined
console.log(a);     // undefined
a = 5;              // execution phase: assignment stays in place
console.log(a);     // 5
```

**Only the declaration is hoisted, never the assignment.**

## 2. Function declaration hoisting

Function declarations are hoisted _with their body_:

```js
add(2, 3);                  // 5, works before the definition

function add(a, b) {
  return a + b;
}
```

This is why you can put helper functions at the bottom of a file and call them from the top.

## 3. Function expressions are not hoisted like declarations

```js
greet();                    // TypeError: greet is not a function

var greet = function () {
  console.log("hi");
};
```

Here `var greet` is hoisted as `undefined`, so calling it is like calling `undefined()`. That gives a **TypeError**, not a ReferenceError.

With `let` or `const` (including arrow functions) you get a **ReferenceError** instead, because of the TDZ:

```js
greet();                    // ReferenceError
const greet = () => console.log("hi");
```

## 4. `let` and `const`: hoisted, but in the TDZ

A common myth is that `let` and `const` are _not_ hoisted. They are. The proof is the shadowing behavior:

```js
let x = "outer";

function test() {
  console.log(x);           // ReferenceError, not "outer"!
  let x = "inner";
}
test();
```

If `let x` weren't hoisted, the `console.log` would find the outer `x` and print `"outer"`. Instead, the inner `x` is already registered for the whole function scope, so it shadows the outer one, but it's **uninitialized** until execution reaches its line. That gap is the **Temporal Dead Zone**:

```js
{
  // ---- TDZ for `y` starts here ----
  console.log(y);           // ReferenceError
  // ---- TDZ ends at the declaration ----
  let y = 10;
  console.log(y);           // 10
}
```

The TDZ is about **time** (execution order), not position in the file. This works fine:

```js
function show() { console.log(z); }
let z = 5;
show();                     // 5, because z is initialized before show runs
```

Even `typeof` isn't safe inside the TDZ:

```js
typeof undeclared;          // "undefined"
typeof q;                   // ReferenceError
let q;
```

## 5. Answer to the quick test from last time

```js
let x = 10;

function foo() {
  console.log(x);   // ReferenceError: Cannot access 'x' before initialization
  let x = 20;
}
foo();
```

The inner `let x` is hoisted to the top of `foo`'s scope and shadows the outer `x`. At `console.log(x)` it's still in the TDZ, so JS throws instead of falling back to the outer `x = 10`.

## 6. Priority when names collide

Function declarations win over `var` during creation, but the `var` assignment can overwrite them at runtime:

```js
console.log(typeof thing);  // "function"

var thing = "I'm a string now";
function thing() {}

console.log(typeof thing);  // "string"
```

Creation phase: `thing` becomes the function. Execution phase: `var thing = "..."` reassigns it. You'll rarely write this on purpose, but it's a favorite interview question.

## 7. Hoisting is per scope

Each scope hoists to **its own** top, not the file's top:

```js
function demo() {
  console.log(v);           // undefined (hoisted to top of demo, not global)
  var v = 1;
}
demo();
console.log(v);             // ReferenceError: v only exists inside demo
```

And `var` ignores block boundaries, so it hoists to the enclosing **function**:

```js
function f() {
  console.log(i);           // undefined
  if (true) {
    var i = 1;
  }
  console.log(i);           // 1
}
```

## Closures


**Definition:** A closure is a function that remembers the variables from the scope where it was created, even after that scope has finished running.

**How it works:**

- Functions can see variables from where they are _written_ (lexical scope).
- When you return an inner function, it carries a hidden link to its birthplace's variables (the "backpack").
- JavaScript only deletes a variable when nothing can reach it, and the inner function can still reach it, so the variable stays alive.

```js
function outer() {
  let message = "hi";
  return function () { console.log(message); };
}
const fn = outer();
fn(); // "hi", even though outer() already finished
```

**Key points:**

1. **Each call makes a fresh copy.** Two counters from `makeCounter()` have separate `count` variables.
2. **Private state.** Nothing outside can touch the captured variable except through the returned function.
3. **Captures variables, not values.** The closure sees the variable's _current_ value when it runs, not a snapshot.
4. **Loop trap.** `var` gives one shared `i` (prints `3 3 3`), while `let` gives a new `i` per iteration (prints `0 1 2`).

## Earlier topic: hoisting and the TDZ

- All declarations are hoisted, including `let` and `const`.
- `var` is hoisted and initialized to `undefined`, so the hoisting is visible.
- `let`/`const` are hoisted but uninitialized until their line runs. That gap is the TDZ, and touching them early throws a `ReferenceError`.

**One-line version of each:**

- Closure: a function plus a live link to the variables around where it was born.
- TDZ: the name exists from the start of the scope, but it has no value until its declaration line runs.

