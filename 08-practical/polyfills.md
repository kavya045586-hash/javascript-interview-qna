## Q: Polyfills (Writing Your Own map/filter/bind)

**Answer:**

A polyfill is custom code that replicates a built-in feature's behavior —
often written for older environments that don't support it, or asked in
interviews to test your understanding of how a built-in method works
internally.

**Custom `map()` polyfill:**

```js
Array.prototype.myMap = function (callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};

console.log([1, 2, 3].myMap(n => n * 2)); // [2, 4, 6]
```

**Custom `filter()` polyfill:**

```js
Array.prototype.myFilter = function (callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

console.log([1, 2, 3, 4].myFilter(n => n % 2 === 0)); // [2, 4]
```

**Custom `bind()` polyfill:**

```js
Function.prototype.myBind = function (context, ...args) {
  const fn = this; // the original function
  return function (...newArgs) {
    return fn.apply(context, [...args, ...newArgs]);
  };
};

function greet(greeting) {
  console.log(greeting + ", " + this.name);
}
const boundGreet = greet.myBind({ name: "Kavya" });
boundGreet("Hello"); // "Hello, Kavya"
```

**Why interviewers ask this:** it tests whether you understand the
underlying mechanics (looping, `this`, `apply`/`call`) rather than just
knowing how to USE the built-in methods.

**Follow-up questions interviewers ask:**

- Why does the bind polyfill use `apply` internally? (apply lets you pass the context AND an array of arguments in one call, which is exactly what's needed to combine originally-bound args with newly-passed ones)
- Could you write a polyfill for reduce()? (Yes — similar loop structure, but accumulates a single value instead of building an array)
