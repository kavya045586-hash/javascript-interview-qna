## Q: Fetch API vs XMLHttpRequest

**Answer:**

Both are used to make HTTP requests from JavaScript, but `fetch` is the
modern, cleaner replacement for the older `XMLHttpRequest` (XHR).

**XMLHttpRequest — older, more verbose, callback-based**

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://api.example.com/data");
xhr.onload = function () {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.responseText));
  }
};
xhr.onerror = function () {
  console.log("Request failed");
};
xhr.send();
```

**Fetch — modern, Promise-based, much cleaner**

```js
fetch("https://api.example.com/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log("Error:", error));

// Or with async/await
async function getData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Error:", error);
  }
}
```

**Important gotcha: fetch does NOT reject on HTTP error status codes (404, 500) — only on network failures**

```js
fetch("https://api.example.com/missing-page")
  .then(response => {
    if (!response.ok) {           // must manually check response.ok
      throw new Error("HTTP error: " + response.status);
    }
    return response.json();
  })
  .catch(error => console.log(error));
```

**Quick comparison table:**

| | XMLHttpRequest | Fetch |
|---|---|---|
| Syntax | Verbose, callback-based | Clean, Promise-based |
| Rejects on 404/500? | No (need manual status check) | No (also needs manual `response.ok` check) |
| Supports async/await? | No | Yes |
| Modern preference | Legacy code only | Standard for new code |

**Follow-up questions interviewers ask:**

- Does fetch throw an error for a 404 response? (No — only for network failures; you must manually check `response.ok` or `response.status`)
- Why is fetch generally preferred over XMLHttpRequest today? (Cleaner syntax, native Promise support, works naturally with async/await)
