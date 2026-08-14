## Q: Map/Set vs Object/Array

**Answer:**

ES6 introduced `Map` and `Set` as alternatives to `Object` and `Array`,
solving specific limitations of the older structures.

**Map vs Object**

```js
// Object — keys are always converted to strings
const obj = {};
obj[1] = "one";       // key becomes the STRING "1"
obj[true] = "yes";     // key becomes the STRING "true"

// Map — keys can be ANY type, and keep their original type
const map = new Map();
map.set(1, "one");        // key stays the NUMBER 1
map.set(true, "yes");      // key stays the BOOLEAN true
map.set({ id: 1 }, "obj"); // even objects can be keys!

console.log(map.get(1)); // "one"
```

**Key differences:**

| | Object | Map |
|---|---|---|
| Key types | Strings/Symbols only (auto-converted) | ANY type (objects, functions, etc.) |
| Order guaranteed? | Mostly (with caveats for numeric keys) | Yes — always insertion order |
| Size | `Object.keys(obj).length` (manual) | `map.size` (built-in property) |
| Iterable directly? | No — need `Object.entries()` first | Yes — directly with `for...of` |

**Set vs Array**

```js
// Array — allows duplicates
const arr = [1, 2, 2, 3, 3, 3];
console.log(arr); // [1, 2, 2, 3, 3, 3]

// Set — automatically removes duplicates
const set = new Set([1, 2, 2, 3, 3, 3]);
console.log(set); // Set(3) {1, 2, 3}
```

**Common real use case — removing duplicates from an array using Set:**

```js
const numbers = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(numbers)];
console.log(unique); // [1, 2, 3, 4, 5]
```

**Key differences:**

| | Array | Set |
|---|---|---|
| Duplicates allowed? | Yes | No — automatically deduplicated |
| Access by index? | Yes (`arr[0]`) | No — no index-based access |
| Checking existence | `.includes()` — slower, checks each element | `.has()` — faster lookup |

**Follow-up questions interviewers ask:**

- Why would you use a Map instead of a plain Object?
- What's the fastest way to remove duplicates from an array? (Spread a Set built from the array: `[...new Set(array)]`)
