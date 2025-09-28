Perfect — here’s your **Concept Mastery Cheat Sheet** for **Advanced JavaScript Types & Structures** — i.e. beyond basic primitives, objects, and arrays. It covers Maps, Sets, WeakMaps, WeakSets, Typed Arrays, and more, with **all gotchas grouped together**.

---

# 🧩 JavaScript Advanced Types & Structures — Cheat Card

**Concept:** Specialized data structures introduced in ES6+ for efficient storage, lookup, and memory control.

---

### Map

```js
const m = new Map();
m.set("a",1); m.set({},2);
m.get("a"); // 1
```

* Key → any type.
* Maintains insertion order.
* `.size`, `.has`, `.delete`, `.clear`.

---

### WeakMap

```js
const wm = new WeakMap();
let obj = {};
wm.set(obj,"secret");
```

* Keys → **objects only**.
* Weak references (GC-eligible).
* Not iterable.

---

### Set

```js
const s = new Set([1,2,2,3]);
s.has(2); // true
```

* Stores **unique values**.
* Insertion order preserved.
* `.size`, `.add`, `.delete`, `.clear`.

---

### WeakSet

```js
const ws = new WeakSet();
let obj={};
ws.add(obj);
```

* Values → **objects only**.
* Weak references.
* Not iterable.

---

### Typed Arrays & Buffers

```js
const buf = new ArrayBuffer(16);  // raw bytes
const view = new Uint8Array(buf);
view[0]=255; // binary storage
```

* For binary data.
* Types: `Int8Array`, `Uint8Array`, `Float32Array`, etc.
* Backed by `ArrayBuffer` + `DataView`.

---

### Other Structures

* **Date** → `new Date()`, timestamps.
* **RegExp** → `/pattern/flags`.
* **Error types** → `Error`, `TypeError`, `SyntaxError`, etc.
* **BigInt** → integers beyond 2^53-1.

---

### Gotchas ⚠️ (Exhaustive)

1. **Map vs Object**

   * Objects stringify keys; Maps keep key identity.
   * `map.set({},1); map.set({},2);` → 2 entries.
2. **Set uniqueness rules**

   * Uses SameValueZero: `NaN` equal to `NaN`, `0` = `-0`.
3. **WeakMap/WeakSet**

   * Keys must be objects.
   * No `.size`, `.keys`, `.forEach`.
   * Entries removed when object key GC’ed.
4. **Iteration differences**

   * Map/Set → iterable directly (`for…of`).
   * WeakMap/WeakSet → not iterable.
5. **Serialization**

   * JSON ignores Map/Set/WeakMap/WeakSet.
   * Must convert (`Array.from(map)` / `[...set]`).
6. **Typed Arrays quirks**

   * Fixed length (cannot resize).
   * Endianness matters in cross-system binary data.
   * Out-of-bounds access returns `undefined`, not error.
7. **Performance traps**

   * Deep prototype chains slower than Map lookups.
   * Overusing Sets for tiny lists slower than arrays.
8. **RegExp quirks**

   * Statefulness with `lastIndex` in global/sticky regex.
   * `test` on global regex advances index.
9. **BigInt**

   * Cannot mix with numbers directly (`1n + 1` ❌).
   * JSON.stringify(BigInt) → ❌ TypeError.

---

### Best Practices

* ✅ Use **Map** for dynamic key-value collections.
* ✅ Use **Set** for uniqueness.
* ✅ Use **WeakMap/WeakSet** for private data & memory-sensitive caching.
* ✅ Use **TypedArrays** for binary/large numeric data.
* ✅ Always convert before serialization.
* ✅ Keep usage clear — Objects for structure, Maps/Sets for collections.

---

👉 Mnemonic:

* **Map = Key store**,
* **Set = Unique bag**,
* **Weak = GC-friendly**,
* **Typed = Binary data**.

---

Would you like me to **split this into separate cheat cards per structure (Map, WeakMap, Set, WeakSet, Typed Arrays)** so you get ultra-focused cards for each?

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Arrays** — fully loaded with syntax, methods, and **all gotchas grouped together**.

---

# 📦 JavaScript Arrays — Cheat Card

**Concept:**
Ordered, zero-indexed collections of values. Special type of object with powerful built-in methods.

---

### Creation

```js
const arr1 = [1,2,3];
const arr2 = new Array(3);    // length = 3, empty slots
const arr3 = Array.of(1,2,3); // [1,2,3]
const arr4 = Array.from("abc"); // ["a","b","c"]
```

---

### Core Properties

* **Index-based access** → `arr[0]`
* **Length** → `arr.length` (mutable)
* **Sparse arrays** allowed (`[ , , 3]`)

---

### Mutator Methods

```js
push(4), pop()          // add/remove end
shift(), unshift(0)     // add/remove start
splice(1,2,"x")         // remove/replace/insert
sort(), reverse()
fill(0), copyWithin()
```

---

### Accessor Methods (non-mutating)

```js
concat(), slice(), join(), includes(), indexOf(), lastIndexOf()
```

---

### Iteration / Functional

```js
forEach(x=>...), map(x=>...), filter(x=>...), reduce((a,b)=>...), reduceRight()
some(x=>...), every(x=>...)
find(x=>...), findIndex(x=>...), flat(), flatMap()
```

---

### ES2023+ Goodies

* `.at(-1)` → last element.
* `toReversed(), toSorted(), toSpliced(), with()` → return **new arrays** without mutating.

---

### Gotchas ⚠️ (Exhaustive)

1. **Sparse arrays**

   * `new Array(3)` → `[ <3 empty items> ]`.
   * Methods may skip empty slots (e.g., `map`).
2. **Length behavior**

   * `arr.length=0` clears array.
   * Setting smaller length truncates.
   * Larger length creates holes.
3. **Type coercion in sort()**

   * Default `sort()` converts to strings.
   * `["10","2"].sort()` → `["10","2"]`.
   * Fix: `arr.sort((a,b)=>a-b)`.
4. **delete operator**

   * `delete arr[0]` leaves a hole, doesn’t shrink length.
5. **Performance**

   * Avoid sparse arrays for performance.
   * Use typed arrays for large numeric datasets.
6. **`for...in` vs `for...of`**

   * `for...in` iterates keys (indices as strings, includes inherited).
   * `for...of` iterates values (correct).
7. **Comparison**

   * Arrays are objects; compared by reference.
   * `[1]==[1]` → false.
8. **Copy pitfalls**

   * Spread (`[...arr]`) = shallow copy.
   * Nested objects still reference same memory.
   * Use `structuredClone` for deep copies.
9. **Negative indices**

   * `arr[-1]` is not last element, it’s a property.
   * Use `.at(-1)` instead.
10. **Array-like objects**

    * `arguments`, `NodeList` look like arrays but lack methods.
    * Convert: `Array.from(arrayLike)`.

---

### Best Practices

* ✅ Use array literals `[]` instead of `new Array()`.
* ✅ Use `.at(-1)` for last element instead of hacks.
* ✅ Always provide compare function to `sort()`.
* ✅ Use `map/filter/reduce` for clarity, but avoid in performance-critical loops.
* ✅ Convert array-likes with `Array.from()`.
* ✅ For immutability, prefer new ES2023 methods (`toSorted`, `with`, etc.).

---

👉 Mnemonic: **“Array = Ordered list, mind the holes, always copy carefully.”**

---

Would you like me to also prepare a **cheat card on Array Iteration Patterns** (classic loops vs `forEach`, `map`, `reduce`, `for...of`), since interviewers often test which to use when?

---

Here’s your **Concept Mastery Cheat Sheet** for the most important **Array Functional Methods** — `map`, `filter`, `reduce`, `find`, `some`, `every` — concise, with **all gotchas grouped together**.

---

# 🛠️ JavaScript Array Functional Methods — Cheat Card

---

### `map()`

```js
[1,2,3].map(x=>x*2); // [2,4,6]
```

* Transforms each element → returns **new array**.
* Callback: `(element, index, array)`

---

### `filter()`

```js
[1,2,3,4].filter(x=>x%2===0); // [2,4]
```

* Keeps elements where callback → truthy.
* Returns **new array**.

---

### `reduce()`

```js
[1,2,3].reduce((acc,val)=>acc+val,0); // 6
```

* Accumulates values → single result.
* Callback: `(acc, current, index, array)`
* Optional **initialValue**.

---

### `find()`

```js
[1,2,3,4].find(x=>x>2); // 3
```

* Returns **first matching element** or `undefined`.

---

### `some()`

```js
[1,2,3].some(x=>x>2); // true
```

* Returns true if **any element** passes test.

---

### `every()`

```js
[1,2,3].every(x=>x>0); // true
```

* Returns true if **all elements** pass test.

---

### Gotchas ⚠️ (Exhaustive)

1. **Original array not mutated**

   * Except if callback mutates elements.
2. **Callback args**

   * All get `(element, index, array)` — forgetting can cause bugs.
   * Example:

     ```js
     ["1","2","3"].map(parseInt); // [1,NaN,NaN]
     ```

     (`parseInt` uses 2nd arg as radix).
3. **Sparse arrays**

   * Methods skip empty slots:

     ```js
     new Array(3).map(()=>1); // [ , , ]
     ```
4. **reduce() quirks**

   * If no initialValue → first element used as accumulator.
   * On empty array without initialValue → ❌ TypeError.
5. **find()**

   * Only first match returned. To get all, use `filter`.
6. **some()/every() short-circuit**

   * `some` stops at first true.
   * `every` stops at first false.
7. **Performance**

   * Chaining (`arr.map(...).filter(...)`) creates intermediate arrays.
   * Use `reduce` for single-pass operations if perf critical.
8. **thisArg**

   * All methods accept optional `thisArg` as second parameter. Rarely used, but important:

     ```js
     arr.map(fn, thisArg);
     ```
9. **Mutation inside callbacks**

   * Can lead to surprising results if you push/modify during iteration.
10. **every() on empty arrays**

    * Returns `true` (vacuous truth).
    * `some()` on empty array returns `false`.

---

### Best Practices

* ✅ Use `map` for transformation, `filter` for selection.
* ✅ Always provide `initialValue` in `reduce`.
* ✅ Use `find` for first match, `filter` for multiple.
* ✅ Don’t use `map` if you’re not returning a value (use `forEach`).
* ✅ Use `some`/`every` for readability over manual loops.
* ✅ Be cautious with sparse arrays; prefer initializing fully.

---

👉 Mnemonic:

* **map = transform**
* **filter = keep**
* **reduce = fold into one**
* **find = first match**
* **some = at least one**
* **every = all pass**

---

Would you like me to also make a **separate cheat card just for `reduce()`** (since it’s the trickiest of these, with many interview use-cases like flattening, grouping, memoization)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Sparse Arrays** in JavaScript — short, complete, with **all gotchas grouped together**.

---

# 🕳️ JavaScript Sparse Arrays — Cheat Card

**Concept:**
Arrays with “holes” → indices exist in `length` count but have **no values** stored.

---

### Creation

```js
const a = new Array(3);   // [ <3 empty items> ]
const b = [1,,3];         // [1, <empty>, 3]
```

---

### Behavior

* **Length counts holes**

  ```js
  [ , , ].length; // 2
  ```
* **Accessing a hole** → `undefined`.
* **Iteration** varies: some methods skip holes.

---

### Methods & Iteration

```js
const arr = [1,,3];

// Skips hole
arr.forEach(x=>console.log(x)); // 1, 3
arr.map(x=>x||0);               // [1, <empty>, 3]

// Preserves hole
arr.join("-");   // "1--3"
arr.toString();  // "1,,3"

// Fills holes
arr[1] = 2;      // now [1,2,3]
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Undefined ≠ empty slot**

   ```js
   [undefined].length; // 1
   [,].length;         // 1 (but hole, not undefined)
   ```
2. **Skipped in iteration**

   * `forEach`, `map`, `filter`, `some`, `every` → **skip holes**.
   * `for...in`, `for...of`, `Object.keys` → include existing indices only.
   * `Array.from` / spread (`[...arr]`) → turn holes into `undefined`.
3. **reduce() quirks**

   * Skips holes, but respects length.

   ```js
   [ ,1].reduce((a,b)=>a+b); // 1 (skips hole)
   ```
4. **Performance**

   * Sparse arrays are slower (not optimized like dense arrays).
5. **JSON.stringify**

   * Converts holes to `null`.

   ```js
   JSON.stringify([,1,2]); // "[null,1,2]"
   ```
6. **Inconsistent debugging**

   * DevTools show `<empty>` vs `undefined`. Don’t confuse.
7. **Array methods**

   * `delete arr[1]` creates a hole, doesn’t shrink array.
   * Use `splice()` to actually remove.
8. **Array holes in ES2023 methods**

   * New immutable methods (`toSpliced`, `toSorted`, `with`) still handle holes consistently → holes preserved unless explicitly filled.

---

### Best Practices

* ✅ Avoid creating sparse arrays intentionally.
* ✅ Use `Array.fill` to initialize:

  ```js
  new Array(3).fill(0); // [0,0,0]
  ```
* ✅ Use `Array.from({length:n}, (_,i)=>i)` for dense ranges.
* ❌ Don’t use `delete arr[i]`; use `splice()`.
* ✅ Treat `undefined` explicitly instead of holes.

---

👉 Mnemonic: **“Holes ≠ Undefined. Holes = skipped, undefined = value.”**

---

Would you like me to now prepare a **cheat card on Array Copying & Cloning** (spread, slice, Array.from, structuredClone), since sparse arrays behave differently across these?

---

Here’s your **Concept Mastery Cheat Sheet** for **Array-like Objects** in JavaScript — compact, practical, and with **all gotchas grouped together**.

---

# 📑 JavaScript Array-like Objects — Cheat Card

**Concept:**
Objects that look like arrays (indexed keys + `length`) but lack array methods.

---

### Examples

```js
// arguments object
function demo(a,b){
  console.log(arguments[0]); // a
  console.log(arguments.length); // count
}

// DOM collections
const divs = document.querySelectorAll("div"); // NodeList
```

* `arguments`
* DOM collections (`NodeList`, `HTMLCollection`)
* Typed arrays (`Uint8Array`, etc.)
* Custom objects `{0:"a",1:"b",length:2}`

---

### Converting to Real Array

```js
Array.from(arguments);
[...divs];
Array.prototype.slice.call(arguments);
```

---

### Properties

* Indexed elements → numeric keys `"0"`, `"1"`, …
* `.length` property.
* No `map`, `filter`, `reduce`, etc.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not true arrays**

   * Fail `Array.isArray()`.
   * Still typeof `"object"`.
2. **Missing methods**

   * No `push`, `pop`, `map`, etc.
   * Must borrow via `Array.prototype.method.call(obj, ...)`.
3. **Live collections**

   * `HTMLCollection` updates automatically if DOM changes.
   * `NodeList` may be live or static (depends on API).
4. **Iteration quirks**

   * Some iterable (NodeList with `for...of`), some not (`arguments` pre-ES6).
5. **Length mismatch**

   * `length` may not reflect true count (custom array-likes).
6. **Sparse-like behavior**

   * Holes behave inconsistently. `Array.from({length:3})` → `[undefined, undefined, undefined]`.
7. **Performance**

   * Converting very large DOM collections repeatedly is costly.
8. **arguments vs rest params**

   * `arguments` is array-like, rest params give true arrays.

   ```js
   function f(...args){ console.log(Array.isArray(args)); } // true
   ```
9. **Copy traps**

   * Spreading `[...obj]` only works if iterable. Non-iterable array-like → ❌.

---

### Best Practices

* ✅ Convert array-likes to arrays with `Array.from` or spread.
* ✅ Use rest params (`...args`) instead of `arguments`.
* ✅ Don’t rely on live collections; convert to static arrays before iteration.
* ✅ Always check `Array.isArray()` when expecting real arrays.

---

👉 Mnemonic: **“Array-like = Looks like array, but walks like object.”**

---

Would you like me to next prepare a **cheat card comparing Array-like vs Iterable vs Real Array** (since interviews often test the difference between these three)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Strings** in JavaScript — complete, concise, and with **all gotchas grouped together**.

---

# 🔤 JavaScript Strings — Cheat Card

**Concept:**
Immutable sequences of UTF-16 code units, used for text.

---

### Creation

```js
const s1 = "hello";
const s2 = 'world';
const s3 = `hi ${s1}`;   // template literal
const s4 = new String("boxed"); // ❌ rarely use
```

---

### Properties

* **Immutable** → cannot change individual chars.
* **Length** → `str.length`.
* Indexed access: `str[0]`.

---

### Common Methods

```js
"abc".charAt(1);      // "b"
"abc".charCodeAt(1);  // 98
"abc".at(-1);         // "c" (ES2022)

"hello".toUpperCase(); // "HELLO"
"HELLO".toLowerCase(); // "hello"

"hello world".includes("lo"); // true
"hello".startsWith("he");     // true
"hello".endsWith("lo");       // true

"repeat ".repeat(3); // "repeat repeat repeat "

"   hi   ".trim();     // "hi"
"   hi   ".trimStart();// "hi   "
"   hi   ".trimEnd();  // "   hi"

"abc".padStart(5,"-"); // "--abc"
"abc".padEnd(5,"-");   // "abc--"

"hi,bye".split(","); // ["hi","bye"]
["a","b"].join("-"); // "a-b"
```

---

### Template Literals

```js
const name="Kalidas";
`Hello, ${name}!`; // interpolation
`line1
line2`;           // multi-line
```

---

### Iteration

```js
for (const ch of "hi") console.log(ch);
// "h", "i"
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Immutability**

   ```js
   let s="hi"; s[0]="H"; // ❌ no effect
   ```

   Must create new strings.
2. **UTF-16 quirks**

   * Some Unicode chars (emoji, astral symbols) use 2 code units.
   * `"😀".length === 2`, not 1.
   * Use ES6 `for...of` or `.codePointAt()` for proper handling.
3. **Boxed vs primitive**

   * `new String("x")` creates object wrapper, not primitive.
   * Comparisons differ:

     ```js
     new String("x") === "x"; // false
     ```
4. **Index access**

   * Read-only.
   * `str[0]` works, but cannot assign.
5. **`split("")` & Unicode**

   * Breaks surrogate pairs:

     ```js
     "😀".split(""); // ["�","�"]
     ```
   * Use `[...str]`.
6. **Locale-sensitive methods**

   * `toLocaleUpperCase`, `toLocaleLowerCase` differ by locale.
7. **Search methods quirks**

   * `indexOf` returns `-1` if not found.
   * Case-sensitive by default.
8. **Performance**

   * Strings are immutable → concatenation creates new string each time.
   * For large builds, use arrays + `join()`.
9. **Template literal traps**

   * Expressions inside `${}` coerced to string unless object has `.toString()`.
10. **Trim & pad support**

    * Older environments may not support `.trimStart`, `.trimEnd`, `.padStart`, `.padEnd`.

---

### Best Practices

* ✅ Prefer string literals over `new String()`.
* ✅ Use template literals for dynamic strings.
* ✅ Use `for...of` or spread for Unicode-safe iteration.
* ✅ Use `Array.join` for efficient large concatenations.
* ✅ Always consider Unicode edge cases if working with emojis/intl.

---

👉 Mnemonic: **“Strings = Immutable text, UTF-16 quirks, handle with care.”**

---

Would you like me to also prepare a **cheat card on Template Literals & Tagged Templates** (since they’re powerful string features often missed in interviews)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Template Literals** in JavaScript — sharp, practical, and with **all gotchas grouped together**.

---

# 📜 JavaScript Template Literals — Cheat Card

**Concept:**
Introduced in ES6, template literals (backticks `` ` ``) allow **embedded expressions, multi-line strings, and custom string processing**.

---

### Syntax

```js
const name = "Kalidas";
const msg = `Hello, ${name}!`; // interpolation
```

* Delimited by backticks `` ` ``.
* Support `${expr}` for embedding values.
* Preserve newlines/whitespace.

---

### Features

1. **Expression Interpolation**

```js
`2 + 3 = ${2+3}`; // "2 + 3 = 5"
```

2. **Multi-line Strings**

```js
`Line 1
Line 2`; 
```

3. **Nesting**

```js
`Hello ${`Mr. ${name}`}`;
```

4. **Tagged Templates**

```js
function tag(strings, ...values){
  return strings[0] + values.map(v=>v.toUpperCase()).join(" ");
}
tag`hi ${"world"} ${"kalidas"}`; // "hi WORLD KALIDAS"
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Automatic toString coercion**

   * Objects interpolated via `.toString()`.

   ```js
   const obj={toString:()=> "OBJ"};
   `${obj}`; // "OBJ"
   ```
2. **Expression execution**

   * Expressions are evaluated immediately (not lazy).
3. **Injection risks**

   * Embedding raw user input can cause XSS in HTML strings.
   * Use proper escaping/sanitization.
4. **Performance**

   * Heavy use in hot loops may create many short-lived strings.
5. **Tagged templates quirks**

   * `strings` array elements are **frozen** (`Object.freeze`).
   * Raw string form accessible via `String.raw`.
6. **Escaping backticks**

   ```js
   `\``; // literal backtick
   ```
7. **JSON pitfalls**

   * Template literals don’t automatically escape quotes.

   ```js
   const json = `{"x": "${val}"}`; // ❌ unsafe if val contains quotes
   ```
8. **Line endings**

   * Newlines preserved literally (not `\n` by default).
9. **Tagged templates + security**

   * Safe for DSLs (e.g., GraphQL, SQL), but misuse can leak injection bugs.

---

### Best Practices

* ✅ Prefer template literals for readability over string concatenation.
* ✅ Use them for multi-line strings, dynamic values.
* ✅ For user input in HTML, sanitize before interpolation.
* ✅ Use tagged templates (`String.raw`, custom tags) for special formatting.
* ✅ Escape backticks if needed.
* ❌ Don’t overuse for static strings.

---

👉 Mnemonic: **“Template Literals = Backticks + ${Expressions} + Multiline + Tags.”**

---

Would you like me to also prepare a **dedicated cheat card on Tagged Templates & String.raw** (since those are the advanced parts interviewers like to test)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Unicode, Graphemes & Normalization** in JavaScript — deep but concise, with **all gotchas grouped together**.

---

# 🌍 JavaScript Unicode, Graphemes & Normalization — Cheat Card

**Concept:**
JS strings are UTF-16 encoded. Characters (graphemes) may consist of one or more code units. Normalization ensures different Unicode forms compare equally.

---

### Code Units vs Code Points

```js
"😀".length;        // 2 (two UTF-16 code units)
"😀".codePointAt(0); // 128512 (true Unicode code point)
```

* **Code unit**: 16-bit chunk (what JS indexes).
* **Code point**: actual Unicode scalar value.
* **Surrogate pairs**: characters outside BMP (`U+10000+`) use 2 code units.

---

### Graphemes

* Visible character may be multiple code points:

  * `🇮🇳` (India flag) = 2 code points.
  * `á` = `a + combining acute`.

```js
[..."🇮🇳"].length; // 2 (not 1)
```

* Grapheme clusters ≠ JS string “character”.

---

### Normalization

```js
const s1 = "é";                // U+00E9
const s2 = "e\u0301";          // e + combining acute
s1 === s2;                     // false
s1.normalize() === s2.normalize(); // true
```

* Unicode allows multiple representations.
* `.normalize(form)` ensures canonical form.
* Forms:

  * **NFC** (composed, default)
  * **NFD** (decomposed)
  * **NFKC**, **NFKD** (compatibility variants).

---

### Iteration

```js
for (const ch of "😀á") console.log(ch);
// "😀", "a", "́"
```

* `for...of` iterates **code points**, not code units.

---

### Gotchas ⚠️ (Exhaustive)

1. **`.length` misleading**

   * Counts UTF-16 units, not graphemes.
   * `"😀".length === 2`.
2. **Index access broken for surrogate pairs**

   ```js
   "😀"[0]; // "\uD83D" (half a char)
   ```
3. **Split issues**

   * `"á".split("")` → `["a","́"]` (combining marks separate).
   * Use `[...str]` or libraries like Graphemer for grapheme-safe splits.
4. **Regex pitfalls**

   * `.` in regex matches code units, not graphemes.
   * ES2015+ `u` flag improves (but still not grapheme-aware).

   ```js
   "😀".match(/./u); // ["😀"]
   ```
5. **Upper/lowercase quirks**

   * Case mapping depends on locale. `"i".toUpperCase()` vs Turkish `İ`.
6. **Normalization required**

   * String visually same may compare unequal unless normalized.
7. **Sorting**

   * Lexical ordering ≠ human ordering. Use `localeCompare()`.
8. **Performance**

   * `.normalize()` and grapheme-safe operations slower on huge texts.
9. **JSON/stringify**

   * Keeps code points but doesn’t normalize.
10. **Emoji sequences**

* Complex emoji (family 👨‍👩‍👧‍👦) = multiple code points joined by zero-width joiners. Length/iteration behave unexpectedly.

---

### Best Practices

* ✅ Use `for...of` or spread `[...str]` for code point iteration.
* ✅ Use `.normalize()` before comparisons.
* ✅ Use `localeCompare()` for sorting.
* ✅ Avoid direct indexing when dealing with Unicode text.
* ✅ Use libraries (`graphemer`, `Intl.Segmenter`) for grapheme awareness.
* ❌ Don’t trust `.length` as character count.

---

👉 Mnemonic: **“Code units ≠ Code points ≠ Graphemes. Normalize for equality.”**

---

Would you like me to make a **dedicated cheat card on `Intl` APIs (Intl.Collator, Intl.Segmenter, Intl.NumberFormat, etc.)**, since they’re crucial for real-world Unicode handling in JS?


Here’s your **Concept Mastery Cheat Sheet** for **Numbers** in JavaScript — everything from basics to quirks, with **all gotchas grouped together**.

---

# 🔢 JavaScript Numbers — Cheat Card

**Concept:**
All numbers in JS (except `BigInt`) are **64-bit IEEE-754 floating point** → can represent integers up to **2⁵³-1 safely**.

---

### Creation

```js
let n1 = 42;         // integer
let n2 = 3.14;       // float
let n3 = 1e6;        // exponential
let n4 = 0b1010;     // binary (10)
let n5 = 0o755;      // octal (493)
let n6 = 0xFF;       // hex (255)
```

---

### Properties

* `Number.MAX_SAFE_INTEGER` → 2⁵³-1
* `Number.MIN_SAFE_INTEGER` → -(2⁵³-1)
* `Number.MAX_VALUE` → ~1.79e308
* `Number.MIN_VALUE` → ~5e-324
* `Number.POSITIVE_INFINITY`, `Number.NEGATIVE_INFINITY`
* `Number.NaN` (Not a Number)

---

### Methods

```js
Number.isNaN(NaN);         // true
Number.isFinite(123);      // true
Number.isInteger(10.0);    // true
Number.parseInt("42px");   // 42
Number.parseFloat("3.14"); // 3.14
```

---

### Math API

```js
Math.round(4.6);   // 5
Math.floor(4.9);   // 4
Math.ceil(4.1);    // 5
Math.trunc(4.9);   // 4
Math.random();     // [0,1)
Math.max(1,2,3);   // 3
Math.pow(2,3);     // 8
2**3;              // 8 (exponentiation operator)
```

---

### Special Values

```js
NaN !== NaN;                // true
Object.is(NaN, NaN);        // true
0 === -0;                   // true
Object.is(0, -0);           // false
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Floating-point precision**

   * `0.1 + 0.2 !== 0.3` → `0.30000000000000004`
   * Use `Number.EPSILON` for tolerance:

     ```js
     Math.abs(a-b) < Number.EPSILON
     ```
2. **Safe integer limits**

   * Beyond ±(2⁵³-1) → precision errors.
   * `Number.isSafeInteger(2**53); // false`
   * Use `BigInt` for huge integers.
3. **NaN quirks**

   * `NaN === NaN` → false.
   * Only reliable via `Number.isNaN()` or `Object.is()`.
4. **Infinity**

   * Dividing by 0 → `Infinity`.
   * Arithmetic with `Infinity` may produce `NaN`.
5. **parseInt pitfalls**

   * `parseInt("08")` → `8` (not octal in ES5+).
   * Always pass radix: `parseInt("08", 10)`.
6. **Loose equality coercion**

   * `"42" == 42` → true.
   * `"42px"` == 42 → false, but `parseInt("42px")` → 42.
7. **Bitwise ops**

   * Convert numbers to **32-bit signed ints**.
   * Large numbers overflow: `1<<31` → -2147483648.
8. **JSON quirks**

   * JSON.stringify drops `Infinity`/`NaN` → `null`.
9. **-0**

   * `-0` exists (distinct from `0` in `Object.is`).
   * Breaks some logic (e.g., dividing).
10. **Math.random**

    * Returns pseudo-random; not cryptographically secure.
    * Use `crypto.getRandomValues` for secure random.
11. **Number wrappers**

    * `new Number(5)` creates object, not primitive. Avoid.
12. **Trailing decimals**

    * `42.` is valid JS (same as `42.0`).
13. **Octal literals**

    * Legacy `012` is disallowed in strict mode.

---

### Best Practices

* ✅ Use `Number.isNaN` / `Number.isFinite`.
* ✅ Use `Number.EPSILON` for float comparison.
* ✅ Use `BigInt` for integers > 2⁵³-1.
* ✅ Always pass radix to `parseInt`.
* ✅ Prefer literals (`42`) over `new Number()`.
* ✅ Use `Math.trunc` instead of bitwise hacks (`x|0`).

---

👉 Mnemonic: **“JS Numbers = Double (64-bit float), Mind Precision & Limits.”**

---

Would you like me to now prepare a **cheat card on `BigInt`** (since it complements Numbers for safe large integer handling)?


Here’s your **Concept Mastery Cheat Sheet** for **Floating-Point Precision** in JavaScript — compact, complete, and with **all gotchas grouped together**.

---

# 🔬 JavaScript Floating-Point Precision — Cheat Card

**Concept:**
All JS numbers use **IEEE-754 double-precision floating point (64-bit)**.

* 53 bits of precision → ~15–17 decimal digits.
* Some decimal fractions **cannot be represented exactly**.

---

### Classic Issue

```js
0.1 + 0.2; // 0.30000000000000004
0.3 - 0.2; // 0.09999999999999998
```

---

### Why?

* Binary floating point can’t exactly store decimals like 0.1 or 0.2.
* Stored as closest binary fraction → rounding errors.

---

### Tolerant Comparison

```js
function nearlyEqual(a,b,eps=Number.EPSILON){
  return Math.abs(a-b) < eps;
}
nearlyEqual(0.1+0.2, 0.3); // true
```

* `Number.EPSILON ≈ 2.22e-16` → smallest difference detectable.

---

### Workarounds

1. **Rounding**

```js
Number((0.1+0.2).toFixed(2)); // 0.3
```

2. **Integer Math (Scaling)**

```js
(0.1*10 + 0.2*10)/10; // 0.3
```

3. **Libraries**

* Use **big.js**, **decimal.js**, **bignumber.js** for financial/math-critical apps.

---

### Gotchas ⚠️ (Exhaustive)

1. **Equality fails**

   * Direct comparison unreliable: `(0.1+0.2)===0.3` → false.
2. **Large vs small numbers**

   * Precision shrinks as numbers grow.
   * `9007199254740993 === 9007199254740992` → true (past safe int).
3. **Division quirks**

   * `1/49*49` ≠ `1`.
4. **Associativity issues**

   * `(a+b)+c !== a+(b+c)` with floats.
5. **JSON.stringify**

   * Converts numbers to shortest decimal that round-trips, may look odd.
6. **toFixed rounding**

   * `1.005.toFixed(2)` → "1.00" (binary fraction issue).
7. **Math.max precision**

   * Very large values may overflow to Infinity.
8. **-0 quirks**

   * `1/-Infinity` → -0.
   * `-0===0` → true, but `Object.is(-0,0)` → false.
9. **Bitwise ops**

   * Coerce floats to 32-bit signed ints → precision loss.

---

### Best Practices

* ✅ Avoid strict equality with decimals; use tolerance.
* ✅ Use integer math when possible (e.g., cents instead of dollars).
* ✅ For critical precision (finance, crypto), use decimal libraries.
* ✅ Always test rounding (e.g., `.toFixed`, `.toPrecision`).
* ✅ Use `BigInt` only for integers, not fractions.

---

👉 Mnemonic: **“Floats Lie — Compare with EPSILON, Scale to Integers, or Use Big Decimal.”**

---

Would you like me to next prepare a **cheat card on BigInt vs Number**, since BigInt was introduced to solve part of this precision problem?


Here’s your **Concept Mastery Cheat Sheet** for **`Number.MAX_SAFE_INTEGER` & `Number.EPSILON`** in JavaScript — compact, practical, and with **all gotchas grouped together**.

---

# 🔢 JavaScript `Number.MAX_SAFE_INTEGER` & `Number.EPSILON` — Cheat Card

---

### `Number.MAX_SAFE_INTEGER`

```js
console.log(Number.MAX_SAFE_INTEGER); 
// 9007199254740991  (2^53 - 1)
```

* Largest integer that can be **safely represented** in JS (`IEEE-754`).
* Past this, integers lose precision → rounding errors.

**Check safety:**

```js
Number.isSafeInteger(9007199254740991); // true
Number.isSafeInteger(9007199254740992); // false
```

---

### `Number.EPSILON`

```js
console.log(Number.EPSILON); 
// 2.220446049250313e-16
```

* The **smallest difference** between 1 and the next representable float.
* Useful for comparing floating-point numbers.

**Example:**

```js
function nearlyEqual(a,b){
  return Math.abs(a-b) < Number.EPSILON;
}
nearlyEqual(0.1+0.2,0.3); // true
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Precision limit**

   * Beyond `MAX_SAFE_INTEGER`, not all integers are unique.

   ```js
   9007199254740992 === 9007199254740993; // true (!)
   ```
2. **BigInt fix**

   * Use `BigInt` for integers larger than ±(2^53 - 1).

   ```js
   9007199254740992n + 1n; // works fine
   ```
3. **EPSILON scale**

   * Too small for big numbers.
   * Works best when numbers are close to 1.

   ```js
   1e20 + 1 === 1e20; // true, EPSILON can’t detect
   ```

   * Use **relative epsilon**:

     ```js
     function nearlyEqualRel(a,b,eps=Number.EPSILON){
       return Math.abs(a-b) < eps * Math.max(Math.abs(a),Math.abs(b));
     }
     ```
4. **Not absolute precision**

   * `EPSILON` is machine precision, not a universal tolerance.
   * Depends on magnitude of compared numbers.
5. **JSON/serialization**

   * Large numbers beyond safe integer may silently round in `JSON.stringify`.
6. **Math operations**

   * Repeated additions/subtractions of tiny decimals accumulate error.

---

### Best Practices

* ✅ Use `Number.isSafeInteger()` before arithmetic on large ints.
* ✅ Use `BigInt` for cryptography, counters, IDs, money.
* ✅ Use `Number.EPSILON` (or relative epsilon) for float comparisons.
* ✅ Document when you rely on float tolerance in critical systems.
* ❌ Don’t rely on equality checks for floats (`===`).

---

👉 Mnemonic:

* **MAX_SAFE_INTEGER** = integer safety limit.
* **EPSILON** = float equality tolerance.

---

Would you like me to now create a **cheat card comparing `Number` vs `BigInt`** (safe integers vs arbitrary precision), since that’s a natural follow-up to MAX_SAFE_INTEGER?

Here’s your **Concept Mastery Cheat Sheet** for **BigInt** in JavaScript — crisp, complete, and with **all gotchas grouped together**.

---

# 🔢 JavaScript BigInt — Cheat Card

**Concept:**
`BigInt` is a primitive type (ES2020) for representing **arbitrary-precision integers**, beyond the safe range of `Number`.

---

### Syntax

```js
const big1 = 123n;        // literal
const big2 = BigInt(123); // constructor
```

---

### Operations

```js
10n + 20n;     // 30n
100n - 50n;    // 50n
5n * 2n;       // 10n
7n / 2n;       // 3n (integer division, no decimals)
7n % 2n;       // 1n
```

* Supports `+ - * / % **`.
* Bitwise ops work (`& | ^ << >>`).
* Comparisons work with Numbers:

  ```js
  10n == 10;   // true (loose)
  10n === 10;  // false (strict)
  11n > 10;    // true
  ```

---

### Conversions

```js
Number(10n);      // 10
BigInt("9007199254740993"); // valid string
```

---

### Gotchas ⚠️ (Exhaustive)

1. **No mixing with Numbers**

   * Arithmetic between `BigInt` and `Number` throws:

     ```js
     10n + 1; // ❌ TypeError
     ```
   * Must convert explicitly.
2. **Division truncates**

   * Always integer: `7n/2n → 3n`, never decimals.
3. **JSON incompatibility**

   * `JSON.stringify(10n)` → ❌ TypeError.
   * Must convert manually (`.toString()`).
4. **Math functions unsupported**

   * `Math.sqrt(16n)` ❌ TypeError.
   * Need custom bigint math or libraries.
5. **Performance**

   * Slower than `Number` (arbitrary precision). Use only when needed.
6. **Boolean coercion**

   * `if (0n)` → false; all other BigInts → true.
7. **Bitwise ops**

   * Work fine but limited to bigint semantics.
8. **Literal suffix**

   * `123n` works; cannot write `0.5n` (no decimals).
9. **Edge case conversions**

   * `Number.MAX_SAFE_INTEGER+2 === Number.MAX_SAFE_INTEGER+1` → true
   * `BigInt(Number.MAX_SAFE_INTEGER)+2n` → correct.
10. **Web APIs**

    * Not all browser APIs support BigInt yet (e.g., IndexedDB keys).

---

### Best Practices

* ✅ Use `BigInt` for IDs, counters, cryptography, financial/large integer ops.
* ✅ Convert explicitly when mixing with `Number`.
* ✅ Use `.toString()` for serialization.
* ❌ Don’t use for decimals/floats.
* ✅ Stick to `Number` when within safe integer range (faster).

---

👉 Mnemonic: **“BigInt = Arbitrary precision ints, but no floats, no JSON, no Math.”**

---

Would you like me to prepare a **cheat card on BigInt vs Number (side-by-side comparison)** — since that’s often an interview favorite?


Here’s your **Concept Mastery Cheat Sheet** for **Symbols** in JavaScript — precise, practical, with **all gotchas grouped together**.

---

# 🌀 JavaScript Symbols — Cheat Card

**Concept:**
A `Symbol` is a **unique and immutable primitive** (ES6) used mainly as **object property keys** to avoid collisions.

---

### Syntax

```js
const sym1 = Symbol("desc");
const sym2 = Symbol("desc");
sym1 === sym2; // false (always unique)

// Global symbol registry
const g1 = Symbol.for("id");
const g2 = Symbol.for("id");
g1 === g2; // true
```

---

### Use as Keys

```js
const id = Symbol("id");
const user = { [id]: 123 };
console.log(user[id]); // 123
```

* Symbols don’t collide with string keys.
* Useful for internal or “hidden” properties.

---

### Well-Known Symbols

```js
class MyCollection {
  [Symbol.iterator]() { return [1,2,3][Symbol.iterator](); }
}
for (const x of new MyCollection()) console.log(x); // 1,2,3
```

* `Symbol.iterator` → iteration protocol.
* `Symbol.toPrimitive` → custom type coercion.
* `Symbol.hasInstance`, `Symbol.species`, etc.

---

### Introspection

```js
Object.getOwnPropertySymbols(user); // [Symbol(id)]
Reflect.ownKeys(user); // ["name", Symbol(id)]
```

* Not returned by `Object.keys` or `for...in`.
* Returned by `Object.getOwnPropertySymbols` / `Reflect.ownKeys`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not auto-convertible to string**

   ```js
   "id:"+Symbol("x"); // ❌ TypeError
   String(Symbol("x")); // "Symbol(x)"
   ```
2. **Uniqueness**

   * Each `Symbol("desc")` is unique, regardless of description.
   * Use `Symbol.for()` to reuse across modules.
3. **Hidden, not private**

   * Symbols hide keys from normal iteration, but anyone with reference can access them.
   * Not true encapsulation (use `#private` fields in classes for that).
4. **JSON ignore**

   * `JSON.stringify({[Symbol("id")]:123})` → "{}".
   * Symbols dropped silently.
5. **Object property order**

   * Symbol keys appear after string keys in `Reflect.ownKeys`.
6. **Global registry leaks**

   * `Symbol.for()` uses global registry → can cause accidental sharing if names collide.
7. **Performance**

   * Overuse may slow down property lookups slightly (not optimized like string keys).
8. **Well-known symbols subtlety**

   * Some (like `Symbol.toPrimitive`) can cause surprising coercion behavior if misused.

---

### Best Practices

* ✅ Use symbols for **internal keys** or meta-properties.
* ✅ Use `Symbol.for` when you need **shared registry keys**.
* ✅ Use well-known symbols to customize built-in behavior (`iterator`, `toStringTag`, etc.).
* ❌ Don’t use Symbols as replacements for private fields.
* ✅ Document symbol usage clearly — they’re not visible via normal inspection.

---

👉 Mnemonic: **“Symbols = Unique keys, hidden from loops, but not truly private.”**

---

Would you like me to follow this with a **cheat card on Well-Known Symbols** (like `Symbol.iterator`, `Symbol.toStringTag`, etc.), since they’re often tested in interviews?

Here’s your **Concept Mastery Cheat Sheet** for **Well-Known Symbols** in JavaScript — focusing on the most common ones: `Symbol.iterator`, `Symbol.asyncIterator`, `Symbol.toStringTag` — with all **gotchas grouped together**.

---

# 🌀 JavaScript Well-Known Symbols — Cheat Card

**Concept:**
Special built-in `Symbol` values that let objects customize language behaviors.

---

### `Symbol.iterator`

```js
const arr = [1,2,3];
const it = arr[Symbol.iterator]();
console.log(it.next()); // {value:1,done:false}

for (const x of arr) console.log(x); // 1,2,3

const custom = {
  *[Symbol.iterator]() { yield 1; yield 2; }
};
[...custom]; // [1,2]
```

* Defines **synchronous iteration protocol**.
* Enables `for...of`, spread `...`, destructuring, etc.

---

### `Symbol.asyncIterator`

```js
const asyncIterable = {
  async *[Symbol.asyncIterator](){
    yield "a"; yield "b";
  }
};
for await (const x of asyncIterable) console.log(x); // a, b
```

* Defines **asynchronous iteration**.
* Enables `for await...of`.

---

### `Symbol.toStringTag`

```js
const obj = { [Symbol.toStringTag]:"Custom" };
Object.prototype.toString.call(obj); // "[object Custom]"
```

* Customizes default `Object.prototype.toString`.
* Used by many built-ins (`[object Array]`, `[object Map]`).

---

### Other Well-Known Symbols (mention)

* `Symbol.hasInstance` → customize `instanceof`.
* `Symbol.isConcatSpreadable` → control array concat.
* `Symbol.match`, `Symbol.replace`, `Symbol.search`, `Symbol.split` → regex behavior.
* `Symbol.toPrimitive` → customize type coercion.
* `Symbol.unscopables` → exclude props from `with` scope.
* `Symbol.species` → control derived constructors.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not enumerable**

   * Well-known symbol props don’t show in `for...in` / `Object.keys`.
   * Must use `Object.getOwnPropertySymbols` or `Reflect.ownKeys`.
2. **Iterator protocol requirements**

   * `next()` must return `{value, done}`.
   * Forgetting `done:true` → infinite loop.
3. **Async iterator quirks**

   * Requires `for await...of`. Using `for...of` on async iterable → ❌ TypeError.
   * Works only in async functions.
4. **`toStringTag` only affects `Object.prototype.toString`**

   * Doesn’t change `typeof`.
   * Misleading if abused.
5. **Custom `[Symbol.iterator]` on non-iterables**

   * Enables `for...of` on arbitrary objects → can confuse maintainers.
6. **Cross-realm issues**

   * Different iframes/windows may not share constructors, but symbols themselves are global across realms.
7. **`Symbol.toPrimitive` vs `valueOf/toString`**

   * Precedence rules subtle. If all three defined, coercion picks in order.
8. **Overriding built-ins**

   * Can break expected behavior (e.g., setting `Symbol.isConcatSpreadable=false` on arrays).
9. **Performance**

   * Custom iterators may be slower than built-ins.

---

### Best Practices

* ✅ Use `Symbol.iterator` for custom data structures.
* ✅ Use `Symbol.asyncIterator` for streams, async data sources.
* ✅ Use `Symbol.toStringTag` only for debugging/logging clarity.
* ✅ Always implement iterator/asyncIterator correctly (return `{done:true}`).
* ❌ Don’t abuse well-known symbols to “hack” language behavior unexpectedly.
* ✅ Document when overriding built-ins with custom symbols.

---

👉 Mnemonic: **“iterator = sync loop, asyncIterator = async loop, toStringTag = identity label.”**

---

Would you like me to also prepare a **cheat card dedicated to Iterator vs Iterable vs AsyncIterable protocols** (since they’re often tested alongside these symbols)?


Here’s your **Concept Mastery Cheat Sheet** for **Dates & the `Temporal` API** in JavaScript — covering legacy pitfalls and the new modern API, with **all gotchas grouped together**.

---

# 📅 JavaScript Dates & Temporal API — Cheat Card

**Concept:**

* **`Date`** (ES1) = legacy object for timestamps.
* **`Temporal`** (Stage 3 TC39) = modern, reliable API for dates/times (proposed replacement).

---

### Legacy `Date`

```js
const d1 = new Date();                  // now
const d2 = new Date("2025-09-28");      // parse string (UTC vs local quirks)
const d3 = new Date(2025, 8, 28, 7, 30);// YYYY,MM(0-based),DD,HH,MM

d1.getFullYear();   // year
d1.getMonth();      // 0–11
d1.getDate();       // 1–31
d1.getDay();        // 0–6 (Sun–Sat)
d1.getTime();       // ms since epoch
d1.toISOString();   // UTC string
Date.now();         // epoch ms (fast)
```

* Stores as **ms since Jan 1 1970 UTC**.
* Methods split into local vs UTC (`getHours` vs `getUTCHours`).

---

### Temporal API (New)

```js
// Absolute times
const inst = Temporal.Now.instant();    // current exact instant
inst.toString();                        // "2025-09-28T02:30:00Z"

// Calendar dates
const date = Temporal.PlainDate.from("2025-09-28");
date.add({days:1});                     // 2025-09-29

// Time + date
const dt = Temporal.PlainDateTime.from("2025-09-28T07:30");
dt.with({hour:10});                     // 2025-09-28T10:30

// Zoned
const zdt = Temporal.Now.zonedDateTimeISO();
zdt.toString(); // "2025-09-28T08:00:00+05:30[Asia/Kolkata]"
```

**Main Types:**

* `Temporal.Instant` → absolute point in time (like `Date`).
* `Temporal.PlainDate`, `PlainTime`, `PlainDateTime` → human-readable parts.
* `Temporal.ZonedDateTime` → date-time with explicit timezone.
* `Temporal.Duration` → span of time.
* `Temporal.Calendar`, `Temporal.TimeZone` → metadata.

---

### Gotchas ⚠️ (Exhaustive)

1. **`Date` quirks**

   * Months are **0-based** (`Jan = 0`).
   * Parsing inconsistent across engines/locales.
   * Timezone offsets implicit (local system).
   * Mutability: `date.setFullYear(...)` mutates object.
   * Leap seconds ignored.
   * Max safe range: ±8.64e15 ms (~±285,000 years).
2. **Temporal advantages**

   * Immutable objects (safe).
   * Timezones explicit.
   * Precise arithmetic (`date.add({days:1})`).
   * Built-in calendar support.
   * Consistent parsing (ISO 8601).
3. **Temporal not yet standard everywhere**

   * Still proposal (Stage 3). Needs polyfill (`proposal-temporal`).
   * Not available in all runtimes (as of 2025).
4. **Conversions**

   * `Date` ↔ `Temporal`:

     ```js
     Temporal.Instant.fromEpochMilliseconds(Date.now());
     new Date(Temporal.Now.instant().epochMilliseconds);
     ```
5. **Performance**

   * Temporal heavier than Date (more objects, immutability). Use `Date.now()` for perf-critical timestamps.
6. **Serialization**

   * JSON.stringify(new Date()) → ISO string.
   * Temporal not natively serialized; use `.toString()`.

---

### Best Practices

* ✅ Use `Date.now()` for quick timestamps.
* ✅ Use `Temporal` (polyfill) for new projects needing robust date/time.
* ✅ Always handle timezones explicitly (ZonedDateTime).
* ✅ Use `.toISOString()` for storage/serialization.
* ❌ Don’t rely on Date parsing of non-ISO strings.
* ❌ Don’t mutate Date objects unless necessary.

---

👉 Mnemonic:

* **Date = legacy, mutable, quirky**
* **Temporal = modern, immutable, precise**

---

Would you like me to prepare a **side-by-side “Date vs Temporal” comparison cheat card** (with use-cases, pros/cons, and migration tips)?


Here’s your **Concept Mastery Cheat Sheet** for **Regular Expressions (RegExp)** in JavaScript — dense but clear, with **all gotchas grouped together**.

---

# 🔍 JavaScript Regular Expressions (RegExp) — Cheat Card

**Concept:**
RegExps are objects describing patterns for matching, searching, and replacing text.

---

### Creation

```js
const re1 = /abc/;             // literal
const re2 = new RegExp("abc"); // constructor
```

---

### Flags

* `g` → global (all matches)
* `i` → case-insensitive
* `m` → multiline (`^/$` match line boundaries)
* `s` → dotAll (`.` matches newlines)
* `u` → Unicode mode (full code points)
* `y` → sticky (match from lastIndex only)

---

### Character Classes

* `.` any char (except newline unless `s`)
* `\d` digit `[0-9]`
* `\w` word `[A-Za-z0-9_]`
* `\s` whitespace
* `\D`, `\W`, `\S` = negations
* Custom: `[abc]`, ranges `[a-z]`, negated `[^a-z]`

---

### Quantifiers

* `*` 0+
* `+` 1+
* `?` 0/1
* `{n}`, `{n,}`, `{n,m}`
* Greedy by default → add `?` for lazy: `.+?`

---

### Anchors & Boundaries

* `^` start, `$` end
* `\b` word boundary, `\B` non-boundary

---

### Groups

* `(abc)` capturing
* `(?:abc)` non-capturing
* `(?<name>abc)` named capture
* Backrefs: `\1`, `\k<name>`

---

### Assertions

* Lookahead: `X(?=Y)` (followed by Y)
* Negative lookahead: `X(?!Y)`
* Lookbehind (ES2018+): `(?<=Y)X` / `(?<!Y)X`

---

### Methods

```js
/ab/.test("abc");       // true
"abc".match(/a./);      // ["ab"]
"abc".match(/a./g);     // ["ab"]
"abc".search(/b/);      // 1
"abc".replace(/b/,"x"); // "axc"
"abc".split(/b/);       // ["a","c"]

const r=/a/g;
r.exec("aab");          // {0:"a",index:0}
r.exec("aab");          // {0:"a",index:1} (g advances lastIndex)
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Escaping hell**

   * In string regex: `new RegExp("\\d+")` (double escapes).
2. **lastIndex with `g`/`y`**

   * Statefulness can cause bugs if not reset.

   ```js
   const r=/a/g; r.test("a"); r.test("a"); // true, false
   ```
3. **Greedy quantifiers**

   * `/.*/` matches whole string; add `?` for minimal match.
4. **Dot (`.`) limits**

   * Doesn’t match newlines unless `s` flag.
5. **Unicode quirks**

   * `"😀".length===2`, `/./.test("😀")` → true but matches half surrogate.
   * Use `u` flag: `/./u`.
6. **Lookbehind support**

   * Not in older engines.
7. **Performance traps**

   * Catastrophic backtracking with nested quantifiers:

     ```js
     /(a+)+$/ on "aaaaaaaaaaaaaaaaaaaa!"
     ```
   * Can hang program.
8. **Anchors in multiline**

   * `^` and `$` match per-line only with `m` flag.
9. **Replace pitfalls**

   * `$&` = matched substring, `$1` = group1, `$$` = literal $.
10. **Regex literals vs constructor**

    * Literal compiled once, constructor each time.
11. **JSON/stringify**

    * `JSON.stringify(/a/g)` → "{}" (not preserved).
12. **Empty regex**

    * `//` invalid. Use `new RegExp("")`.

---

### Best Practices

* ✅ Always use `u` flag for Unicode safety.
* ✅ Use named groups for readability.
* ✅ Reset `lastIndex` before reusing global/sticky regex.
* ✅ Escape user input when building dynamic regex.
* ✅ Avoid catastrophic patterns; use tools like [regex101.com](https://regex101.com/).
* ✅ Prefer non-capturing groups unless capture needed.

---

👉 Mnemonic: **“Regex = Pattern + Flags + State traps.”**

---

Would you like me to also prepare a **cheat card on RegExp vs String methods (`match`, `matchAll`, `replace`, `split`, `search`)** since many interview bugs come from mixing them up?


Here’s your **Concept Mastery Cheat Sheet** for **RegExp Flags, Groups, and Lookarounds (Lookaheads & Lookbehinds)** in JavaScript — concise, deep, and with **all gotchas grouped together**.

---

# 🚩 JavaScript RegExp — Flags, Groups & Lookarounds

---

### 🔖 Flags

```js
/g  // global — find all matches
/i  // case-insensitive
/m  // multiline (^,$ = line boundaries)
/s  // dotAll (dot matches newline)
/u  // Unicode mode (full code points)
/y  // sticky (match only at lastIndex)
```

* Flags can be combined: `/abc/gi`.

---

### 🎭 Groups

```js
/(abc)/         // capturing group
/(?:abc)/       // non-capturing
/(?<name>abc)/  // named group
```

* Captures accessed in results:

  ```js
  /(a)(b)/.exec("ab"); // ["ab","a","b"]
  ```
* Named backref: `\k<name>`

---

### 🔄 Backreferences

```js
/(a)b\1/.test("aba");  // true
/(?<d>\d+)-\k<d>/.test("42-42"); // true
```

---

### 👁️ Lookaheads

```js
X(?=Y)   // positive lookahead
X(?!Y)   // negative lookahead

/\d(?=px)/.exec("10px"); // "0"
```

* Matches `X` if followed by `Y`.

---

### 👁️ Lookbehinds (ES2018+)

```js
(?<=Y)X  // positive lookbehind
(?<!Y)X  // negative lookbehind

/(?<=\$)\d+/.exec("$99"); // "99"
```

* Matches `X` if preceded by `Y`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Flags**

   * `g` + `exec/test` are stateful (`lastIndex` must be reset).
   * `m` only affects `^`/`$`.
   * `s` needed for `.` to match `\n`.
   * `u` required for Unicode > BMP (e.g. emoji).
   * `y` requires match at `lastIndex` → fails otherwise.
2. **Groups**

   * Capturing groups slow regex if unused. Prefer `(?: )`.
   * Named groups not supported in very old browsers.
   * Unmatched groups return `undefined`, not `""`.
3. **Backrefs**

   * Reference numbering depends on left-to-right order of `(`.
   * Nested groups increase count; miscount = bugs.
4. **Lookaheads/Lookbehinds**

   * Zero-width → consume no characters.
   * Lookbehinds require fixed-width patterns in some engines.
   * Negative lookbehind/lookahead easy to misuse: `X(?!Y)` still matches `X`.
   * Catastrophic backtracking possible if nested inside quantifiers.
5. **Unicode quirks**

   * Without `u`, `.` and quantifiers can split surrogate pairs (emoji).
6. **Performance**

   * Heavy use of capturing/backrefs/lookarounds can degrade performance.

---

### Best Practices

* ✅ Use `u` + `s` when working with text containing Unicode & newlines.
* ✅ Prefer **named groups** for readability.
* ✅ Use `(?: )` for grouping without capture.
* ✅ Reset `lastIndex` when reusing `/g` or `/y` regex.
* ✅ Keep lookaheads/behinds simple (avoid nested quantifiers).
* ❌ Don’t rely on group numbers in large regex → use names.

---

👉 Mnemonic:

* **Flags = search mode**
* **Groups = capture or cluster**
* **Lookaround = check before/after without consuming**

---

Would you like me to also prepare a **cheat card just on “Regex Backtracking & Catastrophic Patterns”** (since interviewers often test efficiency issues with groups + lookarounds)?


Here’s your **Concept Mastery Cheat Sheet** for **Sticky (`y`), Global (`g`), and DotAll (`s`) regex flags** in JavaScript — crisp, with **all gotchas grouped together**.

---

# 🚩 JavaScript RegExp Flags — Sticky, Global, DotAll

---

### 🔁 Global (`g`)

```js
const re = /a/g;
"a_a_a".match(re); // ["a","a","a"]
```

* Finds **all matches**.
* Advances `lastIndex` internally with `.exec()` or `.test()`.
* Works across whole string.

---

### 📍 Sticky (`y`)

```js
const re = /a/y;
re.lastIndex = 2;
"__a_a".match(re); // ["a"] (must match at index 2)
```

* Matches **only at `lastIndex`**.
* Fails if pattern not found exactly there.
* Good for tokenizers, incremental parsing.

---

### ⚫ DotAll (`s`)

```js
/hello.world/.test("hello\nworld");  // false
/hello.world/s.test("hello\nworld"); // true
```

* Makes `.` match **all characters**, including `\n`.
* Without `s`, dot excludes line breaks.

---

### Gotchas ⚠️ (Exhaustive)

1. **Global (`g`)**

   * `.exec()` and `.test()` are **stateful**:

     ```js
     const r=/a/g; r.test("a"); r.test("a"); // true, false
     ```
   * Forgetting to reset `lastIndex` causes missed matches.
   * `String.match` with `g` → returns array of matches, **no capture groups**.
2. **Sticky (`y`)**

   * Requires `lastIndex` control. If `lastIndex` wrong → always fail.
   * Ignores matches not starting exactly at `lastIndex`.
   * Useful for step-by-step parsing but rarely needed for simple search.
   * Not widely known → less portable in older engines.
3. **DotAll (`s`)**

   * Changes meaning of `.` drastically → can make regex more expensive.
   * Without `s`, use `[^]` trick for “match anything” (but less readable).
   * Still respects Unicode if combined with `u`.
4. **Combining flags**

   * `/pattern/gs` → global + dotAll, safe for multi-line text.
   * `/pattern/my` → multiline + sticky works but tricky to debug.
5. **lastIndex pitfalls**

   * Both `g` and `y` mutate `lastIndex` when matching.
   * If you reuse regex object, reset manually: `re.lastIndex=0`.
6. **Performance**

   * Sticky can be faster for anchored matching (no backtracking before lastIndex).
   * DotAll may slow regex if pattern spans large text.

---

### Best Practices

* ✅ Use `g` when you need **all matches**, but don’t rely on capture groups with `match`.
* ✅ Use `matchAll` (`/pattern/g`) for captures:

  ```js
  [..."a1a2".matchAll(/a(\d)/g)];
  ```
* ✅ Use `y` for parsing/tokenization where position matters.
* ✅ Use `s` for multi-line text matching readability.
* ❌ Don’t forget `lastIndex` with `g`/`y`.
* ✅ Combine `gs` for “find everything, even across lines”.

---

👉 Mnemonic:

* **g = all matches**
* **y = must stick at lastIndex**
* **s = dot matches everything**

---

Would you like me to also prepare a **cheat card on `lastIndex` and stateful regex behavior** (since it’s the key confusion point with `g` and `y`)?
