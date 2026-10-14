## 1. Template literals

Template literals use backticks (`` ` ``) instead of single or double quotes. They let you insert variables or expressions using `${...}`.

```js
const name = "Rahul";
console.log(`Hello, ${name}!`); // Hello, Rahul!
```

You can put expressions inside `${...}` too:

```js
const a = 5;
const b = 3;

console.log(`${a} + ${b} = ${a + b}`); // 5 + 3 = 8
```

**Remember:** use backticks around the whole template literal and `${...}` around the value or expression to insert.

---

## 2. Destructuring

Destructuring pulls values out of arrays or properties out of objects and assigns them to variables.

### Array destructuring

Array destructuring uses square brackets `[]`. Values are assigned by position.

```js
const numbers = [10, 20, 30];

const [first, second] = numbers;

console.log(first);  // 10
console.log(second); // 20
```

You can skip an element by leaving its position empty:

```js
const numbers = [10, 20, 30];

const [first, , third] = numbers;

console.log(first); // 10
console.log(third); // 30
```

### Object destructuring

Object destructuring uses curly braces `{}`. Variables match the object's property names.

```js
const student = {
    name: "Rahul",
    marks: 90
};

const { marks } = student;

console.log(marks); // 90
```

Here, `marks` is the property name being extracted. The right-hand side is the object, `student`.

**Key difference:**

- Array destructuring: matches by **position**.
- Object destructuring: matches by **property name**.

---

## 3. Spread syntax (`...`)

Spread expands the elements of an array, or the properties of an object, into another array or object.

### Array spread

```js
const a = [10, 20];
const b = [0, ...a, 30];

console.log(b); // [0, 10, 20, 30]
```

`...a` expands the elements of `a` into the new array.

### Copying an array

```js
const a = [3, 6];
const b = [...a];

b.push(9);

console.log(a); // [3, 6]
console.log(b); // [3, 6, 9]
```

`[...a]` creates a new, shallow array. Adding a primitive value to `b` does not add it to `a`.

Compare that with `[a]`:

```js
const a = [3, 6];
const c = [a];

console.log(c);       // [[3, 6]]
console.log(c[0]);    // [3, 6]
console.log(c[0][1]); // 6
```

`[a]` puts the whole array `a` inside another array, creating a nested array. It does not spread `a`'s elements.

### Object spread

```js
const student = {
    name: "Rahul",
    marks: 90
};

const updated = { ...student, age: 20 };

console.log(updated); // { name: "Rahul", marks: 90, age: 20 }
```

`...student` copies the object's enumerable properties into the new object.

If a property appears more than once, the later value wins:

```js
const a = { score: 40 };
const b = { ...a, score: 60 };

console.log(b.score); // 60
```

Because `score: 60` appears after `...a`, it overrides the copied value.

---

## 4. Rest syntax (`...`)

Rest collects remaining values into one variable. It uses the same `...` symbol as spread, but its job is different.

### Array destructuring with rest

```js
const numbers = [10, 20, 30, 40, 50];

const [first, second, ...rest] = numbers;

console.log(first);  // 10
console.log(second); // 20
console.log(rest);   // [30, 40, 50]
```

How it works:

- `first` receives the first element: `10`.
- `second` receives the second element: `20`.
- `rest` collects all remaining elements into a new array: `[30, 40, 50]`.

### Spread vs. rest

```js
const a = [10, 20];

const copied = [...a]; // Spread: expands elements into a new array

const [first, ...remaining] = a; // Rest: collects remaining elements
```

- **Spread:** expands elements or properties outward.
- **Rest:** gathers remaining values into a variable.

---

## 5. Optional chaining (`?.`)

Optional chaining safely accesses a property when the value before `?.` might be `null` or `undefined`.

```js
const student = {
    name: "Rahul"
};

console.log(student.marks?.math); // undefined
```

`student.marks` is `undefined`. The `?.` stops the property access instead of throwing a `TypeError`.

If the property exists, the access continues:

```js
const student = {
    name: "Rahul",
    marks: {
        math: 90
    }
};

console.log(student.marks?.math); // 90
```

If the value before `?.` is `null`, the result is also `undefined`:

```js
const student = {
    name: "Rahul",
    marks: null
};

console.log(student.marks?.math); // undefined
```

**Important:** `?.` short-circuits when the value immediately before it is `null` or `undefined`. It does not return the original `null`; it returns `undefined` for that stopped access.

---

## 6. Nullish coalescing (`??`)

The `??` operator provides a fallback only when the value on its left is `null` or `undefined`.

```js
const name = null;

console.log(name ?? "Guest"); // Guest
```

If the left value is neither `null` nor `undefined`, it is kept:

```js
const name = "Rahul";

console.log(name ?? "Guest"); // Rahul
```

### `??` vs. `||`

`||` uses the fallback for any **falsy** value, including `0`, `false`, and `""`. `??` preserves those values and only falls back for `null` or `undefined`.

```js
const score = 0;

console.log(score ?? 100); // 0
console.log(score || 100); // 100
```

```js
const name = "";

console.log(name ?? "Guest"); // ""
console.log(name || "Guest"); // "Guest"
```



---

## 7. Combining `?.` and `??`

These operators can be used together. Optional chaining safely accesses a property; nullish coalescing supplies a fallback if the result is `null` or `undefined`.

```js
const user = null;

const name = user?.name ?? "Guest";

console.log(name); // Guest
```

Step by step:

1. `user?.name` returns `undefined` because `user` is `null`.
2. `undefined ?? "Guest"` returns `"Guest"`.

---

## Quick recap

|Syntax|Main purpose|
|---|---|
|`` `Hello, ${name}!` ``|Insert values into a string|
|`const [a, b] = array`|Destructure array values by position|
|`const { marks } = student`|Destructure an object property by name|
|`[...array]`|Spread array elements into a new array|
|`{ ...object }`|Copy object properties into a new object|
|`const [a, ...rest] = array`|Collect remaining array elements|
|`value?.property`|Safely access a property when the preceding value may be nullish|
|`value ?? fallback`|Use a fallback only for `null` or `undefined`|