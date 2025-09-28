Here’s your **Concept Mastery Cheat Sheet** for **DOM & Browser APIs in JavaScript** — compact, structured, and interview-ready.

---

# 🌐 DOM & Browser APIs — Cheat Card

**Concept:**
The **DOM (Document Object Model)** = tree-like representation of HTML/XML docs.
Browser APIs extend JS with environment-specific features (networking, storage, UI, etc.).

---

### 1. DOM (Core)

* **Selectors**

  ```js
  document.getElementById("id");
  document.querySelector(".class");
  document.querySelectorAll("div");
  ```

* **Traversal**

  ```js
  el.parentNode; el.children; el.nextSibling;
  ```

* **Manipulation**

  ```js
  el.textContent = "Hi";
  el.setAttribute("data-x", 10);
  el.classList.add("active");
  ```

* **Creation/Removal**

  ```js
  const div = document.createElement("div");
  document.body.appendChild(div);
  el.remove();
  ```

* **Events**

  ```js
  el.addEventListener("click", fn);
  el.removeEventListener("click", fn);
  ```

---

### 2. Browser APIs

**Timers & Async**

```js
setTimeout(fn, 1000);
setInterval(fn, 500);
queueMicrotask(fn);
requestAnimationFrame(fn);
```

**Networking**

```js
fetch("/api").then(res => res.json());
navigator.sendBeacon(url, data);
WebSocket, EventSource
```

**Storage**

```js
localStorage.setItem("k", "v");
sessionStorage.getItem("k");
indexedDB.open("db", 1);
```

**UI & Interaction**

```js
alert("Hi");
prompt("Enter name");
confirm("Sure?");
document.cookie = "id=123";
```

**Geolocation & Sensors**

```js
navigator.geolocation.getCurrentPosition(pos => ...);
```

**Clipboard & Files**

```js
navigator.clipboard.writeText("copy me");
input.files; // File API
```

**Workers**

```js
new Worker("worker.js");
```

**Others**

* History API (`history.pushState`)
* Notifications API
* IntersectionObserver
* MutationObserver

---

### Visualization

```
JS Runtime
 ├─ Core Language (ECMAScript)
 ├─ DOM API (manipulate page)
 └─ Browser APIs (async, storage, network, etc.)
```

🧠 **Mental Model:** DOM = page tree, Browser APIs = extra powers.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not part of ECMAScript**

   * DOM/Browser APIs are **host environment features**.

2. **Async nature**

   * Many APIs (fetch, geolocation) are async → use Promises/callbacks.

3. **Cross-browser differences**

   * Some APIs experimental / prefixed.

4. **Security restrictions**

   * Clipboard, Notifications → require user permission.
   * Cookies → subject to SameSite/CORS rules.

5. **Blocking dialogs**

   * `alert`, `prompt`, `confirm` block main thread → discouraged.

6. **Workers**

   * No DOM access, runs in isolated thread.

7. **Storage limits**

   * localStorage/sessionStorage ~5–10 MB, synchronous blocking.

---

### ✅ Pros

* Rich interaction model.
* Persistent storage & offline support.
* Async networking built-in.
* APIs for sensors, media, notifications.

---

### ❌ Cons

* Fragmented support across browsers.
* Security restrictions (permissions, CORS).
* Some APIs block main thread (bad UX).

---

### Best Practices

* ✅ Use modern async APIs (`fetch`, `async/await`).
* ✅ Batch DOM updates to reduce reflows/repaints.
* ✅ Use `requestAnimationFrame` for smooth animations.
* ✅ Clean up event listeners & intervals → avoid leaks.
* ✅ Check browser compatibility (`caniuse.com`).
* ✅ Prefer `localStorage`/`IndexedDB` over cookies for storage.

---

👉 Mnemonic:
**“DOM = tree, Browser APIs = superpowers.”**

---

⚡ Do you want me to now prepare a **Cheat Card on “Event Handling & Event Delegation in DOM”** (since it’s one of the most asked areas in interviews)?


Perfect — let’s make a **Cheat Card** that covers **DOM Tree Structure, Selecting Elements, and Events (Bubbling/Capturing)** in one place, so you’re crystal-clear for interviews.

---

# 🌳 DOM Tree, Selection & Events — Cheat Card

**Concept:**
The **DOM** is a tree-like structure of nodes representing an HTML document.
JS can **select**, **traverse**, and **manipulate** nodes, and handle **events** through bubbling/capturing.

---

### 1. DOM Tree Structure

* **Root** → `document`
* **Node types**:

  * `document` (root)
  * `element` (`<div>`)
  * `text` (“hello”)
  * `attribute` (`id="x"`)
  * `comment`

**Traversal:**

```js
el.parentNode;
el.children;
el.firstChild;
el.nextSibling;
```

**Visualization:**

```
<html>
 └─ <body>
     ├─ <div id="box">
     │   └─ "Hello"
     └─ <p>
```

---

### 2. Selecting Elements

**By ID / Class / Tag**

```js
document.getElementById("id");
document.getElementsByClassName("cls"); // live HTMLCollection
document.getElementsByTagName("div");
```

**Modern (querySelector)**

```js
document.querySelector("#id");       // first match
document.querySelectorAll(".cls");   // NodeList (static)
```

**Special**

```js
document.forms[0];
document.images;
```

---

### 3. Events & Bubbling/Capturing

**Add & Remove**

```js
el.addEventListener("click", handler, options);
el.removeEventListener("click", handler);
```

**Options:**

* `{ capture: true }` → run in **capturing phase**
* `{ once: true }` → auto-remove after 1 call
* `{ passive: true }` → don’t block scroll

---

#### Event Flow

1. **Capturing** → root → target.
2. **Target** → actual element.
3. **Bubbling** → target → root.

```js
div.addEventListener("click", e => console.log("Div bubble"));
div.addEventListener("click", e => console.log("Div capture"), true);
```

**Stop propagation:**

```js
e.stopPropagation();   // stop bubbling/capturing
e.stopImmediatePropagation(); // stop + no more listeners on same element
```

---

### Visualization

```
Click <button> inside <div> inside <body>

Capture: document → body → div → button
Target: button
Bubble: button → div → body → document
```

🧠 **Mental Model:** Events travel **down (capture)**, hit target, then **up (bubble)**.

---

### Gotchas ⚠️

1. **Not all events bubble** → `focus`, `blur`, `mouseenter`, `mouseleave`.
2. **`querySelectorAll` NodeList is static**, but `getElementsBy*` is live.
3. **Inline `onclick=` discouraged** → no separation of concerns.
4. **Event delegation**

   * Use bubbling to attach one listener to parent instead of many children.

   ```js
   list.addEventListener("click", e => {
     if (e.target.tagName === "LI") console.log(e.target.textContent);
   });
   ```

---

### ✅ Pros

* Event bubbling enables **delegation** → efficient listeners.
* Capturing gives fine-grained control.
* Flexible element selection APIs.

---

### ❌ Cons

* Bubbling can cause **unexpected handlers firing**.
* Old live collections (`getElementsBy*`) may confuse.
* Cross-browser quirks for older APIs.

---

### Best Practices

* ✅ Prefer `querySelector/querySelectorAll`.
* ✅ Use event delegation for dynamic lists.
* ✅ Always clean up event listeners.
* ✅ Use `stopPropagation` carefully.
* ✅ For scroll/touch events → use `{ passive: true }`.

---

👉 Mnemonic:
**“Capture → Target → Bubble (CTB).”**

---

⚡ Do you want me to next prepare a **Cheat Card specifically on Event Delegation** (since it’s a very common interview drill after bubbling/capturing)?


Here’s your **Concept Mastery Cheat Sheet** for **Event Delegation in JavaScript** — compact, visual, and packed with gotchas for interviews.

---

# 👥 Event Delegation — Cheat Card

**Concept:**
**Event Delegation** = attach a single event listener to a **parent element** to handle events on its **child elements** using **event bubbling**.

* Saves memory, easier to manage dynamic elements.

---

### Example

```html
<ul id="list">
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

```js
document.getElementById("list")
  .addEventListener("click", e => {
    if (e.target.tagName === "LI") {
      console.log("Clicked:", e.target.textContent);
    }
  });
```

* Listener on `<ul>` handles clicks for `<li>` children.
* Even works for `<li>` added later.

---

### Why Use Delegation?

1. **Performance** → fewer listeners = less memory overhead.
2. **Dynamic content** → works for elements added after page load.
3. **Cleaner code** → single handler instead of many.

---

### Visualization

```
Click on <li>
 → bubbles up to <ul>
 → parent’s listener checks event.target
 → handles dynamically
```

🧠 **Mental Model:** “One parent rules them all.”

---

### Gotchas ⚠️ (Exhaustive)

1. **Event.target vs currentTarget**

   * `event.target` = actual clicked element.
   * `event.currentTarget` = element with listener.

2. **Non-bubbling events**

   * `focus`, `blur`, `mouseenter`, `mouseleave` don’t bubble.
   * Use `focusin` / `focusout` instead.

3. **Too generic selectors**

   * Parent may capture unwanted clicks.
   * Always filter: `if (e.target.matches("li")) { ... }`.

4. **Nested elements**

   * Clicking child inside `<li>` sets `event.target` to child.
   * Use `closest()` to walk up.

   ```js
   const li = e.target.closest("li");
   if (li) console.log(li.textContent);
   ```

5. **Performance trap**

   * Delegation on `document` for everything = costly.
   * Place listeners at the closest stable ancestor.

6. **Stopping propagation**

   * If a child calls `e.stopPropagation()`, parent won’t catch.

---

### ✅ Pros

* Efficient memory usage.
* Simplifies code when managing many children.
* Works with dynamically added DOM nodes.

---

### ❌ Cons

* Doesn’t work with non-bubbling events.
* Requires extra checks (`matches`, `closest`).
* Can fire unintentionally on nested/overlapping elements.

---

### Best Practices

* ✅ Use `e.target.matches()` or `closest()` to filter targets.
* ✅ Choose the **nearest stable parent** for delegation (not always `document`).
* ✅ Use delegation for dynamic lists, tables, or grids.
* ✅ Avoid overusing global delegation → perf hit.
* ✅ For non-bubbling events → attach directly.

---

👉 Mnemonic:
**“Delegate up, filter down.”**

---

⚡ Next step: Do you want me to prepare a **Cheat Card on Event.target vs currentTarget vs this in event handlers** (since it’s the #1 area of confusion in delegation interviews)?


Here’s your **Concept Mastery Cheat Sheet** for the **Fetch API in JavaScript** — dense, structured, and fully interview-ready.

---

# 🌐 Fetch API — Cheat Card

**Concept:**
`fetch()` = modern Promise-based API for **network requests** (replacement for `XMLHttpRequest`).

* Returns a **Promise** that resolves to a `Response` object.

---

### Syntax

```js
fetch(url, options?)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

### Options

```js
fetch("/api", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Kalidas" }),
  credentials: "include", // cookies
  mode: "cors",           // cors | no-cors | same-origin
  cache: "no-cache",      // cache policy
  redirect: "follow"      // manual | error
});
```

---

### Response Handling

```js
const res = await fetch("/data.json");

await res.text();     // raw text
await res.json();     // JSON object
await res.blob();     // binary data
await res.arrayBuffer(); // low-level binary
await res.formData(); // form data
```

---

### Error Handling

* Fetch only rejects on **network failure**, not HTTP errors.

```js
fetch("/api")
  .then(r => {
    if (!r.ok) throw new Error(r.status);
    return r.json();
  })
  .catch(e => console.error("Failed:", e));
```

---

### Advanced Features

**AbortController** (cancel requests):

```js
const controller = new AbortController();
fetch("/slow", { signal: controller.signal });
controller.abort();
```

**Streaming** (progressive consumption):

```js
const reader = res.body.getReader();
```

**Upload files**:

```js
const form = new FormData();
form.append("file", fileInput.files[0]);
await fetch("/upload", { method: "POST", body: form });
```

---

### Visualization

```
fetch(url, options)
   ↓
Promise<Response>
   ↓
Response methods: json(), text(), blob(), ...
```

🧠 **Mental Model:** `fetch = request → promise → response object`.

---

### Gotchas ⚠️ (Exhaustive)

1. **No automatic rejection on 404/500**

   * Must check `response.ok` or `status`.

2. **CORS restrictions**

   * Browser enforces; server must allow.

3. **Cookies & credentials**

   * Need `{ credentials: "include" }`.

4. **Streaming support**

   * Available in modern browsers, not all.

5. **AbortController needed**

   * No built-in timeout → must implement manually.

6. **Redirect handling**

   * Default is `follow`. Can be changed.

7. **Cross-browser differences**

   * IE doesn’t support `fetch`. Use polyfill.

---

### ✅ Pros

* Promise-based, cleaner than XHR.
* Supports streaming, AbortController.
* Works with modern APIs (Service Workers).

---

### ❌ Cons

* No native timeout (must use AbortController).
* Doesn’t reject on HTTP errors.
* CORS pain points.
* Not supported in very old browsers.

---

### Best Practices

* ✅ Always check `response.ok` + `status`.
* ✅ Use `async/await` for cleaner syntax.
* ✅ Abort slow/unused requests.
* ✅ Wrap with timeout helpers.
* ✅ Use try/catch + `.catch()` for full error handling.

---

👉 Mnemonic:
**“Fetch → Promise → Response → Data.”**

---

⚡ Do you want me to now prepare a **Cheat Card on Axios vs Fetch (pros, cons, feature differences)** — since that’s a common follow-up in interviews?


Here’s your **Concept Mastery Cheat Sheet** for **Web Storage: `localStorage`, `sessionStorage`, and Cookies** — sharp, comparative, and interview-ready.

---

# 🗄️ Web Storage — Cheat Card

**Concept:**
Browsers provide **client-side storage** to persist small amounts of data.
Main options: **`localStorage`**, **`sessionStorage`**, and **Cookies**.

---

### 1. `localStorage`

* Key-value storage (string only).
* Persistent across sessions (until cleared).

```js
localStorage.setItem("user", "Kalidas");
localStorage.getItem("user");  // "Kalidas"
localStorage.removeItem("user");
localStorage.clear();
```

* Size: ~5–10MB.
* Synchronous API (blocks main thread).

---

### 2. `sessionStorage`

* Same API as `localStorage`.
* Data lives only for **tab/session lifetime**.
* Cleared when tab closed.

```js
sessionStorage.setItem("token", "abc123");
```

---

### 3. Cookies

* Key-value pairs sent with **every HTTP request** (if not `HttpOnly`).
* Originally for server state/session tracking.

```js
document.cookie = "theme=dark; path=/; max-age=3600";
```

**Attributes:**

* `Secure` → HTTPS only.
* `HttpOnly` → inaccessible to JS.
* `SameSite` → restricts cross-site sending (`Lax`, `Strict`, `None`).

Size: ~4KB per cookie.

---

### Comparison

| Feature                 | localStorage             | sessionStorage     | Cookies                    |
| ----------------------- | ------------------------ | ------------------ | -------------------------- |
| **Persistence**         | Until cleared            | Tab/session end    | Configurable (expiry)      |
| **Capacity**            | 5–10 MB                  | 5–10 MB            | ~4 KB                      |
| **Auto-sent to server** | ❌ No                     | ❌ No               | ✅ Yes                      |
| **Use case**            | Preferences, cached data | Temporary tab data | Auth/session, server comms |

---

### Visualization

```
localStorage   → persistent key-value (per domain)
sessionStorage → per-tab/session key-value
cookies        → small, auto-sent to server
```

🧠 **Mental Model:**

* **localStorage** = “long-term drawer.”
* **sessionStorage** = “tab sticky note.”
* **cookie** = “tiny server message slip.”

---

### Gotchas ⚠️ (Exhaustive)

1. **Strings only** → need `JSON.stringify`/`parse` for objects.
2. **Synchronous** → heavy use can block UI thread.
3. **Cookies auto-send** → can bloat requests if misused.
4. **Security**

   * XSS → can steal storage/cookies (except HttpOnly).
   * Use `Secure`, `SameSite`, and short lifetimes for cookies.
5. **Cross-domain restrictions** → storage is per origin.
6. **Quota exceeded** → storage errors if size limit reached.
7. **Inconsistent clearing** → private mode may restrict storage.

---

### ✅ Pros

* `localStorage` → large, persistent, easy to use.
* `sessionStorage` → simple, isolated per tab.
* `cookies` → server-compatible, standard for sessions.

---

### ❌ Cons

* `localStorage`/`sessionStorage` → synchronous, no server sync.
* `cookies` → tiny size, sent every request = performance hit.
* All are vulnerable to XSS if not protected.

---

### Best Practices

* ✅ Use `localStorage` for user preferences, cache.
* ✅ Use `sessionStorage` for temporary per-tab state.
* ✅ Use `cookies` only for session/auth tokens — with `HttpOnly + Secure + SameSite`.
* ✅ Store sensitive data **only in HttpOnly cookies**, not storage APIs.
* ✅ Always serialize/deserialize objects properly.

---

👉 Mnemonic:
**“local = long, session = short, cookie = server.”**

---

⚡ Next step: Want me to prepare a **Cheat Card on Secure Client-Side Storage Patterns (JWT handling, cookies vs storage)** — since that’s a frequent interview twist?


Here’s your **Concept Mastery Cheat Sheet** for **Web Workers & Service Workers** — compact, visual, and interview-ready.

---

# ⚙️ Web Workers vs Service Workers — Cheat Card

**Concept:**
Both are **background scripts** running separately from the main JS thread, but with different purposes:

* **Web Workers** → offload heavy computation.
* **Service Workers** → proxy between app & network for offline/caching.

---

### 1. Web Workers

* Run scripts in **background threads**.
* No DOM access.
* Communicate with main thread via **`postMessage` / `onmessage`**.

```js
// worker.js
self.onmessage = e => {
  self.postMessage(e.data * 2);
};

// main.js
const worker = new Worker("worker.js");
worker.postMessage(21);
worker.onmessage = e => console.log(e.data); // 42
```

✅ Use cases:

* CPU-heavy tasks (image processing, data crunching).
* Parallel execution without blocking UI.

---

### 2. Service Workers

* Act as a **network proxy** between app & server.
* Can intercept requests, serve from cache, enable offline.
* Runs even when page closed (event-driven).
* Must use HTTPS.

```js
// service-worker.js
self.addEventListener("install", e => {
  e.waitUntil(
    caches.open("v1").then(c => c.addAll(["/", "/style.css"]))
  );
});

self.addEventListener("fetch", e => {
  e.respondWith(
    caches.match(e.request).then(r => r || fetch(e.request))
  );
});
```

✅ Use cases:

* Offline apps (PWAs).
* Push notifications.
* Background sync.
* Efficient caching strategies.

---

### Visualization

```
Main Thread
 ├─ Web Worker (parallel tasks, no DOM)
 └─ Service Worker (proxy: intercept fetch, offline, caching)
```

🧠 **Mental Model:**

* **Web Worker** = extra CPU hands.
* **Service Worker** = smart network middleman.

---

### Gotchas ⚠️ (Exhaustive)

1. **Web Workers**

   * No DOM/window access.
   * Communication only via messages (structured cloning).
   * Terminate explicitly with `worker.terminate()`.

2. **Service Workers**

   * Lifecycle: install → activate → fetch.
   * Only one active per scope.
   * Requires HTTPS.
   * Can serve stale cache if not updated correctly.
   * Debugging tricky due to persistence.

3. **Shared Workers**

   * Variant of Web Worker, shared across multiple tabs.

---

### ✅ Pros

* **Web Workers**: true parallelism, responsive UI.
* **Service Workers**: offline-first apps, push notifications, caching control.

---

### ❌ Cons

* **Web Workers**: no DOM access, overhead of message passing.
* **Service Workers**: complex lifecycle, must manage cache carefully.

---

### Best Practices

* ✅ Use Web Workers for CPU-heavy logic, not for trivial tasks.
* ✅ Keep Service Worker small → avoid long install/activate times.
* ✅ Always implement cache-busting & update strategy.
* ✅ Use HTTPS for Service Workers (mandatory).
* ✅ Test carefully in DevTools → unregister old workers.

---

👉 Mnemonic:
**“Web Worker = Work. Service Worker = Network.”**

---

⚡ Would you like me to prepare a **Cheat Card on PWA (Progressive Web App) Essentials** — since Service Workers are the backbone of PWAs and a hot interview area?

