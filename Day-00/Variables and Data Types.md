### <font color="#9bbb59"><u>Declarations</u></font>

- <font color="#c0504d"><u>var</u>:</font> 
    var is used when a variable is initialised globally. It doesn't care about block. Once the value is changed, it is changed regardless of block.

```js
var x = 1;

if (x === 1) {
  var x = 2;

  console.log(x);
  // Expected output: 2
}

console.log(x);
// Expected output: 2

```



- <font color="#953734"><u>let:</u></font> 
    let is used when we want to basically change or redeclare the local variable in a block of code. 

```js
let x = 1;

if (x === 1) {
  let x = 2;

  console.log(x);
  // Expected output: 2
}

console.log(x);
// Expected output: 1

```

- <font color="#c0504d"><u>const:</u></font> 
    The **`const`** declaration declares block-scoped local variables. The value of a constant can't be changed through reassignment using the [assignment operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Assignment), but if a constant is an [object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures#objects), its properties can be added, updated, or removed.

```js
let x = 2;
const y = 3;

console.log(x);
// Expected output: 2

console.log((x = y + 1)); // 3 + 1
// Expected output: 4

console.log((y = x * y)); // 4 * 3

```


<font color="#953734"><u>Output</u></font>

```
> 2
> 4
> TypeError: Assignment to constant variable.

```



### <font color="#9bbb59"><u>Data Types</u></font>

### <u>Part 1: Primitive Types</u>:
Primitives are **immutable** (they can’t be changed in place) and are compared and copied **by value**.

#### 1. <font color="#953734">String:</font> Text, in single quotes, double quotes, or backticks.

```js
let a = 'Kolkata';
let b = "Salt Lake";
let c = `Hello ${a}`;      // template literal -> "Hello Kolkata"

a.length;                   // 7
a[0];                       // "K"
a.toUpperCase();            // "KOLKATA" (returns a NEW string)
a[0] = 'X';                 // silently ignored: strings are immutable
```


#### 2. <font color="#953734">Number:</font> One type for both integers and decimals (64-bit floating point, IEEE 754).


```js
let age = 20;
let pi = 3.14;
let big = 2e6;              // 2000000

Number.MAX_SAFE_INTEGER;    // 9007199254740991 (2^53 - 1)
1 / 0;                      // Infinity
-1 / 0;                     // -Infinity
"abc" * 2;                  // NaN (Not-a-Number)

0.1 + 0.2;                  // 0.30000000000000004  (floating point!)
0.1 + 0.2 === 0.3;          // false
```

#### 3. <font color="#953734">BigInt :</font> For integers beyond the safe range. Add `n` at the end.

```js
let huge = 123456789012345678901234567890n;
typeof huge;                // "bigint"

huge * 2n;                  // OK
huge * 2;                   // TypeError: can't mix BigInt and Number
```

#### 4. <font color="#953734">Boolean :</font> `true` or `false`.

```js
let isStudent = true;
let isPassed = 5 > 10;      // false
```

#### 5. <font color="#953734">Undefined :</font> **`undefined`** is a [primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) value automatically assigned to [variables](https://developer.mozilla.org/en-US/docs/Glossary/Variable) that have just been declared, or to formal [arguments](https://developer.mozilla.org/en-US/docs/Glossary/Argument) for which there are no actual arguments.

```js
let x; // create a variable but assign it no value

console.log(`x's value is ${x}`); // logs "x's value is undefined"
```

#### 6. <font color="#953734">null :</font> A special keyword denoting a null value. (Because JavaScript is case-sensitive, `null` is not the same as `Null`, `NULL`, or any other variant.)

```js
console.log(typeof null); // "object"
```

