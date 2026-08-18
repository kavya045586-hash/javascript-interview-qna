## Q: LocalStorage vs SessionStorage vs Cookies

**Answer:**

All three store data in the browser, but differ in lifespan, size, and
how they're sent to the server.

**localStorage — persists forever, until manually cleared**

```js
localStorage.setItem("theme", "dark");
console.log(localStorage.getItem("theme")); // "dark"
localStorage.removeItem("theme");
localStorage.clear(); // removes everything
```
Data survives even after closing the browser or restarting the computer.

**sessionStorage — persists only for the current tab/session**

```js
sessionStorage.setItem("draft", "unsaved text");
// Data is cleared automatically when the TAB is closed
```

**Cookies — small pieces of data, sent to the SERVER with every request**

```js
document.cookie = "username=Kavya; expires=Fri, 31 Dec 2026 23:59:59 GMT";
```
Unlike localStorage/sessionStorage, cookies are automatically included in
every HTTP request to the server — making them useful for authentication
tokens/sessions, but also adding overhead to every request.

**Quick comparison table:**

| | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Lifespan | Until manually cleared | Until tab closes | Set expiration date |
| Storage limit | ~5-10MB | ~5-10MB | ~4KB |
| Sent to server? | No | No | Yes, with every request |
| Common use | User preferences, cached data | Temporary form data | Auth tokens, sessions |

**Follow-up questions interviewers ask:**

- Why are cookies still used for authentication despite localStorage existing? (Cookies are automatically sent with every request, and can be marked HttpOnly to prevent JavaScript access — reducing XSS attack risk for sensitive tokens)
- What's the storage size limit difference between localStorage and cookies?
