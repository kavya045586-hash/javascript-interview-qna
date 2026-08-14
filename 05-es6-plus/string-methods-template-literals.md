## Q: String Methods & Template Literals

**Answer:**

**Template literals (`` ` ` ``) — modern string syntax with embedded expressions and multi-line support**

```js
const name = "Kavya";
const age = 21;

// Old way — string concatenation
const greeting = "Hello, " + name + "! You are " + age + " years old.";

// Template literal — much cleaner
const greeting = `Hello, ${name}! You are ${age} years old.`;
```

**Multi-line strings (no need for `\n` or concatenation):**

```js
const message = `Line 1
Line 2
Line 3`;
```

**Expressions inside `${}` — not just variables, any valid JS expression:**

```js
const a = 5, b = 10;
console.log(`Sum: ${a + b}`);  // "Sum: 15"
console.log(`${a > b ? "a is bigger" : "b is bigger"}`); // "b is bigger"
```

**Common string methods:**

```js
const str = "  Hello World  ";

str.trim();               // "Hello World" — removes whitespace from both ends
str.toUpperCase();          // "  HELLO WORLD  "
str.toLowerCase();           // "  hello world  "
str.includes("World");        // true
str.replace("World", "JS");    // "  Hello JS  "
str.split(" ");                  // ["", "", "Hello", "World", "", ""]
str.slice(2, 7);                   // "Hello" — extracts by index range
"5".padStart(3, "0");                // "005" — useful for formatting IDs/numbers
```

**Follow-up questions interviewers ask:**

- What's the main advantage of template literals over string concatenation?
- What's the difference between `slice()` and `substring()`? (Very similar, but slice() can accept negative indices to count from the end; substring() treats negative values as 0)
