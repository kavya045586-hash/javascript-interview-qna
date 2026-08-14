## Q: Generators & Iterators

**Answer:**

**Iterators** — objects with a `.next()` method that returns
`{ value, done }`, letting you step through a sequence one item at a time.

**Generators** — a special function (`function*`) that makes creating
iterators much easier. Instead of manually managing state, you use `yield`
to "pause" the function and produce a value.

**Basic generator syntax:**

```js
function* countTo3() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = countTo3(); // calling a generator does NOT run it — it returns an iterator

console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

**Key idea: `yield` pauses execution, remembering exactly where it left off**

```js
function* logSteps() {
  console.log("Step 1");
  yield;
  console.log("Step 2");
  yield;
  console.log("Step 3");
}

const gen = logSteps();
gen.next(); // logs "Step 1", then pauses
gen.next(); // logs "Step 2", then pauses
gen.next(); // logs "Step 3"
```

**Generators work with `for...of` automatically (since they're iterable):**

```js
function* countTo3() {
  yield 1;
  yield 2;
  yield 3;
}

for (const num of countTo3()) {
  console.log(num); // 1, 2, 3 — no need to call .next() manually
}
```

**Real use case — generating infinite sequences safely (only computed when needed):**

```js
function* infiniteCounter() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

const counter = infiniteCounter();
console.log(counter.next().value); // 0
console.log(counter.next().value); // 1
console.log(counter.next().value); // 2
// Never actually loops forever in memory — values are generated ONE AT A TIME, on demand
```

**Follow-up questions interviewers ask:**

- What does `yield` actually do, compared to `return`? (yield pauses and can resume later, preserving state; return ends the function completely)
- Why are generators useful for infinite sequences? (They compute values lazily — one at a time, on demand — rather than trying to generate everything upfront, which would crash with a truly infinite loop)
