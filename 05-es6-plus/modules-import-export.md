## Q: JavaScript Modules (import/export)

**Answer:**

Modules let you split code across multiple files, each with its own
scope — nothing leaks into the global scope unless explicitly exported.
This makes code more organized, reusable, and easier to maintain.

**Named exports — export multiple things from one file**

```js
// math.js
export const PI = 3.14;
export function add(a, b) {
  return a + b;
}

// main.js
import { PI, add } from './math.js';
console.log(add(2, 3)); // 5
```

**Default export — one main thing per file**

```js
// user.js
export default function greet(name) {
  console.log("Hello, " + name);
}

// main.js
import greet from './user.js';  // no {} needed, and can name it anything
greet("Kavya");
```

**Renaming imports/exports:**

```js
// math.js
export { add as sum };

// main.js
import { sum as addNumbers } from './math.js';
```

**Importing everything as a namespace object:**

```js
import * as math from './math.js';
console.log(math.PI, math.add(1, 2));
```

**Quick comparison table:**

| | Named Export | Default Export |
|---|---|---|
| How many per file? | Multiple allowed | Only ONE |
| Import syntax | `import { name } from '...'` | `import anyName from '...'` |
| Must match export name? | Yes (unless renamed) | No — can import with any name |

**Follow-up questions interviewers ask:**

- What's the difference between CommonJS (`require`) and ES Modules (`import`)? (CommonJS is Node's older synchronous system; ES Modules are the standardized, asynchronous-capable modern approach, now supported natively in both browsers and Node)
- Can a file have both named and default exports? (Yes — commonly done, e.g. exporting a main component as default plus some helper utilities as named exports)
