## Q: Debounce vs Throttle

**Answer:**

Both are techniques to limit how often a function runs, especially for
frequently-firing events like scroll, resize, or keystroke input — but
they behave differently.

**Debounce — waits until the events STOP firing, then runs once**

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);              // cancel the previous scheduled call
    timer = setTimeout(() => fn(...args), delay); // reschedule
  };
}

const search = debounce((query) => console.log("Searching:", query), 500);

// If user types "hello" quickly, only ONE search runs — 500ms after the LAST keystroke
input.addEventListener("input", (e) => search(e.target.value));
```

Use case: search-as-you-type, form validation — you only want to act
AFTER the user has stopped typing, not on every keystroke.

**Throttle — runs at most once every X milliseconds, no matter how often the event fires**

```js
function throttle(fn, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

const logScroll = throttle(() => console.log("Scroll position:", window.scrollY), 1000);
window.addEventListener("scroll", logScroll);
// Even if scroll fires 100 times per second, logScroll only runs once every 1000ms
```

Use case: scroll/resize handlers — you want REGULAR updates, just not on
EVERY single event firing (which could be hundreds per second).

**Quick comparison table:**

| | Debounce | Throttle |
|---|---|---|
| Runs | Once, after events stop | At regular intervals, DURING events |
| Best for | Search input, form validation | Scroll, resize, mouse move |

**Follow-up questions interviewers ask:**

- When would you choose throttle over debounce?
- What real bug does debounce/throttle prevent? (Excessive function calls — e.g., firing an API search request on every single keystroke, overwhelming the server)
