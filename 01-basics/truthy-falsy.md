## Q: Truthy and Falsy Values

**Answer:**

Every value in JavaScript is either "truthy" or "falsy" when evaluated in a
boolean context (like an `if` statement). Understanding this matters because
you'll constantly write conditions like `if (value)` without directly
comparing to `true`/`false`.

**There are only 6 falsy values in JavaScript — everything else is truthy:**

```js
false
0
""          // empty string
null
undefined
NaN
```

**Everything else — including things that "feel" empty — is truthy:**

```js
if ("0") { }       // truthy! (non-empty string, even though it "looks" like zero)
if ([]) { }         // truthy! (empty array is still truthy)
if ({}) { }          // truthy! (empty object is still truthy)
if (" ") { }          // truthy! (string with just a space)
```

**Example in practice:**

```js
let username = "";

if (username) {
  console.log("Welcome, " + username);
} else {
  console.log("Please enter a username"); // this runs — "" is falsy
}
```

**Follow-up questions interviewers ask:**

- Why are `[]` and `{}` truthy even though they seem "empty"? (Only the 6 listed values are falsy — everything else, including empty objects/arrays, is truthy by rule)
- What does `Boolean(value)` do, and how is it useful for checking truthy/falsy? (Explicitly converts any value to its true/false equivalent — good for debugging)
