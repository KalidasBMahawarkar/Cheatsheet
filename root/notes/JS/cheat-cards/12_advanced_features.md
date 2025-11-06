# 🔧 Advanced & Modern JS Features — Cheat Card
**Concept:** A compact tour of post-ES6 features that matter in real interviews: modern syntax, operators, classes, async, collections, meta-programming, and emerging proposals—plus the sharp edges.
---
## Syntax / Structure (by cluster)
### 1) Destructuring, Rest/Spread, Defaults
```js
const {a, b: alias = 0, ...restO} = obj;
const [x, , y = 10, ...restA] = arr;
const merged = {...a, ...b};           // object spread (shallow)
const copy   = structuredClone(deep);  // deep (structured) clone
```
### 2) Optional Chaining & Nullish Coalescing & Logical Assignment
```js
user?.profile?.email         // safe access
getUser?.(id)                // optional call
dict?.[key]                  // optional index
const v = input ?? "default" // only if null/undefined
a ||= b; a &&= b; a ??= b;   // logical assignments
```
### 3) Numbers, BigInt, Misc
```js
1_000_000 === 1000000
2 ** 10 === 1024
const n = 9007199254740993n; // BigInt (no mix with Number)
```
### 4) Iteration, Iterables, Generators
```js
for (const x of iterable) {}
function* gen(){ yield 1; yield 2; }
for await (const x of asyncIterable) {}
```
### 5) Promises & Async
```js
await Promise.all([p1, p2]);
await Promise.allSettled([p1, p2]);
await Promise.any([p1, p2]);           // first fulfilled
const { promise, resolve, reject } = Promise.withResolvers(); // handy
// Top-level await (ESM only)
```
### 6) Modules (ESM vs CJS) + JSON Modules
```js
// ESM
import x, {y} from "./m.js";
export default fn; export const z = 1;
// JSON module (Node/browsers with assertions)
import data from "./cfg.json" assert { type: "json" };
```
### 7) Classes: Fields, Private, Static Init
```js
class Counter {
  #count = 0;            // private field
  static registry = new Map();
  static { /* static init block */ }
  inc(){ return ++this.#count; }
  get value(){ return this.#count; }
}
```
### 8) Collections & Algorithms
```js
const s = new Set([1,2,3]);
s.has(2); s.add(4); s.delete(1);
// Modern Set methods (widely implemented):
s.union(t); s.intersection(t); s.difference(t); s.symmetricDifference(t);
s.isSubsetOf(t); s.isSupersetOf(t); s.isDisjointFrom(t);
```
### 9) Arrays (ES2023+)
```js
arr.at(-1)                 // last item
arr.findLast(fn)           // last match
arr.findLastIndex(fn)
arr.toSorted(cmp)          // non-mutating
arr.toSpliced(i, del, ...a)
arr.with(i, v)
// Optional (runtime support varies):
// Array.groupBy(fn), Array.groupByToMap(fn)
```
### 10) RegExp Power-ups
```js
/(?<year>\d{4})-(?<m>\d{2})-(?<d>\d{2})/.exec("2025-09-28").groups
/foo(?=bar)/   // lookahead
/(?<=\$)\d+/   // lookbehind
/./s           // dotAll
/\p{Letter}+/u // Unicode property escapes
[...str.matchAll(re)]
```
### 11) Intl
```js
new Intl.NumberFormat("en-IN",{style:"currency",currency:"INR"}).format(1234)
new Intl.RelativeTimeFormat("en").format(-2, "day")   // “2 days ago”
new Intl.ListFormat("en",{type:"conjunction"}).format(["a","b","c"])
```
### 12) Meta, Memory & System
```js
const clone = structuredClone(obj);
const ref = new WeakRef(obj);
const reg = new FinalizationRegistry(token => {/* cleanup */});
const p = new Proxy(target, {
  get(t, prop, r){ /* … */ return Reflect.get(t, prop, r); },
  set(t, prop, val, r){ /* … */ return Reflect.set(t, prop, val, r); }
});
```
---
## Examples (edge-case focused)
```js
// 1) Optional chaining w/ function & index
const email = users?.[0]?.getInfo?.().email ?? "N/A";
// 2) Logical assignment differences
let opts = {retry: 0};
opts.retry ||= 3;   // sets to 3 because 0 is falsy (maybe NOT desired)
opts.retry ??= 3;   // keeps 0; sets only if null/undefined
// 3) Promise.any vs allSettled
const fastestOk = await Promise.any([fetchA(), fetchB()]); // ignores rejects
const all = await Promise.allSettled([fetchA(), fetchB()]); // statuses
// 4) Class private fields enforce encapsulation
class Box {
  #v = 1;
  set v(x){ if (x < 0) throw Error(); this.#v = x; }
  get v(){ return this.#v; }
}
(new Box()).#v; // SyntaxError: can’t access outside
// 5) Non-mutating array methods preserve originals
const a = [3,1,2];
const b = a.toSorted((x,y)=>x-y); // a unchanged [3,1,2], b [1,2,3]
// 6) Proxy for transparent validation
const cfg = new Proxy({}, {
  set(t,k,v){ if(k==="port" && (v|0)!==v) throw Error("int"); return Reflect.set(t,k,v); }
});
cfg.port = 3000;    // ok
cfg.port = "3000";  // throws
// 7) WeakRef caveat: value may vanish any time
const wr = new WeakRef({payload: 42});
const maybe = wr.deref(); // could be undefined after GC
```
---
## Visualization (mental map)
```
Modern JS
├─ Syntax sugar
│  ├─ Destructuring / Rest / Spread
│  └─ Optional Chain ?. / Nullish ??
├─ Data & Collections
│  ├─ Map/Set/WeakMap/WeakSet
│  ├─ Arrays: at, findLast*, toSorted*, with*
│  └─ BigInt, numeric separators
├─ Classes & Objects
│  ├─ Public/Private fields (#)
│  ├─ Static blocks
│  └─ Proxy + Reflect
├─ Async
│  ├─ Promises (any, allSettled, withResolvers)
│  ├─ Async iterators / for-await-of
│  └─ Top-level await (ESM)
├─ Runtime APIs
│  ├─ structuredClone
│  ├─ WeakRef / FinalizationRegistry
│  └─ Intl.*
└─ Tooling/Emerging
   ├─ JSON modules (assertions)
   └─ Set operations (union, intersection, ...)
```
---
## Gotchas ⚠️ (Exhaustive & Interview-relevant)
1. **`?.` short-circuiting**
* `obj?.fn()` won’t bind `this` if `fn` comes from prototype extraction; prefer `obj?.fn?.()` when method may be missing.
* `?.` stops evaluation; side effects in arguments won’t run if left side is nullish.
2. **`??` vs `||`**
* `x ?? y` only replaces `null/undefined`. `x || y` replaces **any falsy** (`0`, `''`, `false`, `NaN`, `null`, `undefined`).
* Paired assignments differ: `a ||= b` may clobber `0`/`''`; `a ??= b` won’t.
3. **Non-mutating array methods** (`toSorted`, `toSpliced`, `with`)
* Copy-on-write semantics; O(n) copy—watch performance in hot paths.
* Not in older runtimes; include polyfills/feature-detect.
4. **`findLast`/`findLastIndex`**
* Direction is right-to-left but the callback’s index is absolute (not “reverse index”).
5. **BigInt**
* Cannot mix `Number` and `BigInt` in arithmetic; must cast explicitly.
* JSON.stringify drops BigInt (TypeError) unless converted to string.
6. **Private class fields (`#`)**
* Not accessible via bracket lookup or `this["#x"]`.
* Not on the instance object—`Object.keys()`/`in` won’t see them.
* Beware transpilation differences if targeting legacy environments.
7. **Proxy invariants**
* Must respect target’s descriptor invariants (e.g., non-configurable). Violations throw at runtime.
* `this` binding inside traps—use `Reflect.*` to mirror default behavior safely.
8. **`structuredClone` limits**
* Clones most built-ins, `Map`, `Set`, `ArrayBuffer` (detachable) but not DOM nodes or Functions.
* Cycle-safe, but transfers vs copies matter for TypedArrays/ArrayBuffers (use `transfer` option in some environments).
9. **Top-level `await`**
* ESM only; can affect module graph loading order. In Node, it may defer module availability; design around waterfalls.
10. **`Promise.any`**
* Rejects with **AggregateError** only if **all** reject; don’t forget try/catch for that case.
11. **WeakRef/FinalizationRegistry**
* Non-deterministic; never use for logic correctness. Treat as best-effort cleanup only.
12. **JSON modules & `assert { type: "json" }`**
* Tooling and runtime support vary; ESM only. Avoid in libraries without fallback.
13. **Set operations**
* Return **new** Sets, do not mutate originals. Equality is SameValueZero; objects compare by reference.
14. **RegExp lookbehind**
* Not supported in some legacy browsers; feature-detect or transpile.
15. **Intl differences**
* Locale data varies across environments; test formats for critical outputs (currency, plural rules).
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Safer access (?.), saner defaults (??, ??=).
* Immutability helpers (toSorted/with) → fewer accidental mutations.
* Stronger encapsulation (#private).
* Richer async toolbox (any, allSettled, withResolvers, top-level await).
* Powerful introspection/control (Proxy/Reflect).
* Cleaner cloning and transfer (`structuredClone`).
**❌ Cons**
* Compatibility gaps (older runtimes/polyfills).
* Performance overhead (copying arrays, Proxies).
* Private fields reduce dynamic metaprogramming flexibility.
* WeakRef/FinalizationRegistry are tricky and non-deterministic.
* Top-level await can introduce load-time waterfalls.
---
## Best Practices (crisp rules)
* Prefer `??`/`??=` for defaults; reserve `||`/`||=` for boolean-intent fallbacks.
* Use `?.` only where absence is expected; otherwise fail fast.
* Default to **non-mutating** array methods (`toSorted`, `with`) in shared data flows; use mutating ones in tight loops for perf.
* Encapsulate with **#private** fields; expose via getters/setters or methods.
* Wrap Promise combinators in utilities with timeouts & AbortController for robustness.
* For Proxies, delegate with `Reflect.*` and enforce invariants.
* Feature-detect (or Babel/TS) for modern features in libraries; document runtime requirements.
* Prefer `structuredClone` over JSON tricks for deep copies.
* Keep **top-level await** rare; isolate in leaf modules or bootstraps.
---
## Mnemonic / Quick Recall
**“S.O.C.A.P.I.M.”** → **S**afe access (?. / ??), **O**bject/Array (rest/spread, toSorted/with), **C**lasses (#, static), **A**sync (any/allSettled/withResolvers/for-await), **P**roxy/Reflect, **I**ntl/Iterables, **M**emory (structuredClone/WeakRef).
---
Would you like me to spin this into **practice Q&A** (senior-level) or apply it to a **mini exercise** (e.g., refactor a legacy snippet to modern JS)?

Got it — let’s build your **Destructuring Cheat Card**.
---
# 🧩 JavaScript Destructuring — Cheat Card
**Concept:**
Destructuring is a concise syntax to unpack values from arrays or properties from objects into distinct variables.
---
## Syntax / Structure
```js
// Array
const [a, b, c] = [1, 2, 3];
// Object
const {x, y: alias, z = 10} = {x: 1, y: 2};
// Nested
const {a: {b}} = {a: {b: 5}};
// Rest
const [head, ...tail] = [1, 2, 3];
const {p, ...rest} = {p: 1, q: 2, r: 3};
```
---
## Examples
```js
// 1) Basic
const [first,,third] = [10, 20, 30];
console.log(first, third); // 10 30
// 2) Defaults
const [a=1, b=2] = [undefined, 5];
console.log(a, b); // 1 5
// 3) Swapping
let x=1, y=2; [x,y] = [y,x];
console.log(x,y); // 2 1
// 4) Nested
const user = { name: "Ana", addr: { city: "Pune" } };
const {addr: {city}} = user;
console.log(city); // Pune
// 5) Functions
function foo([a,b]){ return a+b; }
foo([3,4]); // 7
function bar({id, name="N/A"}){ return id+":"+name; }
bar({id:1}); // "1:N/A"
```
---
## Visualization
```
Source      Pattern      Result
---------   ----------   ---------
[1,2,3]  →  [a, b, c] → a=1, b=2, c=3
{x:10}   →  {x: a}   → a=10
```
---
## Gotchas ⚠️
1. **Defaults only trigger on `undefined`**, not on `null` or falsy:
   ```js
   const [a=1] = [null]; // a = null, not 1
   ```
2. **Missing values = `undefined`:**
   ```js
   const [a,b] = [1]; // b = undefined
   ```
3. **Aliasing in objects:**
   `{x: y}` means variable `y`, not object key `y`.
4. **Rest placement:**
   Rest must be last (`const [a, ...rest, b]` ❌).
5. **Nested destructuring errors:**
   Fails if the path is `undefined`. Use defaults or optional chaining with care.
6. **Function parameter destructuring traps:**
   Passing `undefined` triggers defaults, but passing no arg may crash:
   ```js
   function f({x=1}={}){return x;}
   f(); // 1
   ```
7. **Computed property names not supported in patterns** (directly in destructuring).
8. **Order matters in arrays**; in objects it’s by property name, not order.
9. **Spread/rest shallow:**
   Doesn’t deep clone; nested refs are shared.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Concise unpacking → cleaner code.
* Works directly in function parameters.
* Supports defaults, nesting, rest.
**❌ Cons**
* Readability drops with complex nesting.
* Runtime errors on missing deep paths.
* Can obscure intent when overused.
---
## Best Practices
* Use destructuring for **clear assignments**, not when it reduces readability.
* Provide **safe defaults** in function params (`{x=1}={}`).
* Use aliasing to avoid conflicts with existing vars.
* Don’t over-nest; destructure progressively.
* For deep optional paths, destructure after null-checking.
---
## Mnemonic / Quick Recall
**“Pick, Name, Default, Rest”** → Extract it, Rename it, Fallback it, Gather leftovers.
---
👉 Want me to extend this into a **Destructuring Patterns Q&A Pack** (interview-style tricky cases with answers), or keep it as a pure reference card?
Perfect—let’s make the **Template Literals Cheat Card**.
---
# 📝 JavaScript Template Literals — Cheat Card
**Concept:**
Template literals (backticks `` ` ``) allow embedded expressions, multi-line strings, and advanced formatting using placeholders `${…}`.
---
## Syntax / Structure
```js
// Basic
`string text`
// Interpolation
`Hello, ${name}!`
// Multi-line
`This is
on two lines`
// Tagged
tag`Hello ${name}`
```
---
## Examples
```js
// 1) Interpolation
const a = 5, b = 10;
console.log(`Sum is ${a + b}`); // "Sum is 15"
// 2) Multi-line
console.log(`Line1
Line2`);
// 3) Expressions
`Today: ${new Date().toDateString()}`;
// 4) Nested
const user = {name:"Kalidas", skills:["Node","NestJS"]};
console.log(`${user.name} knows ${user.skills.join(", ")}`);
// 5) Tagged template
function censor(strings, ...values) {
  return strings.reduce((out, str, i) =>
    out + str + (values[i] ? "***" : ""), "");
}
censor`Hello ${"secret"} world`; // "Hello *** world"
```
---
## Visualization
```
`string ${expression} more text`
   Text ─────┐
   Expr ─────┼──> evaluated, converted to string
   Text ─────┘
```
---
## Gotchas ⚠️
1. **Backticks vs quotes:**
   Backticks don’t escape `'` or `"`. Use ``` to escape backticks.
2. **Interpolation casts via `String()`:**
   Non-strings are coerced (`${null}` → `"null"`, `${{}}` → `"[object Object]"`).
3. **Multi-line includes newlines:**
   Exact whitespace preserved; may cause layout bugs.
4. **Tagged template quirks:**
   First arg = array of raw string parts; subsequent = interpolated values.
   `String.raw` keeps escape sequences literally:
   ```js
   String.raw`\n` // → "\\n", not newline
   ```
5. **Performance:**
   Each interpolation runs `toString()` eagerly—don’t put heavy functions inline.
6. **Legacy environments:**
   Not in ES5—requires transpilation.
7. **Security:**
   Direct interpolation into HTML/SQL strings risks injection.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Cleaner string building than `+`.
* Supports multi-line, embedded logic.
* Tagged templates enable sanitization/localization.
**❌ Cons**
* Overuse reduces readability.
* Injection-prone if used naively.
* Debugging whitespace in multi-line strings is tricky.
---
## Best Practices
* Use template literals for **dynamic content**; avoid manual concatenation.
* Keep expressions short; compute complex logic outside.
* For HTML/SQL building, use **tagged templates** to sanitize.
* Escape backticks if literal required: `` \` ``.
* Prefer `String.raw` for regex/filepath literals.
---
## Mnemonic / Quick Recall
**“Backtick + ${} = Power Strings.”**
---
Would you like me to also create a **Tagged Template Deep Dive Cheat Card** (since that’s often an interview curveball), or keep this as the core reference?

Here’s your **Default Parameters Cheat Card** — tuned for interview-level clarity.
---
# ⚙️ JavaScript Default Parameters — Cheat Card
**Concept:**
Default parameters let functions assign initial values to parameters if `undefined` is passed or no argument is provided.
---
## Syntax / Structure
```js
function fn(param = defaultValue) { ... }
function fn(a, b = 10, c = a + b) { ... } // defaults can be expressions
```
---
## Examples
```js
// 1) Basic
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
greet(); // "Hello, Guest"
// 2) Expressions
function add(a, b = a) { return a + b; }
add(5); // 10
// 3) Undefined triggers default
function f(x = 1) { return x; }
f(undefined); // 1
f(null);      // null (no default)
// 4) With destructuring
function draw({x=0, y=0} = {}) {
  return [x, y];
}
draw(); // [0,0]
draw({x:5}); // [5,0]
// 5) Order matters
function log(a = b, b = 1) { console.log(a, b); }
log(); // ReferenceError (b not defined yet)
```
---
## Visualization
```
Call → Argument?
  ├─ undefined / omitted → use default
  └─ otherwise → keep provided value
```
---
## Gotchas ⚠️
1. **Only `undefined` triggers default**
   `null`, `false`, `0`, `NaN`, `''` → all **skip defaults**.
2. **Evaluation order left-to-right**
   Later defaults can use earlier params, not vice versa.
3. **Default params vs arguments object**
   * In **non-strict mode**, `arguments` and parameters de-sync with defaults.
   * In **strict mode/ES6+**, they’re independent:
     ```js
     function f(x=5){ x=10; console.log(arguments[0]); }
     f(); // undefined
     ```
4. **Function re-evaluates default each call**
   Objects/arrays as defaults create new copies every call.
5. **Destructuring defaults traps**
   `function f({x}={}){}` is safe;
   `function f({x}={}){}` fails if argument is `null` (not destructurable).
6. **Default param scope**
   Defaults form their own scope:
   ```js
   const a=1; function f(a=a+1){} // ReferenceError
   ```
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Eliminates boilerplate `x = x || 1`.
* Supports expressions, functions, destructuring.
* Safer with immutables (new objects each call).
**❌ Cons**
* Subtle `undefined` vs `null` semantics.
* Arguments object mismatch confuses debugging.
* Complex defaults reduce readability.
---
## Best Practices
* Use defaults for **safe, common cases**, not complex logic.
* For destructuring, always provide **safe outer defaults** (`={}`).
* Avoid mixing with `arguments`—use rest `...args` instead.
* Document intentional `null` vs `undefined` behavior.
---
## Mnemonic / Quick Recall
**“Undefined → Default, Null → Value.”**
---
Would you like me to also prepare a **comparison card** of “Default Parameters vs Short-Circuit (`||`) vs Nullish Coalescing (`??`)"? That’s a common interview trap.

Alright — here’s a **Rest & Spread Operators Cheat Card** in our strict format.
---
# 🌪️ JavaScript Rest & Spread Operators — Cheat Card
**Concept:**
`...` is used in two roles:
* **Spread** → expands iterable/object into elements/props.
* **Rest** → collects remaining elements/props into an array/object.
---
## Syntax / Structure
```js
// Spread (expand)
fn(...arr);
[...arr1, ...arr2];
{...obj1, ...obj2};
// Rest (collect)
function fn(a, ...rest) {}
const [head, ...tail] = arr;
const {p, ...others} = obj;
```
---
## Examples
```js
// 1) Spread in function calls
Math.max(...[1,2,3]); // 3
// 2) Array literals
const arr = [1, ...[2,3], 4]; // [1,2,3,4]
// 3) Object literals
const obj = {a:1, ...{b:2}, c:3}; // {a:1,b:2,c:3}
// 4) Rest in functions
function sum(...nums){ return nums.reduce((a,b)=>a+b,0); }
sum(1,2,3); // 6
// 5) Rest in arrays
const [x, ...rest] = [1,2,3]; // x=1, rest=[2,3]
// 6) Rest in objects
const {a, ...restObj} = {a:1,b:2,c:3}; // a=1, restObj={b:2,c:3}
// 7) Shallow copy
const copy = {...obj}; // same references for nested objects
```
---
## Visualization
```
Spread → explode
   [1, ...[2,3], 4] → [1,2,3,4]
Rest → gather
   [a, ...rest] = [1,2,3,4]
        a=1, rest=[2,3,4]
```
---
## Gotchas ⚠️
1. **Shallow copy only**
   Spread/rest don’t deep clone nested structures.
   ```js
   const o1 = {nested:{x:1}};
   const o2 = {...o1};
   o2.nested.x=99; // affects o1
   ```
2. **Order matters in object spread**
   Later props override earlier ones:
   `{a:1, ...{a:2}} → {a:2}`.
3. **Non-iterables with spread**
   `[...123]` ❌ → TypeError (only iterable).
   But object spread works on plain objects.
4. **Function rest placement**
   Must be **last param**: `function f(a,...rest){} // ok`
   `function f(...rest,a){} // ❌`.
5. **Performance**
   Spreading large arrays/objects is O(n); avoid in hot paths.
6. **Array holes**
   Spread fills holes:
   ```js
   [...[,,1]] // [undefined, undefined, 1]
   ```
7. **Arguments vs Rest**
   * `arguments` is array-like, includes all args.
   * Rest is a true array, excludes explicitly declared params.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Cleaner syntax for cloning/merging.
* Rest gives true arrays.
* Works with both arrays and objects.
**❌ Cons**
* Shallow copy only.
* Performance hit with huge data.
* Order-sensitive in objects.
---
## Best Practices
* Prefer spread for **cloning/merging** small-medium objects/arrays.
* For **deep clones**, use `structuredClone`, `JSON.parse(JSON.stringify())`, or libs.
* Use rest instead of `arguments`.
* Watch out for overrides when merging objects.
* Use array methods (`concat`) for massive merges to reduce overhead.
---
## Mnemonic / Quick Recall
**“Spread explodes, Rest collects.”**
---
Do you want me to also create a **comparison card**: *Rest vs Arguments vs Spread vs Destructuring* (since that’s a frequent interview confusion)?

Here’s your **Arrow Functions Cheat Card** — compact, dense, and senior-level.
---
# 🏹 JavaScript Arrow Functions — Cheat Card
**Concept:**
Arrow functions (`() => {}`) are concise function expressions with lexical `this`, no `arguments`, and simplified return rules.
---
## Syntax / Structure
```js
// Basic
const fn = (a, b) => a + b;
// Multi-line
const fn = (a, b) => {
  const sum = a + b;
  return sum;
};
// Single param (parens optional)
x => x * 2
// No params
() => 42
// Returning object literal
() => ({id: 1, name: "Kalidas"})
```
---
## Examples
```js
// 1) Concise return
const double = x => x*2; 
double(4); // 8
// 2) Lexical this
const obj = {
  val: 42,
  regFn: function(){ setTimeout(function(){ console.log(this.val); }, 10); }, 
  arrFn: function(){ setTimeout(() => console.log(this.val), 10); }
};
obj.regFn(); // undefined (this = global)
obj.arrFn(); // 42 (this = obj)
// 3) No arguments object
function regular(){ console.log(arguments[0]); }
regular(5); // 5
const arrow = () => console.log(arguments[0]);
arrow(5); // ReferenceError: arguments is not defined
// 4) As callbacks
[1,2,3].map(n => n**2); // [1,4,9]
// 5) Returning objects
const makeUser = (id, name) => ({id, name});
makeUser(1,"Ana"); // {id:1, name:"Ana"}
```
---
## Visualization
```
Arrow Function
   - `this` → outer scope (lexical)
   - `arguments` → not available
   - return → implicit if single expression
```
---
## Gotchas ⚠️
1. **No own `this`**
   * Lexically bound → can’t rebind with `.call`, `.apply`, or `.bind`.
   * Useful for callbacks, but **not** for object methods requiring dynamic `this`.
2. **No `arguments` object**
   * Must use rest `(...args)` instead.
3. **No `new`**
   * Arrow functions aren’t constructors; calling with `new` throws.
4. **No prototype**
   * `(() => {}).prototype` is `undefined`.
5. **Object methods trap**
   * As object method, `this` ≠ object:
     ```js
     const o = {x:1, f:()=>console.log(this.x)};
     o.f(); // undefined
     ```
6. **Returning objects**
   * Must wrap in `()` or `{}` is parsed as block.
7. **Hoisting**
   * Arrow functions are expressions → not hoisted.
8. **Line breaks after `=>`**
   * Implicit return breaks if newline after arrow:
     ```js
     () => 
     { foo:1 } // undefined, block not object
     ```
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Concise syntax.
* Lexical `this` → no `.bind(this)`.
* Great for callbacks, higher-order functions.
**❌ Cons**
* Can’t be constructors.
* No `arguments` / `super` / `new.target`.
* Misused in object methods → subtle bugs.
---
## Best Practices
* Use arrows for short callbacks, array methods, closures.
* Use regular functions for methods needing dynamic `this`.
* Prefer rest args (`(...args)`) over `arguments`.
* Wrap object literal returns in parentheses.
* Avoid arrow in class prototypes when method inheritance needed.
---
## Mnemonic / Quick Recall
**“Arrow sticks to outer `this`.”**
---
Do you want me to also create a **comparison card: Arrow Functions vs Regular Functions** (highlighting all differences side-by-side)? That’s a common interview table question.

Here’s your **Optional Chaining Cheat Card** — dense and interview-ready.
---
# 🔗 JavaScript Optional Chaining (`?.`) — Cheat Card
**Concept:**
Optional chaining lets you safely access nested properties, methods, or array items without throwing errors if part of the chain is `null` or `undefined`.
---
## Syntax / Structure
```js
obj?.prop         // property access
obj?.[expr]       // dynamic property access
obj?.method?.()   // method call
arr?.[index]      // array element
```
---
## Examples
```js
// 1) Safe property access
const user = { profile: { email: "a@b.com" } };
user?.profile?.email;   // "a@b.com"
user?.settings?.theme;  // undefined, no error
// 2) Safe array access
const arr = null;
arr?.[0]; // undefined, no crash
// 3) Safe method call
const api = null;
api?.getData?.(); // undefined, no TypeError
// 4) Combined with nullish coalescing
user?.settings?.lang ?? "en"; // fallback = "en"
```
---
## Visualization
```
Access: obj?.prop
   ├─ If obj === null/undefined → returns undefined
   └─ Else → continue normal property access
```
---
## Gotchas ⚠️
1. **Only short-circuits on `null` or `undefined`**
   Falsy values like `0`, `''`, or `false` still allow access.
   ```js
   const x = { count: 0 };
   x?.count; // 0, not undefined
   ```
2. **Stops evaluation**
   Expressions after a failed chain don’t run.
   ```js
   obj?.fn(expensive()); // `expensive()` never runs if obj?.fn is nullish
   ```
3. **Not a replacement for existence check**
   `obj?.prop` returns `undefined`, but you may need `in` operator if distinguishing between “missing” vs “undefined value”.
4. **Only works on `null/undefined`**
   Throws if trying to access on other non-objects:
   ```js
   (123)?.x; // TypeError
   ```
5. **Cannot appear on left-hand side**
   Invalid:
   ```js
   obj?.prop = 5; // ❌ SyntaxError
   ```
6. **Method calls**
   `obj?.method()` preserves `this` binding correctly.
   But `obj.method?.()` may lose binding if not careful with extraction.
7. **Compatibility**
   ES2020+ only. Not supported in ES5 without transpilation.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Prevents repetitive null checks (`&&` chains).
* Reduces boilerplate.
* Works for properties, methods, and arrays.
**❌ Cons**
* Can silently hide bugs by swallowing `undefined`.
* Easy to misuse for values that should *never* be optional.
* Not supported in old runtimes without transpiler.
---
## Best Practices
* Use when **absence is expected/allowed**, not for critical data.
* Pair with `??` for safe defaults.
* Avoid deep optional chains in critical logic; prefer validation.
* Don’t replace all checks—sometimes explicit errors are safer.
---
## Mnemonic / Quick Recall
**“`?.` → Stop if nullish, else continue.”**
---
Would you like me to also prepare a **side-by-side comparison card**: *Optional Chaining (`?.`) vs Nullish Coalescing (`??`) vs Logical AND (`&&`)*? That’s a very common interview follow-up.

Here’s your **Nullish Coalescing Cheat Card** — sharp, senior-level, and ready for interviews.
---
# 🌀 JavaScript Nullish Coalescing (`??`) — Cheat Card
**Concept:**
The `??` operator returns the right-hand operand **only if** the left-hand operand is `null` or `undefined` (a.k.a. *nullish* values).
---
## Syntax / Structure
```js
const value = expr1 ?? expr2;
// if expr1 is null/undefined → use expr2
```
---
## Examples
```js
// 1) Basic
null ?? "fallback";      // "fallback"
undefined ?? "default";  // "default"
0 ?? 5;                  // 0 (not nullish)
"" ?? "N/A";             // "" (empty string kept)
// 2) With variables
const user = {name: null};
const name = user.name ?? "Anonymous"; 
// "Anonymous"
// 3) Chaining
const port = config.port ?? env.port ?? 3000;
// 4) Function params
function greet(name) {
  return name ?? "Guest";
}
greet(); // "Guest"
// 5) Distinguishing from ||
0 || 42;  // 42 (0 is falsy)
0 ?? 42;  // 0  (0 is not null/undefined)
```
---
## Visualization
```
expr1 ?? expr2
   ├─ if expr1 === null/undefined → expr2
   └─ else → expr1
```
---
## Gotchas ⚠️
1. **Only triggers on nullish**
   Falsy values (`0`, `''`, `false`, `NaN`) do **not** use fallback.
2. **Precedence quirks**
   Lower than `||`/`&&`, higher than `=`.
   Must use parentheses in complex expressions:
   ```js
   a || b ?? c; // SyntaxError
   (a || b) ?? c; // OK
   ```
3. **Assignment form**
   ES2021+ adds `??=`:
   ```js
   let x;
   x ??= 5; // assigns 5 only if x is null/undefined
   ```
4. **Don’t mix blindly with `||`**
   `||` covers *all falsy*, `??` only covers *nullish*. Pick deliberately.
5. **Not supported in ES5**
   Requires transpilation for older environments.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Distinguishes between *absent* vs *falsy* values.
* Cleaner default assignment.
* Useful in config loading, function params.
**❌ Cons**
* Operator precedence surprises.
* Misuse can hide bugs (silently masking null).
* Needs transpiler for old browsers.
---
## Best Practices
* Use `??` for **safe defaults** when `null/undefined` are valid “no value” signals.
* Use `||` when *any falsy* should trigger fallback.
* Wrap in parentheses when mixing with `||` or `&&`.
* Document intentional difference: `value ?? 0` vs `value || 0`.
---
## Mnemonic / Quick Recall
**“?? means Only Null or Undefined → Fallback.”**
---
Would you like me to prepare a **side-by-side cheat card: `||` vs `??` vs default parameters**, since that’s one of the trickiest interview comparisons?

Here’s your **Iterators & Generators Cheat Card** — dense, full coverage, and tuned for interview traps.
---
# 🔄 JavaScript Iterators & Generators — Cheat Card
**Concept:**
* **Iterator:** An object with a `.next()` method returning `{value, done}`.
* **Iterable:** An object implementing `[Symbol.iterator]` that returns an iterator.
* **Generator:** A function (`function*`) that automatically creates an iterator with `yield`.
---
## Syntax / Structure
```js
// Iterator protocol
const iterator = iterable[Symbol.iterator]();
iterator.next(); // { value: ..., done: ... }
// Generator function
function* gen() {
  yield 1;
  yield 2;
  return 3;
}
```
---
## Examples
```js
// 1) Basic iterator
const arr = [10, 20];
const it = arr[Symbol.iterator]();
it.next(); // {value:10, done:false}
it.next(); // {value:20, done:false}
it.next(); // {value:undefined, done:true}
// 2) Custom iterator
const counter = {
  from: 1, to: 3,
  [Symbol.iterator]() {
    let cur = this.from, end = this.to;
    return {
      next() {
        return cur <= end ? {value: cur++, done:false}
                          : {done:true};
      }
    }
  }
};
[...counter]; // [1,2,3]
// 3) Generator
function* gen() {
  yield "a";
  yield "b";
}
for (const x of gen()) console.log(x); // "a", "b"
// 4) Passing values in
function* chat() {
  const answer = yield "What’s your name?";
  yield `Hello, ${answer}`;
}
const it2 = chat();
it2.next();         // {value:"What’s your name?", done:false}
it2.next("Kalidas"); // {value:"Hello, Kalidas", done:false}
// 5) Delegation
function* outer() {
  yield 1;
  yield* [2,3]; // delegates iteration
}
[...outer()]; // [1,2,3]
// 6) Async generator
async function* asyncGen() {
  yield "start";
  await new Promise(r=>setTimeout(r,100));
  yield "end";
}
for await (const val of asyncGen()) console.log(val);
```
---
## Visualization
```
Iterable → Iterator
    └── .next() → { value, done }
Generator
    ├─ function*
    ├─ yield → pauses/resumes
    └─ yields values via .next()
```
---
## Gotchas ⚠️
1. **`done:true` convention**
   After completion, `value` may still exist (return value), but `for...of` ignores it.
2. **Generators vs normal functions**
   * Generators pause/resume stateful execution.
   * Must be called: `gen()` returns iterator, not values directly.
3. **Delegation (`yield*`)**
   Flattens another iterable/generator into current. Beware recursion depth.
4. **Return vs yield**
   * `yield` produces values for consumers.
   * `return` ends iteration and provides final value (ignored by `for...of`).
5. **Async generators**
   * Consume with `for await...of`.
   * Must yield Promises/await internally.
6. **Symbol.iterator vs Symbol.asyncIterator**
   Async iterables need `Symbol.asyncIterator`.
7. **Spread/rest and iterables**
   `[...iterable]` exhausts it; reusing the same iterator may fail.
8. **Performance**
   Generators add overhead; not for hot loops with millions of iterations.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Uniform iteration protocol (arrays, maps, strings, sets, custom).
* Generators simplify state machines & lazy sequences.
* Async generators → natural async streaming.
**❌ Cons**
* Extra indirection vs arrays.
* Easy to misuse `return`/`yield`.
* Async iteration support varies in tooling.
---
## Best Practices
* Use generators for **lazy evaluation** or infinite sequences.
* Prefer `for...of` over manual `.next()`.
* Use `yield*` for composition.
* For async data streams (e.g. API pagination, sockets), prefer async generators.
* Document whether custom objects expose `[Symbol.iterator]`.
---
## Mnemonic / Quick Recall
**“Iterable is readable, Iterator is next-able, Generator is yield-able.”**
---
👉 Do you want me to also prepare a **side-by-side comparison card: Iterator vs Generator vs Async Generator**, since that’s a frequent interview deep dive?

Here’s your **Async Iterators Cheat Card** — senior-level, detailed, and interview-ready.
---
# 🔁 JavaScript Async Iterators — Cheat Card
**Concept:**
Async iterators are objects implementing `[Symbol.asyncIterator]`, consumed with `for await...of`. They allow asynchronous, sequential consumption of data streams (e.g., API pages, file chunks).
---
## Syntax / Structure
```js
// Async iterator protocol
const asyncIterable = {
  async *[Symbol.asyncIterator]() {
    yield 1;
    yield 2;
  }
};
// Consumption
for await (const val of asyncIterable) {
  console.log(val);
}
// Manual
const it = asyncIterable[Symbol.asyncIterator]();
await it.next(); // {value:1, done:false}
```
---
## Examples
```js
// 1) Basic async generator
async function* numbers() {
  yield 1;
  await new Promise(r => setTimeout(r, 100));
  yield 2;
}
for await (const n of numbers()) console.log(n);
// 2) Async pagination
async function* fetchPages() {
  let page=1, hasMore=true;
  while(hasMore) {
    const data = await fetch(`/api?page=${page}`).then(r=>r.json());
    yield data.items;
    hasMore = data.hasMore;
    page++;
  }
}
for await (const items of fetchPages()) {
  console.log("Got page:", items);
}
// 3) Manual consumption
const it = numbers()[Symbol.asyncIterator]();
console.log(await it.next()); // {value:1, done:false}
console.log(await it.next()); // {value:2, done:false}
console.log(await it.next()); // {done:true}
// 4) Async iterator delegation
async function* outer() {
  yield* numbers(); // works with async generators
}
```
---
## Visualization
```
Async Iterable
   └─ [Symbol.asyncIterator]() → Async Iterator
         └─ next() → Promise<{value, done}>
Consumption
   for await...of → await each .next()
```
---
## Gotchas ⚠️
1. **`for await...of` only**
   Regular `for...of` won’t work on async iterables.
2. **`.next()` returns a Promise**
   Must `await` each call; forgetting causes unresolved Promises.
3. **Mixing sync & async**
   * Sync iterables use `[Symbol.iterator]`.
   * Async iterables use `[Symbol.asyncIterator]`.
   * They’re distinct; you can implement both.
4. **`yield*` with async**
   Only works inside async generators, not plain functions.
5. **Error handling**
   Errors thrown inside async generators reject the Promise returned by `.next()`.
   Wrap `for await...of` in `try...catch`.
6. **Early exit / `return()`**
   Breaking a `for await...of` loop calls `return()` on the iterator — must handle cleanup in `finally`.
7. **Backpressure**
   Consumers await each value; producers must manage pacing carefully.
8. **Older environments**
   Async iterators need ES2018+; not in ES5 without transpilers/polyfills.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Natural handling of streams/pagination.
* Sequential async flow without manual Promises.
* Integrates with async/await style.
**❌ Cons**
* More verbose than simple arrays.
* Easy to forget `await` in consumption.
* Tooling/compatibility issues in legacy environments.
---
## Best Practices
* Use `for await...of` for readability in async loops.
* Always wrap async iteration in `try...catch` + `finally` for cleanup.
* Implement both `[Symbol.iterator]` and `[Symbol.asyncIterator]` when designing flexible APIs.
* Use async generators (`async function*`) to simplify async iteration logic.
* Apply throttling/backpressure if consuming external streams.
---
## Mnemonic / Quick Recall
**“Async Iterator = Iterator + Promise.”**
---
👉 Want me to expand this into a **comparison cheat card (Iterator vs Generator vs Async Generator vs Async Iterator)** — side-by-side table? That’s often the *exact* interview follow-up.

Perfect choice — this is a heavy interview favorite. Here’s your **Proxy & Reflect Cheat Card**.
---
# 🪞 JavaScript Proxy & Reflect — Cheat Card
**Concept:**
* **Proxy:** Wrapper around a target object that intercepts operations (`get`, `set`, etc.) via traps.
* **Reflect:** A built-in object with static methods that provide the default behavior of these operations (mirrors object internals).
---
## Syntax / Structure
```js
const proxy = new Proxy(target, {
  get(target, prop, receiver) { ... },
  set(target, prop, value, receiver) { ... },
  // many other traps...
});
// Reflect usage
Reflect.get(target, prop, receiver);
Reflect.set(target, prop, value, receiver);
```
---
## Examples
```js
// 1) Basic logging proxy
const user = { name: "Ana", age: 25 };
const p = new Proxy(user, {
  get(t, prop, r) {
    console.log(`Access: ${prop}`);
    return Reflect.get(t, prop, r);
  }
});
p.name; // logs, then returns "Ana"
// 2) Validation
const validator = new Proxy({}, {
  set(t, prop, val) {
    if(prop === "age" && typeof val !== "number") throw Error("Age must be number");
    return Reflect.set(t, prop, val);
  }
});
validator.age = 30; // ✅
validator.age = "old"; // ❌ throws
// 3) Default values
const withDefault = new Proxy({}, {
  get: (t, prop) => prop in t ? t[prop] : "N/A"
});
console.log(withDefault.missing); // "N/A"
// 4) Private fields protection
const secret = { token: "12345" };
const guard = new Proxy(secret, {
  get: (t, prop) => prop.startsWith("_") ? undefined : t[prop]
});
// 5) Using Reflect to forward
const logSet = new Proxy({}, {
  set(t, prop, val, r) {
    console.log(`Set ${prop}=${val}`);
    return Reflect.set(t, prop, val, r);
  }
});
logSet.a = 42;
```
---
## Visualization
```
Operation → Proxy trap? 
   ├─ Yes → custom logic (optionally call Reflect.* for default)
   └─ No  → default object behavior
```
---
## Gotchas ⚠️
1. **Trap invariants**
   * Proxies must respect target’s descriptor rules (e.g., can’t report non-existent property if non-configurable).
   * Violations → runtime `TypeError`.
2. **`this` binding**
   * Use `Reflect.*` to preserve correct `this` (receiver).
   * `target[prop]` may break prototypes.
3. **Performance**
   * Every access goes through traps → slower than plain objects. Not for hot paths.
4. **Identity**
   * `proxy !== target`. WeakMap keys break if target replaced with proxy.
5. **JSON.stringify / Object.keys**
   * Trigger `ownKeys`/`getOwnPropertyDescriptor`. Must align with reality.
6. **No trap = fallback**
   * If trap missing, default semantics apply (like normal object).
7. **Revocable proxies**
   ```js
   const {proxy, revoke} = Proxy.revocable(target, handler);
   revoke(); proxy.a; // TypeError
   ```
8. **`Reflect` ≠ Mirror**
   * Methods on Reflect return booleans (e.g., `Reflect.defineProperty`), not exceptions like `Object.defineProperty`.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Custom validation, logging, defaults.
* Security wrappers (hiding private data).
* Virtual objects / API façades.
* Great for metaprogramming.
**❌ Cons**
* Slower access.
* Easy to break invariants.
* Debugging harder (unexpected behavior).
* Can be overused for things solvable with functions/classes.
---
## Best Practices
* Use Proxies sparingly for **cross-cutting concerns** (validation, logging, security).
* Always prefer `Reflect.*` inside traps to maintain default behavior.
* Keep traps minimal — don’t reimplement language internals.
* Consider revocable proxies for temporary wrappers.
* Avoid Proxies in performance-critical sections.
---
## Mnemonic / Quick Recall
**“Proxy intercepts, Reflect reflects.”**
* Proxy = customization layer.
* Reflect = safe defaults / direct mirror.
---
👉 Want me to prepare a **side-by-side traps table** (all Proxy traps vs Reflect methods) as a quick reference? That’s gold for interviews.

Here’s your **WeakMap & WeakSet Cheat Card** — interview-focused, complete, and sharp.
---
# 🧩 JavaScript WeakMap & WeakSet — Cheat Card
**Concept:**
* **WeakMap:** Map where **keys must be objects**, held weakly (GC removes entries if no other refs).
* **WeakSet:** Set where **values must be objects**, also held weakly.
* Both are non-enumerable and allow memory-safe associations.
---
## Syntax / Structure
```js
// WeakMap
const wm = new WeakMap();
wm.set(obj, value);
wm.get(obj);
wm.has(obj);
wm.delete(obj);
// WeakSet
const ws = new WeakSet();
ws.add(obj);
ws.has(obj);
ws.delete(obj);
```
---
## Examples
```js
// 1) WeakMap as private storage
const wm = new WeakMap();
class User {
  constructor(name) {
    wm.set(this, {secret: "hidden"});
    this.name = name;
  }
  getSecret() { return wm.get(this).secret; }
}
const u = new User("Ana");
u.getSecret(); // "hidden"
// 2) WeakSet for tracking
const seen = new WeakSet();
function process(obj) {
  if (seen.has(obj)) return "skip";
  seen.add(obj);
  return "process";
}
process({}); // "process"
process({}); // "process" (different object)
const o = {};
process(o); // "process"
process(o); // "skip"
// 3) Auto-GC
let obj = {id: 1};
wm.set(obj, "meta");
obj = null; // key/value auto-collected eventually
```
---
## Visualization
```
WeakMap
   key(obj) → value(any)
   keys held weakly
WeakSet
   { obj, obj, ... }
   values held weakly
```
---
## Gotchas ⚠️
1. **Only objects allowed**
   * `wm.set(1, "x")` ❌ TypeError.
   * `ws.add("str")` ❌ TypeError.
2. **Non-enumerable**
   * No `.keys()`, `.values()`, `.entries()`, `.forEach()`.
   * Can’t spread or iterate.
3. **Weak references only**
   * Keys/values vanish after GC; no reliable way to “list all”.
4. **Identity-based**
   * Different object literals aren’t equal:
     ```js
     ws.add({}); ws.has({}); // false
     ```
5. **Testing in console**
   * GC timing is non-deterministic → can’t force entry removal predictably.
6. **Serialization**
   * Not serializable with JSON; intended only for runtime associations.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Avoids memory leaks (keys auto-collected).
* Great for hidden/private data storage.
* Useful for caching tied to object lifetimes.
**❌ Cons**
* Non-iterable → can’t inspect contents.
* Debugging harder (entries invisible).
* Only supports objects → no primitive keys/values.
---
## Best Practices
* Use **WeakMap** for **private data** in classes or caching metadata.
* Use **WeakSet** for **tracking object membership** (seen, processed).
* Don’t rely on GC timing; treat cleanup as eventual.
* If enumeration is needed, use Map/Set instead.
* Keep keys as objects tied to lifecycle you want GC’d.
---
## Mnemonic / Quick Recall
**“Weak = Object-only, Auto-GC, Invisible.”**
---
👉 Do you want me to also make a **comparison card: Map vs WeakMap vs Set vs WeakSet** — side-by-side table? That’s a common panel-round interview follow-up.

Here’s your **Dynamic `import()` Cheat Card** — detailed, crisp, and interview-ready.
---
# 📦 JavaScript Dynamic `import()` — Cheat Card
**Concept:**
`import()` is a function-like expression that **loads ES modules dynamically at runtime**, returning a Promise that resolves to the module object.
---
## Syntax / Structure
```js
// Returns a Promise
import(moduleSpecifier)
  .then(module => { /* use module */ })
  .catch(err => { /* handle error */ });
// With await
const module = await import("./utils.js");
```
---
## Examples
```js
// 1) Basic
import("./math.js").then(m => {
  console.log(m.add(2,3));
});
// 2) With await (ESM or top-level await)
const math = await import("./math.js");
console.log(math.add(5,7));
// 3) Conditional loading
if (process.env.NODE_ENV === "development") {
  const devTools = await import("./dev-tools.js");
  devTools.init();
}
// 4) Dynamic path
async function loadLocale(lang) {
  const locale = await import(`./locales/${lang}.js`);
  return locale.default;
}
await loadLocale("en"); // imports ./locales/en.js
// 5) Lazy loading heavy lib
button.onclick = async () => {
  const {chart} = await import("./charting.js");
  chart.render();
};
```
---
## Visualization
```
import("module") → Promise<ModuleObject>
   ├─ static import → upfront load
   └─ dynamic import → runtime, async
```
---
## Gotchas ⚠️
1. **Returns a Promise**
   Always async; must use `await` or `.then()`.
2. **Specifier must be a string**
   Must resolve to a valid module path; bundlers may not support fully dynamic expressions.
3. **Top-level `await` required**
   Awaiting directly only works in ES modules (not CommonJS, not in all browsers).
4. **Tree-shaking differences**
   Static `import` supports tree-shaking; dynamic may pull whole module.
5. **Errors at runtime**
   If file missing or path invalid, Promise rejects → must handle.
6. **CORS & policies**
   In browsers, module must follow same-origin or CORS rules.
7. **Node.js nuances**
   * Works in ESM files; in CommonJS, use `await import("...")`.
   * File extension required (`.js`, `.mjs`).
8. **Dynamic expressions**
   `import(someVar)` not always supported by bundlers → may need explicit hints (`/* webpackChunkName */`).
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* On-demand loading (perf optimization).
* Conditional imports (feature flags, environment-based).
* Works with async/await seamlessly.
* Enables code-splitting with bundlers.
**❌ Cons**
* Async → not usable in sync contexts.
* Tree-shaking limited.
* Tooling support varies (Webpack, Rollup handle differently).
* Error handling required at runtime.
---
## Best Practices
* Use for **lazy-loading heavy modules** or environment-specific code.
* Always handle `.catch()` or wrap in `try/catch`.
* Prefer static imports when modules are critical and always needed.
* When using bundlers, leverage `/* webpackChunkName */` for chunk names.
* Avoid overly dynamic paths; use template literals with controlled values.
---
## Mnemonic / Quick Recall
**“Static upfront, Dynamic async.”**
* `import ... from` → compile-time.
* `import()` → runtime, Promise.
---
👉 Do you want me to also create a **side-by-side card: `require()` vs static `import` vs dynamic `import()`**, since that’s a classic Node.js interview question?
Here’s your **BigInt Cheat Card** — crisp, exhaustive, and tuned for interviews.
---
# 🔢 JavaScript BigInt — Cheat Card
**Concept:**
`BigInt` is a built-in numeric primitive that represents **arbitrarily large integers** beyond the `Number.MAX_SAFE_INTEGER` limit (`2^53 - 1`).
---
## Syntax / Structure
```js
// Literal
const big = 123n;
// Constructor
const big2 = BigInt("123");
// Conversion
BigInt(Number.MAX_SAFE_INTEGER); 
```
---
## Examples
```js
// 1) Large safe integer
9007199254740991 + 1;     // 9007199254740992 (⚠️ unsafe)
9007199254740991n + 1n;   // 9007199254740992n (✅ precise)
// 2) Arithmetic
10n + 20n; // 30n
20n * 3n;  // 60n
// 3) Comparisons
1n < 2n;   // true
2n === 2;  // false (different types)
2n == 2;   // true (loose equality)
// 4) Mixing with Number (must cast)
10n + BigInt(5); // 15n
// 5) Division truncates
5n / 2n; // 2n (no decimals)
// 6) Boolean contexts
if (0n) console.log("never"); // falsy
Boolean(123n); // true
// 7) With Math
Math.sqrt(16n); // ❌ TypeError
```
---
## Visualization
```
Number (53-bit safe)      → up to 9,007,199,254,740,991
BigInt                    → arbitrary precision (limited by memory)
```
---
## Gotchas ⚠️
1. **No mixing with Number**
   Must convert explicitly: `10n + 5` ❌ → TypeError.
2. **Division truncates**
   Always rounds down to nearest integer: `7n / 2n → 3n`.
3. **JSON.stringify**
   Throws `TypeError`: BigInt not supported in JSON → convert to string.
4. **Math methods unsupported**
   `Math.*` functions don’t work; need BigInt-aware libs or cast.
5. **Different strict equality**
   `2n === 2` → false, but `2n == 2` → true.
6. **Performance**
   Slower than `Number` for small ints; only use when needed.
7. **Crypto**
   BigInt is precise but not a replacement for cryptographic big integer libs (no modular arithmetic helpers built-in).
8. **TypedArrays**
   Use `BigInt64Array` or `BigUint64Array` for binary data.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Arbitrary integer precision.
* Safe beyond `Number.MAX_SAFE_INTEGER`.
* Works with normal operators (`+`, `-`, `*`, `%`, `**`).
**❌ Cons**
* No decimal support.
* Incompatible with `Math.*`.
* JSON unsupported.
* Slower for small numbers.
---
## Best Practices
* Use `BigInt` only when values exceed safe integer range or require exact precision.
* Convert carefully when mixing with `Number`.
* For serialization, convert explicitly to string.
* Avoid in hot loops unless necessary.
* Don’t use for floats/decimals — use libraries (Decimal.js, etc.).
---
## Mnemonic / Quick Recall
**“BigInt = Big Integers, not Big Decimals.”**
---
👉 Do you want me to also build a **Number vs BigInt comparison card** (with ranges, operators, quirks) since that’s a frequent whiteboard interview trap?
Here’s your **`Intl` API Cheat Card** — covering all key sub-APIs, common examples, and interview traps.
---
# 🌍 JavaScript `Intl` API — Cheat Card
**Concept:**
The `Intl` (Internationalization) API provides locale-sensitive formatting for numbers, dates, times, lists, and messages — essential for global-ready apps.
---
## Syntax / Structure
```js
new Intl.<Formatter>([locales], [options]).format(value)
```
Where:
* `locales`: BCP 47 language tag(s) (`"en-US"`, `"fr-FR"`, `"hi-IN"`)
* `options`: object customizing format
---
## Major APIs & Examples
### 1) **Intl.NumberFormat**
```js
// Currency
new Intl.NumberFormat("en-IN", { style: "currency", currency: "INR" })
  .format(1234567); 
// ₹12,34,567.00
// Compact notation
new Intl.NumberFormat("en", { notation: "compact" })
  .format(12345); // "12K"
// Percent
new Intl.NumberFormat("en", { style: "percent" })
  .format(0.256); // "26%"
```
---
### 2) **Intl.DateTimeFormat**
```js
// Basic
new Intl.DateTimeFormat("en-GB").format(new Date()); // 28/09/2025
// Custom
new Intl.DateTimeFormat("en-US", { 
  weekday: "long", year: "numeric", month: "long", day: "numeric"
}).format(new Date()); 
// "Sunday, September 28, 2025"
// Time zones
new Intl.DateTimeFormat("en-US", { timeZone: "Asia/Kolkata", timeStyle:"short" })
  .format(new Date());
```
---
### 3) **Intl.RelativeTimeFormat**
```js
const rtf = new Intl.RelativeTimeFormat("en", { numeric: "auto" });
rtf.format(-2, "day"); // "2 days ago"
rtf.format(1, "week"); // "next week"
```
---
### 4) **Intl.ListFormat**
```js
const lf = new Intl.ListFormat("en", { style: "long", type: "conjunction" });
lf.format(["Apples", "Bananas", "Cherries"]); 
// "Apples, Bananas, and Cherries"
```
---
### 5) **Intl.PluralRules**
```js
const pr = new Intl.PluralRules("en-US");
pr.select(1); // "one"
pr.select(2); // "other"
```
---
### 6) **Intl.Collator**
```js
const coll = new Intl.Collator("en");
["z", "ä", "a"].sort(coll.compare); // ["a","ä","z"]
```
---
### 7) **Intl.DisplayNames**
```js
new Intl.DisplayNames(["en"], {type:"region"}).of("IN"); // "India"
```
---
### 8) **Intl.Locale** (ES2020+)
```js
const loc = new Intl.Locale("fr-Latn-FR", { calendar: "gregory" });
loc.baseName; // "fr-FR"
loc.language; // "fr"
```
---
## Visualization
```
Intl
├─ NumberFormat       → numbers, currency, %
├─ DateTimeFormat     → date & time (with zones)
├─ RelativeTimeFormat → "2 days ago"
├─ ListFormat         → "A, B, and C"
├─ PluralRules        → plural categories
├─ Collator           → locale string compare
├─ DisplayNames       → names for regions/languages
└─ Locale             → locale parsing & info
```
---
## Gotchas ⚠️
1. **Locale fallback**
   If unsupported locale, falls back to closest match.
2. **Browser/runtime differences**
   Formatting can vary slightly by environment (esp. currency, plural rules).
3. **Number limits**
   Works with `Number` and `BigInt`; throws if mixing.
4. **Time zones**
   Must be valid IANA names (`"Asia/Kolkata"`, not `"IST"`).
5. **PluralRules**
   Returns categories (`"one"`, `"few"`, `"many"`, `"other"`), not words — you must map to messages.
6. **JSON.stringify**
   `Intl` objects are not serializable; must store config, not instances.
7. **Performance**
   Creating formatters repeatedly in loops is costly → reuse them.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Built-in, no external libs.
* Handles locale & script variations.
* Covers numbers, dates, lists, plurals.
**❌ Cons**
* Verbose configuration.
* Output can differ slightly across engines.
* Missing high-level features (e.g. full message formatting, ICU features).
---
## Best Practices
* **Reuse formatter instances** (cache them).
* Always provide `locales` explicitly — don’t rely on defaults.
* Pair `PluralRules` with message dictionaries.
* Validate time zones with `Intl.supportedValuesOf("timeZone")` (ES2022+).
* For complex i18n, use ICU-based libs (FormatJS, Globalize).
---
## Mnemonic / Quick Recall
**“Intl = Numbers, Dates, Lists, Words — all locale-aware.”**
---
👉 Do you want me to also create a **side-by-side card of all Intl APIs (NumberFormat, DateTimeFormat, etc.) with one-liner use cases**? That’s a great quick-reference for interviews.
Here’s your **Symbols & Meta-Programming Cheat Card** — this one’s especially important for senior-level interviews because it touches **language internals, unique IDs, and customizing behavior via well-known symbols.**
---
# 🪙 JavaScript `Symbol` & Meta-Programming — Cheat Card
**Concept:**
* **Symbol:** A unique, immutable primitive often used as hidden object keys.
* **Meta-programming:** Using `Symbol`s (esp. well-known ones) and objects like `Proxy`, `Reflect` to customize or extend language behavior.
---
## Syntax / Structure
```js
// Unique symbols
const id = Symbol("id");
// Global registry
const gid1 = Symbol.for("key");
const gid2 = Symbol.for("key"); // same symbol
Symbol.keyFor(gid1); // "key"
```
---
## Examples
```js
// 1) Hidden keys
const user = { name: "Ana" };
const secret = Symbol("secret");
user[secret] = 12345;
console.log(Object.keys(user));   // ["name"] (symbol hidden)
console.log(user[secret]);        // 12345
// 2) Global registry
const s1 = Symbol.for("app.token");
const s2 = Symbol.for("app.token");
console.log(s1 === s2); // true
// 3) Well-known symbols
class Coll {
  constructor(...items){ this.items = items; }
  [Symbol.iterator]() { // iterable protocol
    let i = 0, arr = this.items;
    return { next: () => ({value: arr[i++], done:i>arr.length}) };
  }
}
for (const v of new Coll(1,2,3)) console.log(v); // 1 2 3
// 4) Symbol.toPrimitive
const obj = {
  [Symbol.toPrimitive](hint) {
    return hint === "number" ? 42 : "obj";
  }
};
+obj;       // 42
`${obj}`;   // "obj"
// 5) Symbol.toStringTag
const f = () => {};
f[Symbol.toStringTag] = "CoolFn";
Object.prototype.toString.call(f); // "[object CoolFn]"
// 6) Symbol.hasInstance
class CheckNum {
  static [Symbol.hasInstance](val) { return typeof val === "number"; }
}
42 instanceof CheckNum; // true
// 7) Symbol.species
class MyArr extends Array {
  static get [Symbol.species]() { return Array; }
}
const a = new MyArr(1,2,3);
const b = a.map(x => x*2); // Array, not MyArr
```
---
## Well-Known Symbols (Meta-Programming Hooks)
| Symbol                                                            | Purpose / Hook                                           |
| ----------------------------------------------------------------- | -------------------------------------------------------- |
| `Symbol.iterator`                                                 | Makes object iterable with `for...of`, spread            |
| `Symbol.asyncIterator`                                            | For async iteration (`for await...of`)                   |
| `Symbol.toPrimitive`                                              | Custom primitive conversion                              |
| `Symbol.toStringTag`                                              | Controls `Object.prototype.toString` output              |
| `Symbol.hasInstance`                                              | Custom `instanceof` check                                |
| `Symbol.species`                                                  | Controls derived object constructor (e.g. Array methods) |
| `Symbol.isConcatSpreadable`                                       | Controls `[].concat(obj)` flattening                     |
| `Symbol.match`, `Symbol.replace`, `Symbol.search`, `Symbol.split` | Customize regex/string behavior                          |
---
## Visualization
```
Symbol
├─ Unique IDs
├─ Global registry (Symbol.for / keyFor)
└─ Well-known symbols (protocol hooks)
      ├─ iteration
      ├─ conversion
      ├─ instanceof
      └─ regex behaviors
```
---
## Gotchas ⚠️
1. **Symbols are unique**
   `Symbol("x") !== Symbol("x")`. Use `Symbol.for` if you need shared identity.
2. **Hidden from normal enumeration**
   Not in `Object.keys`, `for...in`. Use `Object.getOwnPropertySymbols`.
3. **JSON.stringify ignores symbols**
   Symbols are skipped → need manual handling.
4. **Well-known symbols are not magic methods**
   Must implement correctly (e.g., `[Symbol.iterator]` must return an iterator).
5. **`Symbol.toPrimitive` precedence**
   Called before `valueOf` or `toString` when coercing.
6. **Performance**
   Too many symbol keys can complicate debugging — hidden props are harder to track.
7. **`instanceof` traps**
   Overriding `[Symbol.hasInstance]` can confuse maintainers.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Create truly unique object keys.
* Hidden/internal properties safe from collisions.
* Powerful meta-hooks to control language behavior.
**❌ Cons**
* Non-enumerable → debugging harder.
* JSON serialization ignores them.
* Overusing meta-symbols makes code obscure.
---
## Best Practices
* Use `Symbol()` for **hidden/private properties**.
* Use `Symbol.for()` for **shared/global keys**.
* Implement well-known symbols only when customizing behavior is intentional.
* Prefer readability: document when using meta-symbols.
* Avoid using them for “fake private fields” in modern code → prefer `#private`.
---
## Mnemonic / Quick Recall
**“Symbol = Unique ID, Meta-Programming = Hook into JS language.”**
---
👉 Do you want me to also make a **quick-reference table of all meta-programming hooks (Proxy traps + Well-Known Symbols + Reflect)** so you can cover this as one unified interview card?

