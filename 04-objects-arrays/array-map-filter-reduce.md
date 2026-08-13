## Q: Array map, filter, and reduce

**Answer:**

These three are the most commonly used array methods in real code —
each transforms an array in a different way, and none of them mutate the
original array.

**1. `map()` — transforms each element, returns a NEW array of the same length**

```js
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(n => n * 2);

console.log(doubled);  // [2, 4, 6, 8]
console.log(numbers);   // [1, 2, 3, 4] — original unchanged
```

**2. `filter()` — keeps only elements that pass a test, returns a NEW array (possibly shorter)**

```js
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter(n => n % 2 === 0);

console.log(evens); // [2, 4, 6]
```

**3. `reduce()` — combines all elements into a SINGLE value**

```js
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((accumulator, current) => accumulator + current, 0);

console.log(sum); // 10
```

**Step by step how reduce works:**
