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

**How the server allows it (this is a SERVER-side fix, not something you can bypass from the frontend):**
