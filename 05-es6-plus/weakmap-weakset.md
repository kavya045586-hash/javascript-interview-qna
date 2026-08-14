## Q: WeakMap and WeakSet

**Answer:**

`WeakMap` and `WeakSet` are similar to `Map` and `Set`, but with one key
difference: they hold **"weak" references** to their keys (WeakMap) or
values (WeakSet) — meaning those references don't prevent JavaScript's
garbage collector from cleaning up the memory if nothing else references
that object.

**Regular Map — keeps objects alive in memory, even if nothing else references them**

```js
let obj = { name: "Kavya" };
const map = new Map();
map.set(obj, "some data");

obj = null; // remove the only OTHER reference to this object

// The object is STILL alive in memory, because map still holds a reference to it
// This can cause a memory leak if you forget to manually delete it from the map
```

**WeakMap — allows garbage collection automatically**

```js
let obj = { name: "Kavya" };
const weakMap = new WeakMap();
weakMap.set(obj, "some data");

obj = null; // remove the only OTHER reference

// Now the object CAN be garbage collected — WeakMap's reference doesn't count
// as a reason to keep it in memory. JS automatically cleans it up.
```

**Key restrictions of WeakMap/WeakSet (these exist BECAUSE of the weak reference behavior):**

```js
const weakMap = new WeakMap();

weakMap.set("string key", "value"); // ❌ TypeError — keys MUST be objects, not primitives
weakMap.set({}, "value");             // ✅ works — objects only

// NOT iterable — no forEach, no for...of, no .size
weakMap.forEach(...);  // ❌ TypeError — doesn't exist on WeakMap
```

**Why these restrictions exist:** Since entries can disappear at any time
(whenever garbage collection runs), it would be unpredictable/unsafe to
iterate over a WeakMap or check its size — the contents could change
mid-iteration without your code doing anything.

**Real use case — storing private/extra data tied to an object's lifecycle, without causing memory leaks:**

```js
const privateData = new WeakMap();

class User {
  constructor(name) {
    privateData.set(this, { name });  // "private" data tied to this specific instance
  }

  getName() {
    return privateData.get(this).name;
  }
}

const user = new User("Kavya");
console.log(user.getName()); // "Kavya"
// If `user` is ever set to null and garbage collected, its entry in
// privateData is automatically cleaned up too — no manual cleanup needed
```

**Quick comparison table:**

| | Map/Set | WeakMap/WeakSet |
|---|---|---|
| Key/value types | Any type | Objects only (for keys) |
| Prevents garbage collection? | Yes — keeps references alive | No — allows GC when nothing else references it |
| Iterable? | Yes | No |
| Has `.size`? | Yes | No |
| Use case | General-purpose storage | Memory-safe metadata tied to
