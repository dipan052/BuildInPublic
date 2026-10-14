
## 1. Arrays and objects

### Array

An array stores multiple values in order. You access its elements using indexes, starting from `0`.

```js
const marks = [80, 95, 65];

console.log(marks[1]); // 95
```

### Object

An object stores values as named properties.

```js
const student = {
    name: "Rahul",
    marks: 80,
    age: 20
};

console.log(student.marks); // 80
```

### Array of objects

```js
const students = [
    { name: "Rahul", marks: 80 },
    { name: "Priya", marks: 95 },
    { name: "Aman", marks: 65 }
];

console.log(students[1].marks); // 95
```

- Arrays use indexes, such as `students[1]`.
- Objects use property names, such as `.marks`.

---

## 2. `map()`

**Purpose:** Transform each array element and return the transformed values in a new array.

```js
const numbers = [2, 4, 6];

const result = numbers.map(function (n) {
    return n * 3;
});

console.log(result); // [6, 12, 18]
```

How it works:

- The callback receives each element as `n`.
- `return n * 3` gives the transformed value for that element.
- `map()` collects those returned values into a new array.
- The original `numbers` array remains unchanged in this example.

---

## 3. `filter()`

**Purpose:** Keep only the elements that satisfy a condition.

```js
const marks = [35, 82, 49, 91, 60];

const passed = marks.filter(function (mark) {
    return mark >= 50;
});

console.log(passed); // [82, 91, 60]
```

How it works:

- The callback checks each element, here named `mark`.
- If the condition returns `true`, that element is kept.
- If it returns `false`, that element is left out.

Another practice example:

```js
const numbers = [1, 2, 3, 4, 5];

const result = numbers.filter(function (n) {
    return n > 3;
});

console.log(result); // [4, 5]
```

---

## 4. `reduce()`

**Purpose:** Combine array elements into one accumulated result, such as a total.

```js
const prices = [100, 200, 50, 150];

const total = prices.reduce(function (sum, price) {
    return sum + price;
}, 0);

console.log(total); // 500
```

How it works:

- `sum` is the accumulator: the running result.
- `price` is the current array element.
- `0` is the initial accumulator value.
- The value returned by the callback becomes the next `sum`.

For the example above, the accumulator changes like this:

`0 → 100 → 300 → 350 → 500`

### `reduce()` can do more than addition

```js
const numbers = [3, 4, 2];

const result = numbers.reduce(function (sum, n) {
    return sum * n;
}, 1);

console.log(result); // 24
```

Here the initial accumulator is `1`, so the running result is:

`1 → 3 → 12 → 24`

The initial accumulator is the **second argument** passed to `reduce()`. It is not automatically `0`.

```js
const numbers = [10, 20, 30];

const result = numbers.reduce(function (sum, n) {
    return sum + n;
}, 100);

console.log(result); // 160
```

The accumulator starts at `100`: `100 → 110 → 130 → 160`.

---

## 5. `find()`

**Purpose:** Return the **first element** that satisfies a condition.

```js
const students = [
    { name: "Rahul", marks: 65 },
    { name: "Priya", marks: 95 },
    { name: "Aman", marks: 85 }
];

const result = students.find(function (student) {
    return student.marks > 90;
});

console.log(result); // { name: "Priya", marks: 95 }
```

How it works:

- `find()` checks the elements in order.
- It returns the first element for which the condition is `true`.
- It does not return an array of all matching elements.

---

## 6. `sort()`

**Purpose:** Sort the elements of an array.

```js
const numbers = [40, 5, 100, 25];

numbers.sort(function (a, b) {
    return a - b;
});

console.log(numbers); // [5, 25, 40, 100]
```

For this numeric example:

- `a - b` sorts in ascending order (smallest to largest).
- `sort()` changes the original array.

---

## Quick comparison

|Method|Main purpose|What it returns|
|---|---|---|
|`map()`|Transform every element|A new array of returned values|
|`filter()`|Keep elements matching a condition|An array of matching elements|
|`reduce()`|Accumulate values into one result|The final accumulated value|
|`find()`|Find the first matching element|The first matching element, or `undefined` if none matches|
|`sort()`|Sort an array|The sorted array (the original array is modified)|