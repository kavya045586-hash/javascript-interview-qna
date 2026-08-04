## Q: var vs let vs const

**Answer:**
- `var` — function-scoped, can be redeclared and updated, hoisted and initialized as `undefined`
- `let` — block-scoped, can be updated but not redeclared in the same scope, hoisted but NOT initialized (stays in "temporal dead zone")
- `const` — block-scoped, cannot be updated or redeclared; must be initialized at declaration (note: object/array *contents* can still be mutated)

**Example:**
​```js
var a = 1;
let b = 2;
const c = 3;

var a = 10;   // OK
b = 20;       // OK
c = 30;       // ❌ TypeError: Assignment to constant variable
​```

**Follow-up questions interviewers ask:**
- Why was `let`/`const` introduced if `var` already existed?
- Can you reassign a `const` object's property?