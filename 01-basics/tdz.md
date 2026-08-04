## Q: What is the Temporal Dead Zone (TDZ)?

**Answer:**
The TDZ is the time between entering a scope and the point where a `let`/`const`
variable is actually declared. During this window, the variable exists in memory
(hoisted) but cannot be accessed — doing so throws a `ReferenceError`.

**Example:**
​```js
{
  console.log(num); // ❌ ReferenceError: Cannot access 'num' before initialization
  let num = 5;
}
​```

This is why `let`/`const` are considered "safer" than `var` — they prevent
accidental use of a variable before it's properly assigned.

**Follow-up questions interviewers ask:**
- Does TDZ apply to `const` too?
- Is TDZ the same as "not hoisted"? (No — it *is* hoisted, just not initialized)


**Note:** TDZ only applies to `let` and `const`. `var` is not affected because
it's hoisted and auto-initialized to `undefined`, so there's no "dead zone" for it.
