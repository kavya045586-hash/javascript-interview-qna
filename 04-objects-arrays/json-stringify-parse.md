## Q: JSON.stringify and JSON.parse

**Answer:**

These convert between JavaScript objects and JSON strings — essential for
sending/receiving data over APIs, since JSON is a plain text format.

**`JSON.stringify()` — converts a JS object/array into a JSON string**

```js
const user = { name: "Kavya", age: 21 };
const jsonString = JSON.stringify(user);

console.log(jsonString);        // '{"name":"Kavya","age":21}'
console.log(typeof jsonString);  // "string" — it's now text, not an object
```

**`JSON.parse()` — converts a JSON string back into a JS object**

```js
const jsonString = '{"name":"Kavya","age":21}';
const user = JSON.parse(jsonString);

console.log(user.name);        // "Kavya"
console.log(typeof user);       // "object" — back to a real object
```

**Common real use case — localStorage (which only stores strings):**

```js
const user = { name: "Kavya", age: 21 };

localStorage.setItem("user", JSON.stringify(user)); // must convert to string first
const saved = JSON.parse(localStorage.getItem("user")); // convert back to object
console.log(saved.name); // "Kavya"
```

**Important limitations of JSON.stringify — things it silently drops or changes:**

```js
const obj = {
  name: "Kavya",
  greet: function () { console.log("hi"); },  // functions are DROPPED
  age: undefined,                               // undefined is DROPPED
  date: new Date()                               // Date becomes a STRING
};

console.log(JSON.stringify(obj));
// '{"name":"Kavya","date":"2026-08-13T..."}' — greet and age are gone!
```

**Pretty-printing with indentation (useful for debugging/logging):**

```js
console.log(JSON.stringify(user, null, 2));
// {
//   "name": "Kavya",
//   "age": 21
// }
```

**Follow-up questions interviewers ask:**

- What happens to functions and `undefined` values when you use JSON.stringify? (They're silently dropped/skipped, not converted)
- Why can't you store an object directly in localStorage? (localStorage only stores strings — you must stringify first, then parse when reading it back)
