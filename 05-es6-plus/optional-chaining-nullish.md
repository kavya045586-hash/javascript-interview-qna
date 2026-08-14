## Q: Optional Chaining (?.) and Nullish Coalescing (??)

**Answer:**

Both are ES2020 additions that make handling `null`/`undefined` values
much safer and more concise.

**Optional Chaining (`?.`) — safely access nested properties without errors**

```js
const user = { name: "Kavya", address: { city: "Bangalore" } };

console.log(user.address.city);        // "Bangalore" — works fine
console.log(user.contact.phone);        // ❌ TypeError: Cannot read properties of undefined

console.log(user.contact?.phone);        // ✅ undefined — no error, safely stops if contact doesn't exist
```

**How it works — it stops evaluating as soon as it hits null/undefined:**

```js
const data = { profile: null };
console.log(data.profile?.name);   // undefined — doesn't even try to access .name
console.log(data.profile.name);     // ❌ TypeError — direct access still crashes
```

**Optional chaining also works for function calls and array access:**

```js
const obj = {};
obj.greet?.();          // safely does nothing if greet doesn't exist — no error

const arr = null;
console.log(arr?.[0]);   // undefined — instead of crashing on null[0]
```

**Nullish Coalescing (`??`) — provides a default value ONLY for null/undefined**

```js
const count = 0;
console.log(count || 10);   // 10 — || treats 0 as falsy, incorrectly falls back!
console.log(count ?? 10);    // 0 — ?? only falls back for null/undefined, correctly keeps 0
```

**This is the key difference from `||` — a classic interview trap:**

```js
const values = ["", 0, false, null, undefined, NaN];

values.forEach(v => {
  console.log(v || "default");  // ALL of these become "default" — too aggressive
  console.log(v ?? "default");   // only null and undefined become "default" — more precise
});
```

**Combining both together (very common real pattern):**

```js
const city = user?.address?.city ?? "Unknown";
console.log(city); // "Bangalore" if it exists, otherwise "Unknown" — safe AND has a default
```

**Follow-up questions interviewers ask:**

- Why is `??` often safer than `||` for default values? (`||` treats ALL falsy values as "missing", including valid values like 0 or empty string; `??` only treats null/undefined as missing)
- Can you chain multiple `?.` together? (Yes — `a?.b?.c?.d` stops safely at the first null/undefined found anywhere in the chain)
