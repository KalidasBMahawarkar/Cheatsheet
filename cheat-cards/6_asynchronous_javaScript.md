Excellent choice 👍 — **Asynchronous JavaScript** is a core interview topic and a real-world survival skill for backend/frontend work.
Here’s your **Concept Mastery Cheat Sheet** in the **improved “best-ever” format** we set.

---

# ⏳ Asynchronous JavaScript — Cheat Card

**Concept:**
JavaScript is **single-threaded** with a **non-blocking concurrency model**.
Async behavior is achieved using:

* **Event loop** + **Callback Queue** + **Microtask Queue**.
* APIs like **callbacks, promises, async/await, timers, events, fetch, I/O**.

---

## 1. Key Mechanisms

### **1.1 Callbacks**

* Functions passed as arguments to run later.

```js
setTimeout(() => console.log("done"), 1000);
```

### **1.2 Promises**

* Represent a value that may resolve or reject in future.

```js
fetch("/data")
  .then(res => res.json())
  .then(console.log)
  .catch(console.error);
```

### **1.3 Async/Await**

* Syntactic sugar over promises.

```js
async function getData() {
  try {
    const res = await fetch("/data");
    return await res.json();
  } catch(e) { console.error(e); }
}
```

### **1.4 Event Loop & Queues**

* **Call Stack** executes sync code.
* **Web APIs/Node APIs** handle async tasks.
* **Task Queue (macrotasks)**: `setTimeout`, `setInterval`, I/O.
* **Microtask Queue**: `Promise.then`, `queueMicrotask`, `MutationObserver`.
* Event loop prioritizes **microtasks > macrotasks** after stack clears.

---

## 2. Visualization

```mermaid
graph TD
A[Call Stack] -->|async call| B[Web API]
B --> C[Task Queue (macrotask)]
B --> D[Microtask Queue]
C -->|event loop picks| A
D -->|higher priority| A
```

🧠 **Mental Model:**

1. Sync → runs immediately.
2. Async → offloaded to API.
3. When done → queued.
4. Event loop pulls → executes when stack empty.

---

## 3. Gotchas ⚠️ (Exhaustive)

1. **Blocking code**

   * Long loops/blocking I/O freeze UI despite async features.

2. **Microtasks vs Macrotasks**

   * Promises run before `setTimeout`.

   ```js
   setTimeout(() => console.log("timeout"), 0);
   Promise.resolve().then(() => console.log("promise"));
   // promise → timeout
   ```

3. **Callback Hell**

   * Nested callbacks → unreadable. Use promises/async-await.

4. **Error handling**

   * Callbacks → must handle manually.
   * Promises → `.catch()` needed.
   * Async/await → try/catch.

5. **Uncaught Promise Rejections**

   * Without `.catch()` → crash in Node (since v15).

6. **Promise chaining traps**

   * Forgetting `return` inside `.then()` → breaks chain.

7. **Async functions always return a promise**

   ```js
   async function f(){ return 42; }
   f().then(console.log); // 42
   ```

8. **Parallel vs Sequential with await**

   * Sequential:

     ```js
     await task1(); 
     await task2();
     ```
   * Parallel:

     ```js
     await Promise.all([task1(), task2()]);
     ```

9. **Event Loop differences**

   * Browser vs Node.js have different task queue priorities.
   * Node.js microtask queue drains **between macrotasks** too.

10. **Timer inaccuracies**

    * `setTimeout(fn, 0)` doesn’t run immediately → queued.
    * Delay depends on event loop state.

11. **Memory leaks**

    * Unresolved promises hold references → GC blocked.

---

## 4. Best Practices ✅

* Use **promises/async-await** instead of raw callbacks.
* Always add **`.catch()`** or `try/catch`.
* Use **Promise.all / Promise.allSettled** for parallel tasks.
* Keep async functions small and predictable.
* Avoid blocking loops in main thread (use workers if heavy).
* Prefer **microtask scheduling** (`queueMicrotask`) for critical defers.

---

## 5. Mnemonic 🔑

**“Stack → API → Queue → Loop.”**

* Sync runs on stack.
* Async handed to APIs.
* APIs queue results.
* Event loop pushes back to stack.

---

👉 Next natural topic after this is **Event Loop & Task Queues Cheat Card** — going deeper into microtask vs macrotask behavior.

Do you want me to prepare that as the follow-up?


Perfect 👌 — let’s upgrade the cheat sheet style itself so every card we make in this window feels **interview-crushing, exam-ready, and visually superior** to even the polished version you showed me.

Here’s how I’ll **improve the Call Stack Cheat Card** right now:

* Add **layered breakdowns** (Concept → Flow → Example → Visualization → Gotchas → Best Practices).
* Use **annotated examples** (comments showing stack state).
* Use **ASCII diagrams** that grow/shrink to simulate stack.
* Add **real-world connections** (e.g., recursion in parsing, async with Event Loop).
* Keep **mnemonic sharper & catchier**.

---

# 🗂️ JavaScript Call Stack — Ultimate Cheat Card

**Concept:**
The **Call Stack** is a LIFO structure that tracks active **Execution Contexts**.

* Each function call pushes a new frame.
* Returning pops the frame.
* Only one stack → JavaScript is **single-threaded**.

---

### Flow of Execution

1. **Start** → Global Execution Context pushed.
2. **Function call** → New Execution Context pushed.
3. **Return** → Pop the frame.
4. **If empty** → Event Loop pulls next async task.

---

### Example (with stack state)

```js
function a(){ console.log("A"); b(); }
function b(){ console.log("B"); c(); }
function c(){ console.log("C"); }

a();
```

**Step by step (stack):**

```
Call a()
[ a ]                // push a
[ Global ]           // global always at base

a() calls b()
[ b ]
[ a ]
[ Global ]

b() calls c()
[ c ]
[ b ]
[ a ]
[ Global ]

c() returns
[ b ]
[ a ]
[ Global ]

b() returns
[ a ]
[ Global ]

a() returns
[ Global ]           // only global left
```

---

### Visualization

```
Top ──► [c()]        <- executing
        [b()]
        [a()]
        [Global EC]
```

Each return pops the top until only global remains.

---

### Gotchas ⚠️ (Exhaustive)

1. **Stack Overflow**

   ```js
   function recurse(){ recurse(); }
   recurse(); // RangeError: Maximum call stack size exceeded
   ```

2. **Recursion Depth Limits**

   * Engine-specific (V8 ~10k calls).

3. **Async Functions & Callbacks**

   * Don’t stay on stack; resumed later via Event Loop.

4. **try/finally Behavior**

   * Context not popped until finally executes fully.

5. **Tail Call Optimization (TCO)**

   * Spec includes, but not widely implemented.

6. **Error Stack Traces**

   * Built from call stack frames; very deep stacks = costly.

7. **Single Thread Bottleneck**

   * Heavy sync tasks block entire stack → async tasks & UI freeze.

8. **Optimizations**

   * Engines may inline functions or collapse tail calls invisibly.

---

### Best Practices

* ✅ Keep functions small to avoid deep nesting.
* ✅ Replace deep recursion with loops/iterators where safer.
* ✅ Don’t block with heavy sync tasks → split into async chunks.
* ✅ Use stack traces for debugging, but avoid expensive deep trace logging.
* ✅ For heavy computation → consider Web Workers/Worker Threads.

---

👉 Mnemonic:
**“Push, Run, Pop, Repeat.”**
That’s the life of the Call Stack.

---

⚡ With **Execution Context → Call Stack → Event Loop**, you now have the **runtime core** locked down.
The natural next “superior” step is **Closures Cheat Card** (because they combine **scope chain + async memory retention**).

Do you want me to prepare **Closures** in this **upgraded format** next?


Perfect 👌 — now let’s build the **Macrotasks vs Microtasks Cheat Card** in the **superior style** we’re using: precise, visual, with exhaustive gotchas grouped.

---

# 🕒 JavaScript Macrotasks vs Microtasks — Cheat Card

**Concept:**
The Event Loop processes **two main task queues**:

* **Macrotasks (task queue)** → big chunks (timers, events, I/O).
* **Microtasks (job queue)** → smaller, high-priority callbacks (promises, `queueMicrotask`).

📌 **Rule:** After each synchronous execution + macrotask, the engine **empties the microtask queue completely** before moving to the next macrotask.

---

### Flow

1. Execute all **sync code** on the call stack.
2. Process all **microtasks** (priority).
3. Run the **next macrotask**.
4. Repeat cycle.

---

### Examples

```js
console.log("A");

setTimeout(()=>console.log("B"), 0);   // macrotask
Promise.resolve().then(()=>console.log("C")); // microtask

console.log("D");
```

**Output:** A → D → C → B
*Sync → Micro → Macro*

---

### Visualization

```
Call Stack → executes sync
   ↓
Microtask Queue → drain fully (Promises, queueMicrotask, process.nextTick)
   ↓
Macrotask Queue → next task (setTimeout, setInterval, I/O, events)
   ↓
Event Loop repeats
```

🧠 **Mental Model:**

* **Microtasks = urgent jobs** (run *before* next render).
* **Macrotasks = scheduled jobs** (run *on next tick*).

---

### Common Sources

**Microtasks:**

* `Promise.then/catch/finally`
* `queueMicrotask()`
* `MutationObserver`
* Node.js: `process.nextTick` (runs *before* other microtasks)

**Macrotasks:**

* `setTimeout`, `setInterval`
* `setImmediate` (Node.js)
* Script execution (top-level code)
* I/O callbacks
* UI events (click, input, etc.)

---

### Gotchas ⚠️ (Exhaustive)

1. **Order guarantee**

   * Microtasks always run before next macrotask, even if macrotask has 0ms delay.

2. **Starvation risk**

   * Recursive microtasks can block macrotasks forever:

   ```js
   function loop(){ Promise.resolve().then(loop); }
   loop(); // starves timers/events
   ```

3. **Promise vs Timeout confusion**

   * Promises beat setTimeout always.

4. **Node.js differences**

   * `process.nextTick` runs **before** microtasks.
   * Event loop phases: timers → pending → poll → check → close.

5. **Top-level await (ES2022)**

   * Resolved like a microtask, not macrotask.

6. **Rendering timing**

   * Browsers usually render **after microtasks, before macrotasks**.

7. **Timer clamping**

   * In browsers, `setTimeout(fn, 0)` → actually ≥ 4ms delay.

8. **Async/await**

   * `await` splits function → rest runs as a microtask.

---

### Best Practices

* ✅ Use **microtasks** (promises) for short, immediate follow-ups.
* ✅ Use **macrotasks** (timers, setImmediate) for scheduling after rendering or I/O.
* ✅ Avoid recursive microtasks → risk of starvation.
* ✅ In Node.js, prefer `setImmediate` over `setTimeout(fn,0)` for clarity.
* ✅ Profile event loop behavior when debugging perf issues.

---

👉 Mnemonic:
**“Micro now, Macro later.”**

* Microtasks run **before** the next macrotask tick.

---

⚡ The next deep dive could be **Node.js Event Loop Phases Cheat Card** (timers, poll, check, close), since many interviews test that difference from the browser loop.

Do you want me to prepare that next?


Alright — let’s lock in the **Task Queue vs Job Queue Cheat Card** in the same **superior style** we’ve been refining.

---

# 📥 JavaScript Task Queue vs Job Queue — Cheat Card

**Concept:**
JavaScript runtimes manage async execution with **two queues**:

* **Task Queue (Macrotask Queue)** → holds big tasks (timers, events, I/O).
* **Job Queue (Microtask Queue)** → holds smaller, higher-priority jobs (promises, microtasks).

📌 **Rule:** After sync code & each macrotask, the engine **empties the job queue fully** before moving to the next macrotask.

---

### Flow

1. **Sync code** → executes on call stack.
2. **Tasks (macrotasks)** → queued when async APIs finish.
3. **Jobs (microtasks)** → queued immediately (promises, queueMicrotask).
4. **Event Loop cycle**:

   * Drain job queue (all microtasks).
   * Then pick next task.
   * Repeat.

---

### Examples

```js
console.log("1");

setTimeout(()=>console.log("2"), 0);   // Task
Promise.resolve().then(()=>console.log("3")); // Job

console.log("4");
```

**Output:** 1 → 4 → 3 → 2
(*Sync → Jobs → Tasks*)

---

### Visualization

```
Call Stack → runs sync
   ↓
Job Queue (microtasks) → drain fully (Promise, queueMicrotask)
   ↓
Task Queue (macrotasks) → timers, I/O, UI events
   ↓
Event Loop repeats
```

🧠 **Mental Model:**

* **Job Queue = urgent work** → cleared immediately.
* **Task Queue = scheduled work** → next tick.

---

### Common Sources

**Job Queue (Microtasks):**

* Promises (`.then`, `.catch`, `.finally`)
* `queueMicrotask()`
* `MutationObserver`
* Node.js: `process.nextTick` (runs *before* other jobs)

**Task Queue (Macrotasks):**

* `setTimeout`, `setInterval`
* `setImmediate` (Node.js)
* I/O events
* DOM events (click, input, etc.)
* Script execution (top-level code)

---

### Gotchas ⚠️ (Exhaustive)

1. **Jobs outrank tasks**

   * Promise callbacks run before timers, even with 0ms delay.

2. **Starvation**

   * Recursive microtasks can block macrotasks:

   ```js
   function loop(){ queueMicrotask(loop); }
   loop(); // starves Task Queue forever
   ```

3. **Node.js twist**

   * `process.nextTick` runs *before* other microtasks, giving it ultra-high priority.

4. **await = job**

   * Async/await resumes as a job (microtask).

5. **Rendering placement**

   * Browsers typically render after jobs, before next task.

6. **Timer clamping**

   * `setTimeout(fn,0)` has minimum delay (≥4ms in browsers).

7. **Multiple queues**

   * One task queue, but multiple job queues possible internally (spec vs engine details).

8. **Order within queues**

   * Both queues follow FIFO.
   * Job queue drained fully each cycle, task queue advances one task at a time.

---

### Best Practices ✅

* ✅ Use **Promises/async-await** for critical short async flows (jobs).
* ✅ Use **setTimeout/setImmediate** for deferring heavy work (tasks).
* ✅ Don’t recurse microtasks endlessly → risk starving tasks & rendering.
* ✅ In Node, use `setImmediate` instead of `setTimeout(fn,0)` for clarity.
* ✅ Profile queues with browser DevTools (`Performance`) or Node async hooks.

---

👉 Mnemonic:
**“Jobs now, Tasks later.”**

* Jobs (microtasks) always flush before the next task tick.

---

⚡ The natural next deep dive: **Node.js Event Loop Phases Cheat Card** (timers → I/O → check → close), where Task vs Job scheduling gets real-world twists.

Do you want me to prepare that one next?


Perfect 👌 — let’s make this **Callbacks Cheat Card** even more **superior** by adding a dedicated **Pros & Cons** section (something you can use in interviews to compare patterns).

---

# 📞 JavaScript Callbacks — Cheat Card

**Concept:**
A **callback** is a function passed into another function to be executed later (sync or async).

* Fundamental async mechanism before Promises/async-await.

---

### Flow

1. Caller passes function.
2. Receiver stores callback.
3. Callback executed when condition/event completes.

---

### Examples

**Sync Callback**

```js
function greet(name, cb) {
  console.log("Hello", name);
  cb();
}
greet("Kalidas", () => console.log("Done!"));
```

**Async Callback**

```js
setTimeout(() => console.log("Task complete"), 1000);
```

**Node-style (Error-first)**

```js
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

---

### Visualization

```
Caller → passes callback
   ↓
Receiver → holds callback
   ↓
Event done → callback executed
```

---

### Gotchas ⚠️ (Exhaustive)

1. Callback Hell (nested pyramid).
2. Error handling inconsistent (sync vs async).
3. Inversion of Control (you give control away).
4. Can be called multiple times → bugs.
5. May never be called → hanging program.
6. `this` context often lost.
7. Async execution order confusion.
8. Mixing with Promises awkward (need promisify).

---

### ✅ Pros

* Simple concept, easy to implement.
* Flexible: works for sync and async.
* Direct control of timing.
* Supported everywhere (even pre-ES6).
* Still used in performance-critical or legacy Node APIs.

---

### ❌ Cons

* Callback Hell → nested unreadable code.
* Error handling clumsy, split between try/catch and error-first args.
* Harder to compose/control flow (parallel, series, race).
* Inversion of control risk (lib decides when/if to call).
* Debugging stack traces messy.
* Not cancelable once scheduled.

---

### Best Practices

* ✅ Use Promises/async-await for async workflows.
* ✅ Stick to error-first `(err, data)` convention in Node.
* ✅ Ensure callback is called exactly once.
* ✅ Use arrow functions to avoid `this` binding loss.
* ✅ Modularize to flatten nesting.
* ✅ Convert legacy APIs with `util.promisify`.

---

👉 Mnemonic:
**“Call me back later.”** — that’s all a callback is.

---

⚡ Next step: **Promises Cheat Card** — with pros/cons, examples, gotchas, and comparison to callbacks.

Do you want me to create the **Promises Cheat Card** next in this same upgraded format?


Perfect 👌 — let’s rebuild the **Promises Cheat Card** and make it **truly superior** by adding a **Methods section** (with concise explanations), while keeping everything in our refined format.

---

# 🤝 JavaScript Promises — Cheat Card

**Concept:**
A **Promise** is an object representing the eventual completion (or failure) of an async operation.

* States: **pending → fulfilled OR rejected**.
* Once settled, state/value is immutable.

---

### Flow

1. **Create** promise (executor runs immediately).
2. **Resolve/Reject** settles it.
3. **Consume** with `.then()`, `.catch()`, `.finally()`.
4. Runs via **microtask (job) queue**.

---

### Examples

```js
// Basic
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("done"), 1000);
});

p.then(val => console.log(val))
 .catch(err => console.error(err))
 .finally(() => console.log("finished"));
```

```js
// Chaining
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => x * 2)
  .then(console.log); // 4
```

```js
// Parallel
Promise.all([p1, p2, p3])
  .then(results => console.log(results))
  .catch(err => console.error(err));
```

---

### Visualization

```
Pending ----resolve---> Fulfilled
   |                         |
   ----reject--------------> Rejected
```

🧠 Promise = "IOU for a future value"
Always async → callbacks in microtask queue.

---

### Methods

* **then(onFulfilled, onRejected?)** → chain async results.
* **catch(onRejected)** → handle errors.
* **finally(cb)** → always runs (cleanup).
* **Promise.all([...])** → all must fulfill, else reject.
* **Promise.allSettled([...])** → wait all, return statuses.
* **Promise.race([...])** → first settled wins.
* **Promise.any([...])** (ES2021) → first fulfilled wins, reject if all fail.
* **Promise.resolve(val)** → immediately fulfilled.
* **Promise.reject(err)** → immediately rejected.

---

### Gotchas ⚠️ (Exhaustive)

1. **Uncaught rejections** → crash in Node ≥ v15.
2. **Chaining errors** → forgetting `return` breaks chain.
3. **Multiple resolutions ignored** → only first counts.
4. **Errors bubble** → thrown in `.then` go to `.catch`.
5. **finally** → doesn’t change result, passes value/error through.
6. **Promises are eager** → executor runs instantly.
7. **Promise.all short-circuits** → one rejection cancels all.
8. **Promise.any** → AggregateError if all reject.
9. **Promise.race** → resolves/rejects with *first settled*.
10. **Top-level await** → resolved like a promise (microtask).

---

### ✅ Pros

* Cleaner async flow than callbacks.
* Built-in error propagation.
* Avoids callback hell via chaining.
* Concurrency utilities (`all`, `race`, etc.).
* Always async → predictable scheduling.

---

### ❌ Cons

* Still verbose for linear code.
* Easy to forget returns in chains.
* Debugging stack traces harder.
* Memory leaks if unresolved promises linger.
* Not inherently cancelable (needs AbortController).

---

### Best Practices

* ✅ Always end with `.catch()` (or use `try/catch` in async/await).
* ✅ Use `Promise.allSettled` when failures shouldn’t short-circuit.
* ✅ Modularize chains to avoid nesting.
* ✅ Prefer async/await for sequential logic.
* ✅ Use `Promise.all`/`any` for concurrency.
* ✅ Use `AbortController` for cancellation.

---

👉 Mnemonic:
**“Pending → Done or Fail — once, forever.”**

---

⚡ Next up: **Async/Await Cheat Card** — syntactic sugar over Promises, solves readability & chaining pain.

Do you want me to create the **Async/Await Cheat Card** next in this same superior format (with methods, pros/cons, gotchas)?

Perfect — let’s zoom in on the **Promise States** part and give it its own **cheat card style treatment**, since this is a common interview checkpoint.

---

# 🔄 Promise States — Cheat Card

**Concept:**
A Promise is always in **exactly one of three states**:

* **pending** → operation not complete yet.
* **fulfilled** → operation finished successfully.
* **rejected** → operation failed.

Once **settled** (fulfilled/rejected), a promise’s state/value is immutable.

---

### States

1. **Pending**

   * Initial state.
   * Waiting for `resolve` or `reject`.
   * Value unknown.

   ```js
   const p = new Promise(()=>{}); // stays pending forever
   ```

2. **Fulfilled**

   * Completed successfully with a value.
   * Triggers `.then()` callbacks.

   ```js
   Promise.resolve("OK")
     .then(v => console.log(v)); // "OK"
   ```

3. **Rejected**

   * Completed with an error/reason.
   * Triggers `.catch()` callbacks.

   ```js
   Promise.reject("Error")
     .catch(e => console.error(e)); // "Error"
   ```

---

### Visualization

```
 Pending
   │
 resolve ───► Fulfilled
 reject  ───► Rejected
```

🧠 **Key Rule:** Once settled, can’t change again.

---

### Gotchas ⚠️ (Exhaustive)

1. **Multiple resolves** → only the first counts.

   ```js
   new Promise(res => { res(1); res(2); }) // stays 1
   ```

2. **Unhandled rejections** → crash in Node ≥ v15.

3. **Errors bubble** → throw inside `.then()` → rejection.

4. **Promise.resolve vs new Promise**

   * `Promise.resolve(val)` → immediately fulfilled.
   * `new Promise` → stays pending until `resolve` called.

5. **Async/await link**

   * `await` unwraps fulfilled value or throws rejection.

6. **Never-settled promise**

   * If neither resolve nor reject → pending forever.

---

### ✅ Pros of State Model

* Clear lifecycle → predictable async flow.
* Guarantees immutability once settled.
* Built-in error propagation.

---

### ❌ Cons

* Pending promises can “leak” memory if never resolved.
* No built-in cancellation (fixed via AbortController in APIs).

---

👉 Mnemonic:
**“Start Pending → End Happy (fulfilled) or Sad (rejected).”**

---

⚡ Would you like me to now expand this into a **Promises Lifecycle Cheat Card** (states + methods + chaining + async/await) so you get one master view of promises from creation → consumption → settlement?


Perfect 👌 — let’s **upgrade** the **Promise Chaining Cheat Card** to be *interview-crushing*, adding explicit sections like **Error Propagation**, **Return Behavior**, and **Control Flow patterns**. That way you’ll always sound crystal clear in interviews.

---

# ⛓️ JavaScript Promise Chaining — Cheat Card

**Concept:**
**Chaining** = sequencing async operations by returning values/promises inside `.then()`.

* Each `.then()` returns a **new promise**, enabling flow control and error handling.
* Errors bubble down until caught.

---

### Flow

1. Start with a resolved/rejected promise.
2. Attach `.then()` → returns a new promise.
3. Values auto-wrapped → `Promise.resolve(val)`.
4. Errors propagate to next `.catch()`.
5. `.finally()` runs always, passes original value/error through.

---

### Example

```js
Promise.resolve(1)
  .then(x => x + 1)        // returns 2
  .then(x => x * 2)        // returns 4
  .then(console.log)       // 4
  .catch(console.error)
  .finally(() => console.log("done"));
```

---

### Error Propagation

```js
Promise.resolve(10)
  .then(x => { throw "fail"; })
  .catch(err => console.log(err)) // "fail"
  .then(() => console.log("next")); // continues
```

📌 **Rule:** Errors jump to the next `.catch()`; chain continues afterward.

---

### Return Behavior

```js
// Returning value
Promise.resolve(5)
  .then(x => x + 1)     // auto-wrapped: Promise.resolve(6)
  .then(console.log);   // 6

// Returning promise
Promise.resolve(5)
  .then(x => Promise.resolve(x * 2))
  .then(console.log);   // 10
```

📌 **Rule:** Values → wrapped. Promises → awaited.

---

### Control Flow Patterns

**Sequential**

```js
Promise.resolve()
  .then(task1)
  .then(task2)
  .then(task3);
```

**Parallel**

```js
Promise.all([task1(), task2(), task3()])
  .then(results => console.log(results));
```

**Race**

```js
Promise.race([p1, p2]).then(console.log);
```

---

### Methods in Chains

* **then(onFulfilled, onRejected?)** → handle value/error, return new promise.
* **catch(onRejected)** → handle rejection.
* **finally(cb)** → always runs, doesn’t alter value/error.

---

### Gotchas ⚠️ (Exhaustive)

1. Forgetting `return` → chain breaks.
2. Errors bubble until `.catch()`.
3. Unhandled rejections → crash in Node ≥ v15.
4. Multiple resolves → only first counts.
5. `.finally()` runs but doesn’t modify chain value.
6. Nesting chains → don’t; always return.
7. Async loops → may create unintended serial vs parallel execution.

---

### ✅ Pros

* Sequential async flows readable.
* Built-in error propagation.
* Avoids callback hell.
* Auto-wrapping saves boilerplate.

---

### ❌ Cons

* Verbose for linear code.
* Easy to miss `return`.
* Debugging long chains hard.
* Still less intuitive than async/await.

---

### Best Practices

* ✅ Always `return` in `.then()` when transforming values.
* ✅ End chains with `.catch()`.
* ✅ Use `.finally()` for cleanup tasks.
* ✅ Modularize → small functions inside chains.
* ✅ Use `Promise.all/any` for parallel work.

---

👉 Mnemonic:
**“Return or Burn — Errors Flow Down.”**

* Return values/promises to keep chain alive.
* Errors bubble until caught.

---

⚡ Next natural card: **Async/Await Cheat Sheet** — to show how chaining translates into cleaner syntax (but same rules underneath).

Would you like me to now prepare the **Async/Await Cheat Card** in this **superior format**?


Perfect 👌 — let’s tackle **Error Handling in JavaScript Promises** in the same **superior, interview-ready format**. This card will highlight **how errors propagate, patterns to handle them, gotchas, pros/cons, and best practices**.

---

# ⚠️ JavaScript Promise Error Handling — Cheat Card

**Concept:**
Errors in Promises **bubble down the chain** until a `.catch()` handles them.

* A rejected promise or a thrown error in `.then()` → passes to the nearest `.catch()`.
* After handling, chain **continues** with resolved state.

---

### Flow

1. **Throw or reject** → error propagates down.
2. **First .catch()** → intercepts, handles error.
3. Chain resumes as fulfilled (unless re-thrown).
4. `.finally()` always runs but does not modify value/error.

---

### Examples

**Basic Catch**

```js
Promise.reject("fail")
  .catch(err => console.log("Caught:", err)); // Caught: fail
```

**Error Propagation**

```js
Promise.resolve(10)
  .then(x => { throw "boom"; })
  .catch(err => console.log("Caught:", err)) // "boom"
  .then(() => console.log("Continues"));     // Continues
```

**Re-throwing**

```js
Promise.reject("bad")
  .catch(err => { throw "worse"; })
  .catch(err => console.log(err)); // "worse"
```

**Finally Behavior**

```js
Promise.reject("oops")
  .finally(() => console.log("cleanup")) // cleanup
  .catch(console.error);                 // oops
```

---

### Visualization

```
.then() → throw/reject → ↓
                  nearest .catch()
                         ↓
        continues as resolved (unless rethrow)
```

🧠 **Key Rule:** Error handling is like a **try/catch** that stretches across async chains.

---

### Gotchas ⚠️ (Exhaustive)

1. **No .catch() → Unhandled Rejection**

   * In Node ≥ v15 → crashes process.
   * In browsers → console warning.

2. **finally doesn’t modify result**

   * Cleanup only, value/error flows through.

3. **Return in catch**

   * Returning resolves chain with that value.

4. **Throw in catch**

   * Turns chain back into rejected state.

5. **Synchronous errors auto-wrap**

   ```js
   new Promise(() => { throw "error"; })
     .catch(console.log); // "error"
   ```

6. **Async/await link**

   * `await` errors = promise rejections.
   * Must wrap in `try/catch`.

7. **Multiple catches**

   * First one that handles stops bubbling, unless re-throw.

8. **Parallel promises**

   * `Promise.all` → one rejection cancels all (short-circuits).
   * Use `allSettled` for independent error handling.

---

### ✅ Pros

* Built-in, chain-wide error propagation.
* Cleaner than manual try/catch in callbacks.
* Works across sync + async seamlessly.

---

### ❌ Cons

* Easy to miss `.catch()`.
* Debugging async stack traces harder.
* `Promise.all` fails fast → may not see all results.

---

### Best Practices

* ✅ Always add `.catch()` or wrap with `try/catch` (async/await).
* ✅ Use `.finally()` for cleanup (close DB, stop spinner).
* ✅ Prefer `Promise.allSettled` when all results matter.
* ✅ Modularize error handling for clearer logic.
* ✅ In Node, listen for `unhandledRejection` events in critical apps.

---

👉 Mnemonic:
**“Throw → Bubble → Catch → Continue.”**

* Errors travel down until caught.
* After catch, chain goes on.

---

⚡ Next natural step: **Async/Await Error Handling Cheat Card** (wrapping promises in `try/catch/finally`).

Would you like me to prepare that as the **next card**?


Excellent 👌 — this one is **core interview material**. Let’s build a **Cheat Card** comparing **`Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any`** in our **superior, interview-ready format**.

---

# 🏁 JavaScript Promise Combinators — Cheat Card

**Concept:**
Combinator methods let you handle **multiple promises together**.
Each has different **completion rules** and **error behavior**.

---

### Methods

1. **`Promise.all(iterable)`**

   * Resolves when **all promises fulfill**.
   * Rejects immediately if **any reject** (fail-fast).

   ```js
   Promise.all([p1, p2, p3])
     .then(results => console.log(results)) // [v1, v2, v3]
     .catch(err => console.error(err));
   ```

2. **`Promise.allSettled(iterable)`** (ES2020)

   * Resolves when **all settle** (fulfilled or rejected).
   * Never rejects; gives array of `{status, value|reason}`.

   ```js
   Promise.allSettled([p1, p2])
     .then(results => console.log(results));
   // [{status:"fulfilled", value:…}, {status:"rejected", reason:…}]
   ```

3. **`Promise.race(iterable)`**

   * Resolves/rejects with the **first promise that settles**.
   * Doesn’t wait for others.

   ```js
   Promise.race([p1, p2])
     .then(val => console.log("Winner:", val))
     .catch(err => console.error("First fail:", err));
   ```

4. **`Promise.any(iterable)`** (ES2021)

   * Resolves with the **first fulfilled** promise.
   * Rejects only if **all reject**, with `AggregateError`.

   ```js
   Promise.any([p1, p2])
     .then(val => console.log("First success:", val))
     .catch(err => console.error(err)); // AggregateError if all fail
   ```

---

### Visualization

```
all        → wait all, fail-fast on first reject
allSettled → wait all, collect results (status/value/reason)
race       → settle with first one (success OR failure)
any        → first success wins; all fail → AggregateError
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Promise.all**

   * Rejects immediately if one fails → no other results.
   * May cancel side-effects unexpectedly.

2. **Promise.allSettled**

   * No short-circuit → always waits for all.
   * Useful for cleanup tasks or bulk results.

3. **Promise.race**

   * First to settle decides → even if it rejects.
   * Can be used for timeouts with `Promise.race([task, timeout])`.

4. **Promise.any**

   * Rejections ignored unless *all fail*.
   * Throws `AggregateError` (unique to `any`).

5. **Empty iterable**

   * `Promise.all([])` → resolves to `[]`.
   * `Promise.allSettled([])` → resolves to `[]`.
   * `Promise.race([])` → never settles.
   * `Promise.any([])` → rejects immediately with `AggregateError`.

6. **Parallel vs Sequential**

   * These methods run promises in **parallel**.
   * For sequential execution → use chaining or loops with `await`.

---

### ✅ Pros

* Manage multiple async tasks easily.
* Fail-fast (`all`) or fail-safe (`allSettled`).
* Handle races and first-wins scenarios (`race`, `any`).

---

### ❌ Cons

* `all` can lose partial results if one rejects.
* `any` less widely supported in older environments.
* Harder debugging with multiple rejections.

---

### Best Practices

* ✅ Use `Promise.all` when all must succeed.
* ✅ Use `Promise.allSettled` when all results matter, errors included.
* ✅ Use `Promise.race` for timeouts & competitive tasks.
* ✅ Use `Promise.any` when you only need one success.
* ✅ Handle `AggregateError` properly in `any`.

---

👉 Mnemonic:
**“All succeed, Settled collects, Race is fastest, Any is first success.”**

---

⚡ Next natural cheat card: **Async/Await** — showing how to use these combinators cleanly with `await`.

Would you like me to create the **Async/Await Cheat Card** next in this format?


Here’s your **Concept Mastery Cheat Sheet** for **`async/await` in JavaScript** — in the same **superior, interview-ready format** we’ve been building.

---

# ⏳ JavaScript `async/await` — Cheat Card

**Concept:**
`async/await` is **syntactic sugar** over Promises that makes async code look synchronous.

* `async` → function always returns a Promise.
* `await` → pauses execution until promise settles.

---

### Flow

1. **Mark function with `async`** → it returns a Promise.
2. **Use `await` inside async function** → unwraps the fulfilled value or throws rejection.
3. Execution “pauses” at `await` but doesn’t block the event loop.

---

### Examples

**Basic**

```js
async function fetchData() {
  return "Hello";
}
fetchData().then(console.log); // Hello
```

**Awaiting a Promise**

```js
async function example() {
  const res = await Promise.resolve(42);
  console.log(res); // 42
}
example();
```

**Error Handling**

```js
async function risky() {
  try {
    const data = await Promise.reject("fail");
  } catch (err) {
    console.error(err); // "fail"
  } finally {
    console.log("cleanup");
  }
}
```

**Sequential vs Parallel**

```js
// Sequential
const a = await task1();
const b = await task2();

// Parallel
const [a, b] = await Promise.all([task1(), task2()]);
```

---

### Visualization

```
async fn → always returns Promise
   │
   ├─ return value → wrapped as resolved Promise
   └─ throw error → wrapped as rejected Promise
await promise → pause until settled
```

🧠 **Mental Model:**

* `await` = "unbox this promise value".
* Think of `try/catch` as async `.then/.catch`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Top-level await**

   * Allowed only in ES modules (ES2022+).

2. **Parallelism trap**

   ```js
   // BAD: sequential
   await task1();
   await task2();
   // GOOD: parallel
   await Promise.all([task1(), task2()]);
   ```

3. **Error propagation**

   * Uncaught error in async fn → rejected promise.

4. **Blocking inside loops**

   * Using `await` in `for` loops serializes tasks unintentionally.

5. **Mixing with .then**

   * Still works, but messy; stick to one style.

6. **Return types**

   * Async functions always return Promise, even if returning value.

7. **Unhandled rejection risk**

   * If you forget `try/catch` or `.catch()` → crash in Node ≥ v15.

---

### ✅ Pros

* Clean, synchronous-like syntax.
* Easier error handling (`try/catch`).
* Great for sequential async flows.
* Plays well with Promise utilities (`all`, `race`, `any`).

---

### ❌ Cons

* Must be inside `async` function (except top-level in modules).
* Naive usage can cause sequential performance bottlenecks.
* Debugging async stack traces still tricky.

---

### Best Practices

* ✅ Use `try/catch/finally` for safe error handling.
* ✅ Combine with `Promise.all` for parallel tasks.
* ✅ Don’t `await` inside loops unless order matters.
* ✅ Keep async functions small and composable.
* ✅ Always return values (implicitly wrapped in Promise).

---

👉 Mnemonic:
**“async always returns a Promise; await unwraps it.”**

---

⚡ Next logical step: **Top-level Await & Advanced Patterns Cheat Card** (covering dynamic imports, module loading, parallel execution strategies).

Would you like me to create that as the **next card**, or go deeper into **Async/Await Error Handling** (specialized cheat card)?


Got it ✅ — let’s now create a **Cheat Card on Error Handling with `try/catch` in Async/Await**, since this is *the* interview sweet spot after promises and chaining.

---

# 🛡️ Error Handling with `try/catch` — Cheat Card

**Concept:**
In `async/await`, errors are handled using **`try/catch/finally`**, similar to synchronous code.

* `await` unwraps a promise → if rejected, the error is **thrown** and caught in `catch`.

---

### Flow

1. Wrap async code in `try`.
2. On rejection or thrown error → jumps to `catch`.
3. `finally` always executes, regardless of outcome.

---

### Examples

**Basic**

```js
async function fetchData() {
  try {
    const data = await Promise.resolve("OK");
    console.log(data); // OK
  } catch (err) {
    console.error("Error:", err);
  } finally {
    console.log("Done");
  }
}
```

**Rejection Handling**

```js
async function risky() {
  try {
    const result = await Promise.reject("fail");
    console.log(result); // never runs
  } catch (err) {
    console.error("Caught:", err); // Caught: fail
  }
}
```

**Parallel Handling**

```js
try {
  const [a, b] = await Promise.all([task1(), task2()]);
} catch (err) {
  console.error("One failed:", err);
}
```

**Granular Handling**

```js
try {
  const user = await getUser();
} catch (err) {
  console.error("User fetch failed");
}

try {
  const orders = await getOrders();
} catch (err) {
  console.error("Orders fetch failed");
}
```

---

### Visualization

```
try {
   await promise   → success → continue
} catch (err) {
   promise reject → thrown → handled here
} finally {
   always runs
}
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Silent failures if unhandled**

   * No try/catch → async function returns rejected promise.

2. **Parallel with Promise.all**

   * One rejection cancels all results. Use `allSettled` to capture all.

3. **Looping traps**

   * Using `await` in loops → sequential execution, not parallel.

4. **Nested try/catch**

   * Harder to read → modularize or split into smaller async functions.

5. **finally doesn’t change outcome**

   * Runs always, passes original result/error through.

6. **Top-level await**

   * Needs try/catch at top scope in modules:

   ```js
   try {
     const data = await fetch("/api");
   } catch (err) {
     console.error(err);
   }
   ```

7. **Error typing**

   * Errors can be anything (`string`, `object`, not always `Error`).

---

### ✅ Pros

* Clean, synchronous-like syntax.
* Unified handling for sync + async errors.
* Easier to reason about vs `.catch()` chains.
* Integrates with `finally` for cleanup.

---

### ❌ Cons

* Naive usage → sequential performance hits.
* Verbose if handling multiple independent async calls.
* Still need Promise combinators for bulk tasks.

---

### Best Practices

* ✅ Always wrap awaited calls that may fail.
* ✅ Use `Promise.allSettled` if you need *all results*, not fail-fast.
* ✅ Split concerns: one try/catch per logical block.
* ✅ Log errors with context → avoid generic `console.error(err)`.
* ✅ Use custom error classes for clarity in large apps.

---

👉 Mnemonic:
**“Await may throw → try/catch always knows.”**

---

⚡ Do you want me to now create the **Top-level Await Cheat Card** (covering ES Modules, dynamic imports, and error handling at the module scope)?


Here’s your **Concept Mastery Cheat Sheet** for **Parallelism with `await Promise.all`** — crisp, visual, and fully interview-ready.

---

# ⚡ Parallelism with `await Promise.all` — Cheat Card

**Concept:**
`await Promise.all([...])` lets you run multiple async tasks **in parallel**, waiting until all settle.

* More efficient than sequential `await`s when tasks are independent.

---

### Flow

1. Start tasks immediately (don’t `await` yet).
2. Pass promises into `Promise.all`.
3. Wait until **all fulfill** → results array (ordered).
4. If any reject → whole thing rejects (fail-fast).

---

### Example

**Sequential (slow)**

```js
async function run() {
  const a = await task1(); // waits
  const b = await task2(); // waits again
  return [a, b];
}
```

**Parallel (fast)**

```js
async function run() {
  const [a, b] = await Promise.all([task1(), task2()]);
  return [a, b];
}
```

**Error Handling**

```js
try {
  const [a, b] = await Promise.all([task1(), task2()]);
} catch (err) {
  console.error("One failed:", err);
}
```

---

### Visualization

```
Sequential → task1 ──finish──> task2 ──finish──> results
Parallel   → task1 + task2 run together ──> wait all ──> results
```

🧠 **Mental Model:** Fire off all tasks, then await them as a group.

---

### Gotchas ⚠️ (Exhaustive)

1. **Fail-fast behavior**

   * If one rejects, whole `Promise.all` rejects.

2. **Lose partial results**

   * Other results discarded if any task fails.

3. **Order preserved**

   * Results follow array order, not finish order.

4. **Start early**

   * Promises inside array must be **created before** `Promise.all`.

5. **Mix with async/await**

   * Don’t write `await task1()` before `Promise.all` → kills parallelism.

6. **Memory impact**

   * All tasks kept alive until all settle → can spike memory usage.

7. **Error aggregation**

   * Only first rejection is caught; others ignored.
   * Use `Promise.allSettled` for full error collection.

---

### ✅ Pros

* Big performance win for independent tasks.
* Clean destructuring syntax.
* Preserves input order for results.

---

### ❌ Cons

* Fail-fast hides other errors.
* Can cause heavy parallel load (API rate limits, DB pressure).
* Memory-intensive with large arrays.

---

### Best Practices

* ✅ Use for **independent async calls** (APIs, DB queries).
* ✅ Wrap in `try/catch` for rejection safety.
* ✅ Use `Promise.allSettled` if all outcomes matter.
* ✅ Consider throttling when running many tasks.
* ✅ Start tasks **before** passing into `Promise.all`.

---

👉 Mnemonic:
**“All succeed or all fail — no in-between.”**

---

⚡ Next logical card: **`Promise.allSettled` Cheat Card** (showing how to handle parallel tasks where failures must be collected, not short-circuit).

Do you want me to prepare that as a separate cheat card next?

Here’s your **Concept Mastery Cheat Sheet** for **`setTimeout`, `setInterval`, `setImmediate`** in JavaScript — complete, compact, and interview-ready.

---

# ⏲️ JavaScript Timers — Cheat Card

**Concept:**
JS provides built-in timer APIs for **scheduling async tasks**.

* `setTimeout` → run once after delay.
* `setInterval` → run repeatedly at interval.
* `setImmediate` (Node.js) → run after current poll phase, before timers.

---

### Methods

**setTimeout(fn, delay, ...args)**

* Executes `fn` **once** after `delay` (ms).
* Returns an ID (use with `clearTimeout`).

```js
const id = setTimeout(() => console.log("Hi"), 1000);
clearTimeout(id); // cancel
```

**setInterval(fn, delay, ...args)**

* Executes `fn` **repeatedly** every `delay` ms.
* Returns an ID (use with `clearInterval`).

```js
const id = setInterval(() => console.log("tick"), 1000);
clearInterval(id); // stop
```

**setImmediate(fn, ...args)** *(Node.js only)*

* Executes `fn` **immediately after I/O events**, before timers.
* Useful to break up blocking tasks.

```js
setImmediate(() => console.log("Runs after current poll phase"));
```

---

### Visualization

```
setTimeout → Task scheduled after delay → Task Queue (macrotask)
setInterval → Task repeated each interval → Task Queue (macrotask)
setImmediate → Task after I/O callbacks → Check phase (Node.js)
```

🧠 **Mental Model:**

* `setTimeout` = “run later once.”
* `setInterval` = “repeat until stopped.”
* `setImmediate` = “next tick after I/O.”

---

### Gotchas ⚠️ (Exhaustive)

1. **Delay inaccuracy**

   * `setTimeout(fn,0)` doesn’t mean immediate → queued after microtasks.
   * Browser clamps min delay to ~4ms.

2. **Blocking code**

   * Timers delayed if call stack busy.

3. **Interval drift**

   * `setInterval` doesn’t guarantee exact timing (accumulates drift).
   * Use recursive `setTimeout` for more accurate intervals.

4. **setImmediate vs setTimeout(fn,0)**

   * In Node.js:

     * `setImmediate` runs after poll phase.
     * `setTimeout(fn,0)` runs in timers phase.
   * Order can differ depending on context.

5. **clear functions**

   * Always clear timers to prevent memory leaks (`clearTimeout`, `clearInterval`, `clearImmediate`).

6. **Arguments passing**

   * Extra args after delay are passed into callback:

   ```js
   setTimeout((a, b) => console.log(a+b), 1000, 2, 3); // 5
   ```

7. **GC references**

   * Active timers keep Node.js event loop alive.

---

### ✅ Pros

* Simple async scheduling mechanism.
* Widely supported (timeout/interval in browsers & Node).
* `setImmediate` gives precise control in Node event loop.

---

### ❌ Cons

* Timing is approximate (affected by stack load).
* `setInterval` prone to drift & overlap.
* Overuse can keep event loop busy → perf issues.
* `setImmediate` is Node-only (not in browsers).

---

### Best Practices

* ✅ Use `setTimeout` for one-off delays.
* ✅ Prefer recursive `setTimeout` over `setInterval` for stable periodic tasks.
* ✅ Always clear timers to prevent leaks.
* ✅ Use `setImmediate` in Node when breaking heavy tasks into chunks.
* ✅ For high precision → use `requestAnimationFrame` (browser) or external schedulers.

---

👉 Mnemonic:
**“Timeout once, Interval repeat, Immediate after I/O.”**

---

⚡ Next step could be a **Cheat Card on `requestAnimationFrame` vs Timers** (browser rendering timing), which interviewers love to compare.

Would you like me to prepare that as the follow-up?


Here’s your **Concept Mastery Cheat Sheet** for **`process.nextTick` in Node.js** — polished, dense, and interview-ready.

---

# ⏭️ Node.js `process.nextTick` — Cheat Card

**Concept:**
`process.nextTick(cb)` schedules a callback to run **immediately after the current operation**, **before the event loop continues** to the next phase.

* Belongs to the **microtask queue**, but with even **higher priority** than Promise callbacks.

---

### Flow

1. Current operation finishes (call stack clears).
2. All `nextTick` callbacks run **before** any microtasks (Promises).
3. Event loop then continues to next phase (timers, I/O, etc.).

---

### Example

```js
console.log("start");

process.nextTick(() => console.log("nextTick"));
Promise.resolve().then(() => console.log("promise"));
setTimeout(() => console.log("timeout"), 0);

console.log("end");
```

**Output:**

```
start
end
nextTick
promise
timeout
```

---

### Visualization

```
Call Stack
   ↓
nextTick Queue (highest priority)
   ↓
Microtask Queue (Promises, MutationObserver)
   ↓
Macrotask Queue (timers, I/O, setImmediate)
   ↓
Event Loop cycles
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Higher priority than Promises**

   * `nextTick` runs before Promise microtasks.

2. **Starvation risk**

   * Recursive `process.nextTick` calls can block the event loop:

   ```js
   function loop() {
     process.nextTick(loop);
   }
   loop(); // Event loop starves, timers never run
   ```

3. **Not in browsers**

   * Node.js only; no equivalent in browser runtimes.

4. **Too many nextTicks**

   * Can delay I/O callbacks and timers significantly.

5. **Order of execution**

   * `nextTick` queue always fully drained before moving to Promises.

---

### ✅ Pros

* Useful for deferring execution without waiting for next event loop phase.
* Ensures code runs immediately after current stack clears.
* Great for library developers to **normalize async behavior**.

---

### ❌ Cons

* Can easily starve event loop if overused.
* Priority so high it can delay critical tasks (I/O, timers).
* Debugging starvation issues is tricky.

---

### Best Practices

* ✅ Use for **internal APIs** to defer callbacks consistently.
* ✅ Prefer Promises/`queueMicrotask` for normal microtask scheduling.
* ✅ Avoid recursive `nextTick` loops → risk of event loop starvation.
* ✅ Don’t overuse → can cause hidden performance bottlenecks.
* ✅ Use `setImmediate` instead if you need to yield to I/O first.

---

👉 Mnemonic:
**“nextTick runs before next Loop.”**

---

⚡ A great follow-up would be a **Node.js Event Loop Phases Cheat Card** (timers → pending callbacks → idle → poll → check → close), showing exactly where `nextTick` and Promises slot in.

Do you want me to prepare that next?


Here’s your **Concept Mastery Cheat Sheet** for **`queueMicrotask`** in JavaScript — crisp, visual, and fully interview-ready.

---

# 🧵 JavaScript `queueMicrotask` — Cheat Card

**Concept:**
`queueMicrotask(cb)` schedules a callback to run in the **microtask queue**, after the current synchronous code but **before the next macrotask** (timers, I/O).

* Similar to `Promise.resolve().then(cb)`, but faster and without creating a promise object.

---

### Flow

1. Current synchronous task runs.
2. Microtask queue drained (includes `queueMicrotask` callbacks).
3. Then macrotasks (`setTimeout`, I/O, etc.) run.

---

### Example

```js
console.log("A");

queueMicrotask(() => console.log("B"));

setTimeout(() => console.log("C"), 0);

console.log("D");
```

**Output:**

```
A
D
B
C
```

---

### Visualization

```
Call Stack → run sync
   ↓
Microtask Queue (Promise.then, queueMicrotask)
   ↓
Macrotask Queue (setTimeout, I/O)
   ↓
Event Loop repeats
```

🧠 **Mental Model:**

* Use `queueMicrotask` for **tiny, urgent callbacks** that must run *before timers/events*.

---

### Gotchas ⚠️ (Exhaustive)

1. **Higher priority than macrotasks**

   * Runs before timers, rendering, or I/O.

2. **Starvation risk**

   * Recursive usage can block macrotasks:

   ```js
   function loop(){ queueMicrotask(loop); }
   loop(); // UI freeze
   ```

3. **Equivalent to Promise.then**

   * But avoids extra allocation of a Promise.

4. **Error handling**

   * Exceptions thrown inside a microtask behave like unhandled promise rejections.

5. **Not cancelable**

   * Once scheduled, always runs.

---

### ✅ Pros

* Lightweight way to schedule microtasks.
* Runs faster and cleaner than creating promises.
* Ensures callback runs ASAP after current code.
* Great for deferring small tasks without waiting for timers.

---

### ❌ Cons

* Can starve event loop if misused.
* Debugging recursive microtasks tricky.
* Not useful for long/CPU-heavy tasks (blocks rendering).

---

### Best Practices

* ✅ Use for deferring **lightweight, immediate** callbacks.
* ✅ Prefer over `Promise.resolve().then()` when performance matters.
* ✅ Avoid recursive scheduling → risk of starvation.
* ✅ Don’t use for heavy logic → split into macrotasks (`setTimeout`) or workers.

---

👉 Mnemonic:
**“Microtask now, before macro.”**

---

⚡ Next step: do you want me to prepare a **Cheat Card comparing `process.nextTick` vs `queueMicrotask` vs `Promise.then`** — since interviews often test these subtle differences?

Here’s your **Concept Mastery Cheat Sheet** for **Generators (`function*`, `yield`) in JavaScript** — sharp, visual, and interview-ready.

---

# 🔄 JavaScript Generators — Cheat Card

**Concept:**
A **generator** is a special function (`function*`) that can **pause and resume execution**.

* Uses `yield` to return values one at a time.
* Controlled with an **iterator object** (`.next()`, `.throw()`, `.return()`).

---

### Syntax

```js
function* genFunc() {
  yield 1;
  yield 2;
  return 3;
}
const it = genFunc();
```

---

### Flow

1. Call generator → returns an **iterator**, not executed yet.
2. Each `.next()` resumes until next `yield` or `return`.
3. Each yield gives `{ value, done }`.
4. Execution pauses after yield; resumes later.

---

### Example

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
const it = numbers();

console.log(it.next()); // { value: 1, done: false }
console.log(it.next()); // { value: 2, done: false }
console.log(it.next()); // { value: 3, done: false }
console.log(it.next()); // { value: undefined, done: true }
```

---

### Yielding Values

```js
function* gen() {
  const x = yield 1;
  console.log("Got:", x);
}
const it = gen();
it.next();       // { value: 1, done: false }
it.next(42);     // logs "Got: 42"
```

---

### Methods

* **`next(val?)`** → resume with optional value for last yield.
* **`throw(err)`** → throw into generator at paused point.
* **`return(val)`** → end generator, optionally return value.

---

### Visualization

```
function* → paused until .next()
   │
   ├─ yield → return value, pause
   │
   ├─ .next(val) → resume with val
   │
   └─ return → done: true
```

🧠 **Mental Model:** A generator is like a **bookmarkable function** that you can resume later.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not auto-run**

   * Must use `.next()` manually or via iteration.

2. **done flag**

   * When `done: true`, further `.next()` gives `{ value: undefined, done: true }`.

3. **`return` vs `yield`**

   * `yield` produces values.
   * `return` ends generator, value usually ignored in iteration.

4. **Exception handling**

   * `.throw()` injects error at paused yield.
   * Must be caught inside generator or it ends.

5. **Delegation with `yield*`**

   * Forwards iteration to another generator/iterable.

6. **Not parallel**

   * Values are produced lazily, not concurrently.

7. **Async Generators** (`async function*`)

   * Work with `for await...of`.

---

### ✅ Pros

* Lazy evaluation of sequences.
* Pause/resume execution → powerful control.
* Can model coroutines, streams, infinite sequences.
* Interoperable with `for...of`, spread, destructuring.

---

### ❌ Cons

* More complex mental model.
* Easy to misuse `.next()` and break flow.
* Limited real-world usage vs async/await.

---

### Best Practices

* ✅ Use generators for custom iterators.
* ✅ Combine with `yield*` for delegation.
* ✅ Wrap with `for...of` for clean consumption.
* ✅ Use `async generators` for async streams.
* ✅ Don’t overuse for simple async tasks → prefer async/await.

---

👉 Mnemonic:
**“function* yields, next resumes.”**

---

⚡ Next step: do you want me to prepare a **Cheat Card on `yield*` delegation and Async Generators (`async function*`)** — since those are the natural extensions of this topic?


Here’s your **Concept Mastery Cheat Sheet** for **Async Generators (`async function*`, `for await…of`)** — tuned for interviews, crisp, and complete.

---

# 🌊 JavaScript Async Generators — Cheat Card

**Concept:**
An **async generator** (`async function*`) produces a sequence of **promises of values**.

* Consume with **`for await…of`** loop.
* Useful for **asynchronous streams** (chunks of data, events, I/O).

---

### Syntax

```js
async function* gen() {
  yield "a";
  yield Promise.resolve("b");
  yield "c";
}

for await (const val of gen()) {
  console.log(val);
}
// a, b, c
```

---

### Flow

1. Call `async function*` → returns **AsyncIterator**.
2. Each `.next()` returns a **Promise** resolving to `{ value, done }`.
3. `for await…of` unwraps promises automatically in sequence.

---

### Example: Streaming

```js
async function* streamData() {
  yield await fetch("/api/1").then(r => r.json());
  yield await fetch("/api/2").then(r => r.json());
}

for await (const chunk of streamData()) {
  console.log("Chunk:", chunk);
}
```

---

### Methods

* **next(val?)** → returns `Promise<{value, done}>`.
* **throw(err)** → throw error into generator.
* **return(val)** → end generator, optional return value.

---

### Visualization

```
async function* → AsyncIterator
   │
   ├─ yield promise/value → wrapped in Promise
   │
   └─ for await...of → awaits each value
```

🧠 **Mental Model:** Like generators, but each step may be async.
Values arrive **over time**, not instantly.

---

### Gotchas ⚠️ (Exhaustive)

1. **for await only**

   * Normal `for...of` won’t work. Must use `for await...of`.

2. **Await inside yield**

   * Can yield promises directly; auto-unwrapped.

3. **Error handling**

   * `throw()` injects error → must be caught inside.

4. **Parallelism trap**

   * `for await` runs sequentially. For parallel fetches, collect in `Promise.all`.

5. **AsyncIterator protocol**

   * Methods return promises. Needs `await` to consume.

6. **Environment support**

   * ES2018+ only. Older runtimes may lack support.

7. **Closing early**

   * Breaking `for await` loop calls `.return()` on generator.

---

### ✅ Pros

* Handles async data streams naturally.
* Clean consumption with `for await…of`.
* Perfect for reading files, sockets, APIs in chunks.
* Auto-awaits yielded promises.

---

### ❌ Cons

* Sequential by default → slower for parallel workloads.
* Not supported in very old Node/browsers.
* Error handling requires careful try/catch.

---

### Best Practices

* ✅ Use `for await…of` for consuming streams/events.
* ✅ Use async generators with Node.js streams (`Readable.from`).
* ✅ Combine with `yield*` for delegation.
* ✅ Use `Promise.all` for parallel tasks if order doesn’t matter.
* ✅ Always handle errors inside generator and consuming loop.

---

👉 Mnemonic:
**“Async gen yields promises; for await unwraps them.”**

---

⚡ Next step: Would you like me to prepare a **Cheat Card comparing Generators vs Async Generators** (side-by-side differences, use cases, gotchas) so you have a one-glance interview answer?

