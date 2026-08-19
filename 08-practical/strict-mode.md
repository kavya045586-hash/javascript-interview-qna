## Q: Strict Mode

**Answer:**

`"use strict"` enables a stricter set of rules for JavaScript execution,
catching common mistakes and preventing certain unsafe actions.

**Enabling strict mode:**

```js
"use strict";  // applies to the entire file if at the top

function example() {
  "use strict";  // or just this function, if placed inside it
}
```

**What strict mode changes — key differences:**

```js
// 1. Assigning to an undeclared variable throws an error
"use strict";
x = 10; // ❌ ReferenceError: x is not defined
// (without strict mode, this would silently create a global variable)

// 2. `this` in a plain function call is undefined, not the global object
"use strict";
function show() {
  console.log(this); // undefined
}
show();

// 3. Duplicate parameter names are not allowed
"use strict";
function sum(a, a, b) {} // ❌ SyntaxError

// 4. Assigning to a read-only property fails loudly (throws), instead of silently doing nothing
"use strict";
const obj = Object.freeze({ name: "Kavya" });
obj.name = "Nagar"; // ❌ TypeError (silently fails without strict mode)
```

**Why it matters:** ES6 modules and classes are automatically in strict
mode by default — so if you're using modern JS syntax, you're often
already benefiting from these protections without writing `"use strict"`
explicitly.

**Follow-up questions interviewers ask:**

- What happens if you assign to an undeclared variable WITHOUT strict mode? (Silently creates a global variable — a common source of bugs)
- Are ES6 classes automatically in strict mode? (Yes — along with ES6 modules)
