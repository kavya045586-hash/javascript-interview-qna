## Q: CORS (Cross-Origin Resource Sharing)

**Answer:**

CORS is a browser security feature that restricts web pages from making
requests to a DIFFERENT origin (domain, protocol, or port) than the one
that served the page — unless that other server explicitly allows it.

**The problem CORS solves:** without it, any malicious website could
silently make requests to your bank's API using YOUR logged-in session,
since browsers automatically send cookies with requests.

**What triggers a CORS error:**

```js
// Page is served from https://myapp.com
fetch("https://api.otherdomain.com/data")
  .then(res => res.json())
  .catch(err => console.log(err));
// ❌ Blocked by CORS policy — unless otherdomain.com explicitly allows myapp.com
```
Access-Control-Allow-Origin: https://myapp.com

**How the server allows it (this is a SERVER-side fix, not something you can bypass from the frontend):**
The server must include this header in its response for the browser to
allow the request to succeed.

**Important: CORS is NOT a frontend problem to "fix" in your JS code** —
it's enforced by the browser based on the SERVER's response headers. If
you're building the API, you (or your backend framework, like Express's
`cors` package) need to configure it there.

**Follow-up questions interviewers ask:**

- Is CORS a frontend or backend concern to actually fix? (Backend — the server must send the right headers; frontend code cannot bypass CORS restrictions)
- What real-world attack does CORS help prevent? (Cross-site request forgery-style attacks where a malicious site tries to make authenticated requests to another site using the user's existing session/cookies)
