### Gotchas ⚠️ (Exhaustive)

1. **ASI (Automatic Semicolon Insertion)**

2. **Hoisting quirks**

   - `var`: hoisted + initialized as `undefined`.
   - `let`/`const`: hoisted but in **TDZ** until declaration.
   - `TDZ`: Temporary Dead Zone is the period of time between the variable declaration and its initialization.
   - `Hoisting`: is the process of moving declarations to the top of their scope during the compilation phase.
   - `function and variables` are hoisted.
   - `Function expressions/arrow functions` **not hoisted** in function expression only variables are hoisted but not the function assigned to variable.

3. **`var` vs `let/const`**

   - `var` ignores block scope.
   - `let/const` are block-scoped.
   - `const` requires initializer.

4. **Block vs object literal**

   - `{}` is dual-purpose — block in statement context, object literal in expression context.

   ```js
   // block+label not object
   {
     a: 1;
   } // block statement with a label a and expression 1,
   // object literal
   let obj = { a: 1 }; // because it’s after =, the parser knows {} must be an expression, so it’s an object
   ({ a: 1 }); // force it to be an object
   ```

5. **Switch fall-through**

   - Missing `break` → executes next cases unintentionally.

6. **`for…in` vs `for…of`**

   - `for…in`: iterates enumerable keys (incl. inherited).
   - `for…of`: iterates values (requires iterable).
   - Order not guaranteed in `for…in`.

7. **Labels**

   - `break`/`continue` can jump to labels → confusing spaghetti code.

8. **Strict mode (`"use strict"`)**

   - Assignment to undeclared variable → ReferenceError.
   - `this` in functions = `undefined` (not global).
   - Silent failures become thrown errors.

9. **Imports/Exports**

   - Must be top-level (not inside functions/blocks). // gives syntax errors -> coz they are not hoisted, compiler needs to know dependencies before execution

   ```js
   if (cond) {
     import { x } from "./lib.js"; // SyntaxError
   }
   if (cond) {
     const { default: devTool } = await import("./dev.js"); // ✅ dynamic import() allows conditional imports and is also hoisted as it is a function expression
   }
   ```

   - Are **static** → names fixed at parse time.

   ```js
   import { value } from "./lib.js";
   value = 5; // ❌ TypeError, read-only binding, but if the content can be changed from the imported file, then it will be allowed, as values referenced not copied
   ```

10. **Catch block quirks**

    - `catch(e)` shadows outer `e`.
    - Errors swallowed if not re-thrown.

11. **Reserved words**

    - Keywords like `class`, `import` can’t be used as identifiers.
    - Some old reserved words (`enum`, `implements`) disallowed in strict mode.

12. **Unicode confusion**

    - Visually identical characters (e.g., Greek `Σ` vs Latin `S`) can slip into identifiers.

13. **Legacy quirks**

    - `with` is deprecated (still parses in sloppy mode).
    - `eval` can inject new variables into scope (sloppy mode).

---

### Best Practices

- Prefer `const` → `let`; avoid `var`.
- Wrap all blocks in `{}` (no single-line shortcuts).
- Use `for…of` or array methods (`map`, `forEach`) over `for…in`.
- Enable `"use strict"` or default to ES modules.
- Avoid labels, `with`, `eval`.

### Scope & Lifetime

- **Validity:**
  - Statements live in function, block, or global scope.
- **Creation moment:**
  - Declared variables are hoisted at parse-time.
- **Destruction:**

  - Block scope → end of block.
  - Function scope → when function returns.
  - GC removes unreachable objects.

  8. **Arrays**

  - Length auto-updates on push/pop, but `delete arr[i]` leaves hole.
  - `arr.length=0` clears whole array.

9. **Functions**

   - `First-class functions` can be assigned to variables, passed as arguments, returned from functions.
   - `typeof function(){} === "function"` (only special case).

10. **WeakMap/WeakSet**

    - Keys must be objects.
    - Not iterable, no size property → can’t enumerate.

    7. **Spread/Rest**

    - Works only on **iterables Values** (Array, string, Set).
    - Spreading non-iterable (`...123`) → TypeError.
    - Object spread ignores non-enumerable props i.e.
    - hidden properties of objects( `__proto__, __defineGetter__, __defineSetter__` etc. ) and arrays(`length, push, pop` etc. these are not enumerable and are not spread in object spread).
    - Iterable → "Can I loop its values?" (Array, string, Maps, Sets), used by
    - (for...of, spread, destructuring)
    - Enumerable → "If property shown up in key listings?" (objects, blocks{a:1}), used by - (for...in, Object.keys)

    8. **Default exports in ES modules**

    - Function declarations exported as default are named, only if explicitly given; else function expressions may show as `default`.

    ```js
    export default function foo() { } // named -> foo
    export default function () { } // anonymous -> default
    ```

# 🎯 JavaScript `this` Binding Rules — Cheat Card

**Concept:**
`this` refers to the **execution context** (how a function is called), not where it’s defined.

---

### 1. Global / Default Binding

```js
console.log(this); // Browser: window, Node: global
function f() {
  console.log(this);
}
f(); // Browser: window (sloppy), undefined (strict)
```

---

### 2. Implicit Binding (Object Method)

```js
const obj = {
  x: 42,
  getX() {
    return this.x;
  },
};
obj.getX(); // 42
```

- `this` = object before the dot.

---

### 3. Explicit Binding

```js
function f() {
  return this.x;
}
const obj = { x: 10 };
f.call(obj); // 10
f.apply(obj); // 10
const g = f.bind(obj);
g(); // 10
```

---

### 4. `new` Binding (Constructor)

```js
function Person(name) {
  this.name = name;
}
const p = new Person("Kalidas");
p.name; // "Kalidas"
```

- `new` creates new object, sets it as `this`.

---

### 5. Arrow Functions (Lexical `this`)

```js
const obj = { x: 1, f: () => console.log(this.x) };
obj.f(); // undefined (this not obj, inherits outer)
```

- `this` is inherited from surrounding scope.
- Cannot be changed with `call/apply/bind`.

---

### 6. Class Methods

```js
class C {
  m() {
    return this;
  }
}
new C().m(); // instance
```

- Behaves like object methods.
- Not auto-bound; lose `this` if detached.

---

### 7. Event Handlers

```js
btn.onclick = function () {
  console.log(this);
}; // element
btn.onclick = () => console.log(this); // lexical (e.g. window)
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Strict vs sloppy mode**
   - In sloppy: `this` in functions = global.
   - In strict: `this` = undefined.
2. **Method extraction loses context**
   ```js
   const m = obj.getX;
   m(); // undefined
   ```
3. **Arrow function traps**
   - Arrow inside method captures outer `this`, not the object.
   - Useful in callbacks, harmful if expecting dynamic binding.
4. **bind vs arrow**
   - Arrow cannot be rebound with `.call/.bind/.apply`.
   - Bound functions override `this` permanently.
5. **new with arrow ❌**
   - Arrow funcs cannot be constructors.
6. **Default binding precedence**
   - Order: `new` > explicit (`bind`) > implicit (obj call) > global/default.
7. **setTimeout / async callbacks**
   - Plain functions lose `this`.
   - Fix: arrow function or `.bind(this)`.
8. **Classes & strict**
   - All class bodies run in strict mode → `this` never global inside.
9. **Getters/Setters**
   - `this` refers to object the property is accessed on.
10. **Prototype chain**
    - If method inherited, `this` still bound to calling object, not prototype.

---

### Best Practices

- ✅ Use **arrow functions** for callbacks needing lexical `this`.
- ✅ Use **bind** when passing methods as callbacks.
- ✅ Avoid relying on default/global binding.
- ✅ Keep `this` usage consistent within a module/class.
- ✅ In modern code, prefer `class fields = ()=>{}` for auto-bound methods.

---

## 👉 Mnemonic: **“`new` > explicit > implicit > default. Arrows inherit.”**

## Would you like me to follow this up with a **cheat card on `call`, `apply`, and `bind`** (since they’re the tools to control `this` explicitly)?

## Here’s your **Concept Mastery Cheat Sheet** for **Object vs Map** in JavaScript — compact, side-by-side, and with **all gotchas grouped together**.

# 📊 JavaScript: Array vs Set — Cheat Card

**Concept:**
Both store collections of values, but differ in **duplicates, order, and API**.

---

### Array

```js
const arr = [1, 2, 2, 3];
arr.push(4);
```

- Ordered list of elements.
- Allows **duplicates**.
- Indexed access: `arr[0]`.
- Rich API: `map`, `filter`, `reduce`, `sort`, etc.

---

### Set

```js
const set = new Set([1, 2, 2, 3]);
set.add(4);
```

- Collection of **unique values**.
- Stores **any type**.
- No index access (iterable only).
- Useful for deduplication and membership checks.

---

### API Comparison

| Feature    | Array                          | Set                             |
| ---------- | ------------------------------ | ------------------------------- |
| Duplicates | ✅ allowed                     | ❌ auto-removed                 |
| Order      | Insertion order preserved      | Insertion order preserved       |
| Access     | Index-based (`arr[i]`)         | Value-based (`set.has(v)`)      |
| Size       | `arr.length`                   | `set.size`                      |
| Iteration  | `for`, `for…of`, array methods | `for…of`, `forEach`             |
| Conversion | –                              | `[...set]` or `Array.from(set)` |

---

### Gotchas ⚠️ (Exhaustive)

1. **Uniqueness rules**
   - `Set` uses **SameValueZero** equality:
     - `NaN` == `NaN` (treated as same).
     - `0` and `-0` treated as same.
   - Array allows all duplicates.
2. **Performance**
   - Large lookups: `set.has(val)` faster than `arr.includes(val)`.
   - Array better for indexed/random access.
3. **Iteration order**
   - Both preserve insertion order, but Set can’t be reindexed.
4. **Indexing in Set**
   - No direct access: `set[0]` → `undefined`. Must iterate.
5. **Conversion traps**
   - `new Set(arr)` removes duplicates silently.
   - `[...set]` converts back but may change length.
6. **Mutability**
   - Array → can splice, reorder.
   - Set → no index ops, only add/delete/clear.
7. **Empty slots in arrays**
   - Arrays can have “holes”:
     ```js
     const a = [, , 3];
     a.length; // 3
     ```
   - Set never has holes.
8. **Serialization**
   - `JSON.stringify(new Set([1,2]))` → `{}` (ignored).
   - Convert manually: `[...set]`.
9. **Methods mismatch**
   - Array has higher-order functions (`map`, `reduce`), Set doesn’t.
   - Must convert Set to Array first.
10. **Identity**

- Objects in Set compared by reference.
- Two identical objects `{}` ≠ `{}` in Set.

---

### Best Practices

- ✅ Use **Array** for ordered, index-based data or when duplicates are fine.
- ✅ Use **Set** for uniqueness & fast membership checks.
- ✅ Convert between them as needed (`Array.from(new Set(arr))` for deduplication).
- ❌ Don’t expect Set to behave like Array with indices.
- ✅ Be mindful of serialization (convert Sets before stringifying).

---

## 👉 Mnemonic: **“Array = List (allows duplicates). Set = Unique Bag (fast lookup).”**

## Would you like me to also prepare a **cheat card on Set vs WeakSet** (since that’s the next logical extension and often tested in interviews)?

```

```

### Map

```js
const map = new Map();
map.set("a", 1);
map.set(42, "num");
map.set({}, "obj");
```

- Keys: `any type` (`objects, functions, NaN`).
- Ordered by insertion.
- `m.size` -> length of the map.
- Optimized for frequent add/remove.
- Best for dynamic collections, lookup tables.

#### Comparison

| Feature       | Object                                                                                              | Map                                                                       |
| ------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Key types     | String, Symbol                                                                                      | Any value                                                                 |
| Iteration     | `for…in`, `Object.keys`                                                                             | `map.keys()`, `map.values()`, `map.entries()`, `for…of`                   |
| Order         | Not guaranteed (but stable since ES6 -> integer keys first (ascending), then strings, then symbols) | Insertion order preserved.                                                |
| Size          | `Object.keys(obj).length`                                                                           | `map.size`                                                                |
| Prototype     | Inherits from `Object.prototype`                                                                    | Pure container(no prototype)                                              |
| Performance   | Slower for frequent adds/removes                                                                    | Optimized for adds/removes                                                |
| Serialization | JSON.stringify works                                                                                | Must convert manually manually(`JSON.stringify(Object.fromEntries(map))`) |
| Iteration     | `for…in` includes inherited props (unless guarded with `hasOwn`)                                    | `map.entries()` iterates own entries, safe.                               |
| Key coercion  | Keys auto-stringified                                                                               | Keys type preserved                                                       |
|               | `obj[{a:1}] = "x";`                                                                                 | `map.set({a:1}, "x");`                                                    |
|               | `obj["[object Object]"]`                                                                            | `map.get({a:1})`                                                          |
| Syntax        | `obj.length`                                                                                        | `map.size`                                                                |
|               | `"key" in obj`                                                                                      | `map.has(key)`                                                            |
|               | `delete obj.key`                                                                                    | `map.delete(key)`                                                         |
|               | `Object.keys(obj)`                                                                                  | `map.keys()`                                                              |
|               | `Object.values(obj)`                                                                                | `map.values()`                                                            |
|               | `Object.entries(obj)`                                                                               | `map.entries()`                                                           |

#### Object.create(null) vs Map

##### Choose `Map` when:

- You need **non-string keys** (objects, numbers without string coercion, `NaN`, `-0` vs `0` distinct).
- You do **frequent inserts/deletes** or very **large** collections.
- You need **stable insertion-order iteration** and built-in iterators.
- You care about **O(1) size** via `map.size`.

##### Choose `Object.create(null)` when:

- You want a **pure dictionary of string/symbol keys** with **minimal overhead**.
- You need to avoid **prototype pollution** (no inherited `__proto__`/`constructor`).
- You want **direct JSON** serialization to an object.
- Your key set is **small/medium** and mostly **lookup-heavy**.

---

| Aspect              | `Object.create(null)` (null-proto object)                                 | `Map`                                                                                       |
| ------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Prototype           | **None** (no `toString`, `hasOwnProperty`, etc.)                          | `Map.prototype` methods available                                                           |
| Allowed key types   | **Strings & Symbols** only (non-string keys are coerced to string)        | **Any value**: objects, functions, NaN, 0/-0 distinct, etc.                                 |
| Prototype pollution | **Immune** (no `__proto__` on the prototype chain)                        | N/A (not an object literal)                                                                 |
| Insertion order     | Mostly insertion order but integers iterate in **numeric** order first    | **Strict insertion order** for iteration                                                    |
| Size                | No built-in; must track manually or use `Object.keys(obj).length` (O(n))  | `map.size` (O(1))                                                                           |
| Methods             | None built-in; manual `obj[k] = v`, `delete obj[k]`                       | Rich API: `.set`, `.get`, `.has`, `.delete`, `.clear`, `.keys()`, `.values()`, `.entries()` |
| Iteration           | `for (const k in obj)` (own props only if careful), `Object.keys/entries` | `for (const [k,v] of map)`; easy iterators                                                  |
| Performance         | Great for small/fixed sets & fast lookups; deletes/size calc are weaker   | Typically faster for frequent **add/remove** and large maps                                 |
| JSON serialization  | Direct: `JSON.stringify(obj)`                                             | Not direct; must convert (`Array.from(map)`, or to plain object)                            |
| Memory              | Very lightweight per entry                                                | Slightly higher overhead for structure                                                      |
| Symbol keys         | ✅ supported                                                              | ✅ supported                                                                                |

---






## Concepts


### Map

```js
const m = new Map();
m.set("a", 1);
m.set({}, 2);
m.get("a"); // 1
```

- Key → any type.
- Maintains insertion order.
- `.size`, `.has`, `.delete`, `.clear`.

---

### WeakMap

```js
const wm = new WeakMap();
let obj = {};
wm.set(obj, "secret");
```

- Keys → **objects only**.
- Weak references (GC-eligible).
- Not iterable.

---

### Set

```js
const s = new Set([1, 2, 2, 3]);
s.has(2); // true
```

- Stores **unique values**.
- Insertion order preserved.
- `.size`, `.add`, `.delete`, `.clear`.

---

### WeakSet

```js
const ws = new WeakSet();
let obj = {};
ws.add(obj);
```

- Values → **objects only**.
- Weak references.
- Not iterable.

---

### Typed Arrays & Buffers

```js
const buf = new ArrayBuffer(16); // raw bytes
const view = new Uint8Array(buf);
view[0] = 255; // binary storage
```

- For binary data.
- Types: `Int8Array`, `Uint8Array`, `Float32Array`, etc.
- Backed by `ArrayBuffer` + `DataView`.

---

### Other Structures

- **Date** → `new Date()`, timestamps.
- **RegExp** → `/pattern/flags`.
- **Error types** → `Error`, `TypeError`, `SyntaxError`, etc.
- **BigInt** → integers beyond 2^53-1.

---

### Gotchas ⚠️ (Exhaustive)

1. **Map vs Object**
   - Objects stringify keys; Maps keep key identity.
   - `map.set({},1); map.set({},2);` → 2 entries.
2. **Set uniqueness rules**
   - Uses SameValueZero: `NaN` equal to `NaN`, `0` = `-0`.
3. **WeakMap/WeakSet**
   - Keys must be objects.
   - No `.size`, `.keys`, `.forEach`.
   - Entries removed when object key GC’ed.
4. **Iteration differences**
   - Map/Set → iterable directly (`for…of`).
   - WeakMap/WeakSet → not iterable.
5. **Serialization**
   - JSON ignores Map/Set/WeakMap/WeakSet.
   - Must convert (`Array.from(map)` / `[...set]`).
6. **Typed Arrays quirks**
   - Fixed length (cannot resize).
   - Endianness matters in cross-system binary data.
   - Out-of-bounds access returns `undefined`, not error.
7. **Performance traps**
   - Deep prototype chains slower than Map lookups.
   - Overusing Sets for tiny lists slower than arrays.
8. **RegExp quirks**
   - Statefulness with `lastIndex` in global/sticky regex.
   - `test` on global regex advances index.
9. **BigInt**
   - Cannot mix with numbers directly (`1n + 1` ❌).
   - JSON.stringify(BigInt) → ❌ TypeError.

---

## Best Practices

- ✅ Use **Map** for dynamic key-value collections.
- ✅ Use **Set** for uniqueness.
- ✅ Use **WeakMap/WeakSet** for private data & memory-sensitive caching.
- ✅ Use **TypedArrays** for binary/large numeric data.
- ✅ Always convert before serialization.
- ✅ Keep usage clear — Objects for structure, Maps/Sets for collections.

---

## Mnemonic

- **Map = Key store**,
- **Set = Unique bag**,
- **Weak = GC-friendly**,
- **Typed = Binary data**.

---
