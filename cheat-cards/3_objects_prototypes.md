Here’s your **Concept Mastery Cheat Sheet** for **Objects & Prototypes** in JavaScript — packed with essentials, quirks, and **all gotchas grouped together**.

---

# 🧱 JavaScript Objects & Prototypes — Cheat Card

**Concept:**

- **Objects** = collections of key–value pairs.
- **Prototypes** = hidden links (`[[Prototype]]`) that provide inheritance in JS.

---

### Object Basics

```js
// Literal
const obj = { a: 1, b: 2 };

// Constructor
const obj2 = new Object();

// Create with prototype
const obj3 = Object.create(proto, { x: { value: 42 } });
```

- Keys: strings or symbols.
- Values: any type (including functions → methods).
- `Object.keys`, `Object.values`, `Object.entries`.
- Property attributes: `writable`, `enumerable`, `configurable`.

---

### Prototypes

```js
const animal = { eats: true };
const dog = Object.create(animal);
dog.barks = true;

console.log(dog.eats); // true (via prototype)
console.log(Object.getPrototypeOf(dog) === animal); // true
```

- Every object has an internal `[[Prototype]]` (except `Object.create(null)`).
- Accessed via `Object.getPrototypeOf(obj)` / `Object.setPrototypeOf(obj)` or legacy `__proto__`.
- Functions have `prototype` property → used when creating objects with `new`.

---

### Prototype Chain

- Lookup order: `obj → obj.[[Prototype]] → … → Object.prototype → null`.
- If not found, returns `undefined`.
- Inheritance in JS = delegation via this chain.

---

### Examples

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  return "Hi " + this.name;
};

const p = new Person("Kalidas");
p.sayHi(); // "Hi Kalidas"
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Own vs inherited props**

   - `for...in` iterates **all enumerable** (incl. inherited).
   - Use `obj.hasOwnProperty(key)` or `Object.hasOwn(obj,key)`.

2. **`__proto__` pitfalls**

   - Non-standard historically, now standardized but still discouraged.
   - Setting via `obj.__proto__=x` is slow. Use `Object.create` instead.

3. **Shadowing prototype methods**

   ```js
   obj.toString = () => "custom";
   ```

   - Overrides inherited methods → may break expectations.

4. **Object.create(null)**

   - Creates object with no prototype (good for dictionaries).
   - But lacks `toString`, `hasOwnProperty`.

5. **Prototype pollution**

   - Mutating `Object.prototype` leaks into all objects → major security risk.

6. **Property descriptors**

   - Default: `writable:true, enumerable:true, configurable:true`.
   - Forgetting descriptors when using `Object.defineProperty` can lock fields.

7. **Constructor vs prototype confusion**

   - `obj.constructor` is just a property; can be reassigned.
   - Don’t rely on it for type checks.

8. **Inheritance performance**

   - Deep prototype chains slow down property lookups.

9. **Class syntax sugar**

   - ES6 `class` just wraps prototypes; not true classical OOP.
   - Still prototype-based underneath.

10. **Delete operator**

    - Removes own props only; inherited props still visible.

    ```js
    delete dog.eats; // no effect, still gets from prototype
    ```

11. **JSON.stringify**

    - Ignores functions and non-enumerable properties.

12. **Symbols**

    - Not accessible in `for...in` or `Object.keys`. Must use `Object.getOwnPropertySymbols`.

13. **Object.freeze/seal**

    - Shallow only. Nested objects still mutable.

---

### Best Practices

- ✅ Use `Object.create(null)` for pure maps.
- ✅ Prefer `Object.hasOwn(obj, key)` over `hasOwnProperty`.
- ✅ Avoid modifying `Object.prototype`.
- ✅ Use `class` syntax for clarity, but remember it’s prototype sugar.
- ✅ Keep prototype chains shallow for perf.
- ✅ Use Symbols for truly private keys.

---

👉 Mnemonic: **“Objects store, Prototypes share.”**

---

Would you like me to expand this into **two separate cheat cards** — one focusing purely on **Objects (creation, descriptors, built-ins)** and another on **Prototypes (inheritance, chain, classes)** — so each remains ultra-crisp?

---

Here’s your **Concept Mastery Cheat Sheet** for **Object Literals & Property Descriptors** — compact, complete, with **all gotchas grouped together**.

---

# 📝 JavaScript Object Literals & Property Descriptors — Cheat Card

**Concept:**

- **Object Literal** → Quick syntax to define objects.
- **Property Descriptor** → Metadata for object properties (controls behavior).

---

### Object Literals

```js
// Basic
const obj = { a: 1, b: "hi" };

// Shorthand props
const x = 10,
  y = 20;
const obj2 = { x, y }; // {x:10,y:20}

// Shorthand methods
const obj3 = {
  greet() {
    return "Hello";
  },
};

// Computed property names
const key = "age";
const obj4 = { [key]: 30 }; // {age:30}

// Symbols as keys
const id = Symbol("id");
const obj5 = { [id]: 123 };

// Spread properties (ES2018+)
const obj6 = { ...obj2, z: 99 };
```

---

### Property Descriptors

- Every property has metadata:

  - **value** → stored value
  - **writable** → can change value?
  - **enumerable** → shows in loops/`Object.keys`?
  - **configurable** → can delete/modify attributes?
  - **get/set** → accessor functions

```js
const user = {};
Object.defineProperty(user, "name", {
  value: "Kalidas",
  writable: false,
  enumerable: true,
  configurable: false,
});
```

Inspecting descriptors:

```js
Object.getOwnPropertyDescriptor(user, "name");
```

---

### Accessor Properties

```js
const person = {
  first: "A",
  last: "B",
  get full() {
    return this.first + " " + this.last;
  },
  set full(val) {
    [this.first, this.last] = val.split(" ");
  },
};
person.full = "X Y"; // updates first/last
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Default descriptor values**

   - Literal/defineProperty defaults differ:

     - Object literal: props → `writable:true, enumerable:true, configurable:true`.
     - `defineProperty`: omitted flags default to `false`.

2. **Non-writable still mutable (objects)**

   ```js
   const obj = {};
   Object.defineProperty(obj, "x", { value: {}, writable: false });
   obj.x.y = 1; // works! shallow only
   ```

3. **Configurable = false**

   - Property cannot be deleted or redefined. Permanent.
   - Can still modify writable value if writable=true.

4. **Enumerable quirks**

   - `for...in` → inherited + enumerable props.
   - `Object.keys` → only own + enumerable.
   - Non-enumerable props still accessible directly.

5. **Getters/Setters pitfalls**

   - Cannot combine `value`/`writable` with `get/set`.
   - Accessors share scope; side effects can confuse.

6. **Spread operator**

   - Only copies **own enumerable string-keyed props**.
   - Ignores non-enumerable, symbol-keyed, accessors.

7. \***\*proto** in literals\*\*

   - Special key:

     ```js
     const o = { __proto__: null }; // prototype null
     ```

   - But not safe across all engines.

8. **Freeze/Seal with descriptors**

   - `Object.freeze(obj)` → sets all props `{writable:false, configurable:false}` (shallow).
   - `Object.seal(obj)` → `{configurable:false}`, keeps writable.

9. **Performance**

   - Heavy use of descriptors (defineProperty) slower than literals.

---

### Best Practices

- ✅ Use object literals for most cases.
- ✅ Use `defineProperty` only when controlling flags.
- ✅ Prefer `Object.freeze` / `seal` for immutability contracts.
- ✅ Be explicit with getters/setters (document side effects).
- ✅ Use spread for shallow copies, structuredClone for deep.
- ❌ Don’t overuse `__proto__`.

---

👉 Mnemonic: **“Literal = quick; Descriptor = control (writable, enumerable, configurable, accessors).”**

---

Would you like me to now create a **dedicated cheat card on `Object.defineProperty` vs `Object.defineProperties` vs `Object.assign` vs spread** (since interviews often ask how they differ in copying/controlling props)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Object Property Access: Dot vs Bracket Notation** — crisp, complete, and with **all gotchas grouped together**.

---

# 🎯 JavaScript Property Access — Dot vs Bracket

**Concept:** Two syntaxes to access object properties.

---

### Dot Notation

```js
const obj = { name: "Kalidas", age: 28 };
console.log(obj.name); // "Kalidas"
```

- Property name written literally.
- Cleaner & faster to read.
- Only works with **valid identifiers** (letters, digits, `_`, `$`, no spaces).

---

### Bracket Notation

```js
const key = "age";
console.log(obj[key]); // 28
console.log(obj["name"]); // "Kalidas"
```

- Property name given as **string or symbol**.
- Useful for **dynamic keys**.
- Allows invalid identifiers (`obj["first-name"]`).

---

### Symbols

```js
const id = Symbol("id");
const user = { [id]: 123 };
console.log(user[id]); // 123
```

- Accessible **only with bracket notation**.

---

### Gotchas ⚠️ (Exhaustive)

1. **Invalid identifiers**

   - Dot fails on keys with spaces, hyphens, digits at start:

     ```js
     obj.first - name; // NaN (parsed as obj.first - name)
     obj["first-name"]; // works
     ```

2. **Dynamic access**

   - Dot cannot use variable key:

     ```js
     const prop = "age";
     obj.prop; // undefined
     obj[prop]; // ✅ 28
     ```

3. **Reserved words**

   - Some keywords allowed as keys but need brackets in old engines:

     ```js
     obj["default"]; // safe
     ```

4. **Symbols**

   - Cannot use dot; only brackets.

5. **Numeric keys**

   - `"0"` and `0` treated same:

     ```js
     const arr = [10, 20];
     arr["0"] === arr[0]; // true
     ```

6. **Prototype chain**

   - Both dot & bracket traverse prototype chain the same way.

7. **Performance**

   - Negligible difference in modern engines.

8. **Minification issues**

   - Dot notation survives minifiers; bracket with string keys may bloat.

9. **Optional chaining**

   - Works with both:

     ```js
     obj?.name;
     obj?.["name"];
     ```

10. **Property existence**

    - Accessing missing key via either → `undefined`, not error.
    - Exception: accessing on `null`/`undefined` → TypeError (use `?.`).

---

### Best Practices

- ✅ Prefer **dot notation** when key is a valid identifier.
- ✅ Use **bracket notation** for dynamic keys, symbols, invalid identifiers.
- ✅ Use optional chaining (`?.`) for safety.
- ❌ Don’t rely on string-concatenated property names; prefer computed keys.

---

👉 Mnemonic: **“Dot for static, Bracket for dynamic.”**

---

Here’s your **Concept Mastery Cheat Sheet** for **Computed Property Names** in JavaScript — concise, complete, and with **all gotchas grouped together**.

---

# 🔢 JavaScript Computed Property Names — Cheat Card

**Concept:** Use an **expression inside `[]`** in object literals to dynamically compute keys at creation. (ES6 feature)

---

### Syntax

```js
const key = "age";
const obj = {
  name: "Kalidas",
  [key]: 28, // dynamic key
  ["first" + "Name"]: "K M",
  [1 + 2]: "three", // numeric expr → "3"
};
console.log(obj.age); // 28
console.log(obj[3]); // "three"
```

---

### Use-Cases

- Dynamic keys from variables.
- Generate keys programmatically (`"prop_"+i`).
- Symbol properties.
- Creating enums or lookup tables.

---

### Examples

```js
const lang = "en";
const messages = {
  [lang + "_hello"]: "Hello!",
  [lang + "_bye"]: "Goodbye!",
};

const id = Symbol("id");
const user = { [id]: 123 };
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Expressions evaluated at creation**

   - Expression runs once when object is created.
   - If variable changes later, key won’t update.

2. **Always coerced to string or symbol**

   ```js
   {[true]: "yes"}; // key "true"
   {[1+2]: "val"};  // key "3"
   ```

3. **Numeric keys**

   - Stored as strings internally.
   - Ordering: integer-like keys iterate first in ascending order.

4. **Symbols**

   - Work with computed syntax:

     ```js
     const sym = Symbol();
     const obj = { [sym]: 42 };
     ```

5. **Property name collisions**

   - If computed key matches existing, it overrides silently.

6. **Debugging difficulty**

   - Dynamic keys harder to track in code inspection or autocomplete.

7. **Performance**

   - Minor overhead vs static keys (evaluates expression).

8. **Not hoisted in class fields**

   ```js
   class C {
     [foo]() {} // must have foo defined before
   }
   ```

9. **Spread operator ignores symbols by default**

   - `{...obj}` does not copy symbol properties from computed keys.

---

### Best Practices

- ✅ Use for truly **dynamic keys** (variables, symbols).
- ✅ Keep expressions simple (avoid heavy logic in `[]`).
- ✅ Document computed keys in objects meant for external APIs.
- ❌ Don’t abuse for static strings (use dot notation instead).
- ✅ Use `Object.hasOwn(obj, key)` to check for existence.

---

👉 Mnemonic: **“Computed = \[expr] → string/symbol key at creation.”**

---

Here’s your **Concept Mastery Cheat Sheet** for **Property Attributes** (`writable`, `configurable`, `enumerable`) in JavaScript — crisp, complete, and with **all gotchas grouped together**.

---

# ⚙️ JavaScript Property Attributes — Cheat Card

**Concept:** Every object property has metadata (a **descriptor**) that controls its behavior.

---

### Attributes (Data Properties)

- **value** → actual value.
- **writable** → can value be changed?
- **enumerable** → does it show up in loops/`Object.keys`?
- **configurable** → can it be deleted or redefined?

```js
const obj = {};
Object.defineProperty(obj, "x", {
  value: 10,
  writable: false,
  enumerable: false,
  configurable: false,
});
```

---

### Accessor Properties

- Instead of `value` & `writable`, use:

  - **get()**
  - **set()**

- Still have **enumerable** & **configurable**.

```js
const user = {
  first: "Kalidas",
  last: "M",
  get full() {
    return this.first + " " + this.last;
  },
  set full(v) {
    [this.first, this.last] = v.split(" ");
  },
};
```

---

### Checking Descriptors

```js
Object.getOwnPropertyDescriptor(obj, "x");
Object.getOwnPropertyDescriptors(obj);
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Defaults differ**

   - Object literals → props are `{writable:true, enumerable:true, configurable:true}`.
   - `Object.defineProperty` defaults all flags to **false** unless specified.

2. **Non-writable shallow only**

   ```js
   const obj = {};
   Object.defineProperty(obj, "a", { value: {}, writable: false });
   obj.a.b = 1; // works (object itself not frozen)
   ```

   (Use `Object.freeze` for deep immutability).

3. **Configurable=false is permanent**

   - Cannot be deleted or reconfigured.
   - Once set, irreversible.

4. **Enumerable quirks**

   - `for...in` iterates inherited enumerable props too.
   - `Object.keys` → own enumerable only.
   - `Object.getOwnPropertyNames` → includes non-enumerable.

5. **Cannot mix data & accessor attributes**

   - Either `{value, writable}` OR `{get, set}`, never both.

6. **delete operator**

   - Works only if `configurable:true`.
   - On globals: `delete varName` is ignored if declared with `var/let/const`.

7. **`Object.freeze` & `seal`**

   - `seal`: sets `configurable:false` (props still writable).
   - `freeze`: sets both `configurable:false, writable:false`.
   - Shallow only (nested objects still mutable).

8. **Proto chain unaffected**

   - Attributes only apply to **own properties**, not inherited ones.

9. **Performance**

   - Using descriptors can make properties slower in some engines (not optimized like literals).

---

### Best Practices

- ✅ Use `Object.defineProperty` for controlled APIs.
- ✅ Use `Object.freeze` for immutability contracts.
- ✅ Keep props enumerable unless deliberately private.
- ✅ Avoid changing descriptors dynamically in hot code paths.
- ✅ Prefer `Object.create(null)` for pure dicts (no prototype).

---

👉 Mnemonic: **“Writable = value change, Enumerable = visible, Configurable = delete/change.”**

---

Would you like me to now do a **dedicated cheat card on Object.freeze / Object.seal / Object.preventExtensions** (since they’re built directly on top of these attributes)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Sealing, Freezing, and Preventing Extensions** in JavaScript — tight, complete, and with **all gotchas grouped together**.

---

# 🛡️ JavaScript: Seal vs Freeze vs Prevent Extensions — Cheat Card

**Concept:** Built-in methods that restrict how objects can be modified.

---

### Prevent Extensions

```js
const obj = { a: 1 };
Object.preventExtensions(obj);
obj.b = 2; // ❌ ignored or throws in strict
```

- Cannot add new properties.
- Can delete/modify existing ones.
- `Object.isExtensible(obj)` → false.

---

### Seal

```js
const obj = { a: 1 };
Object.seal(obj);
delete obj.a; // ❌ fails
obj.a = 2; // ✅ allowed
```

- Prevents adding/removing properties.
- Marks all properties **configurable\:false**.
- Still allows **writable** changes.
- `Object.isSealed(obj)` → true.

---

### Freeze

```js
const obj = { a: 1 };
Object.freeze(obj);
obj.a = 2; // ❌ fails
```

- Prevents adding/removing props.
- Makes all properties **writable\:false, configurable\:false**.
- Shallow only (nested objects still mutable).
- `Object.isFrozen(obj)` → true.

---

### Gotchas ⚠️ (Exhaustive)

1. **Shallow only**

   - Nested objects remain mutable:

     ```js
     const o = { x: { y: 1 } };
     Object.freeze(o);
     o.x.y = 2; // ✅ still changes
     ```

   - Use recursive freeze helper for deep immutability.

2. **Strict mode**

   - Silent fails in sloppy mode.
   - Throws `TypeError` in strict mode.

3. **Performance**

   - Freezing large objects may be slow. Don’t overuse in hot paths.

4. **Array quirks**

   - Preventing extensions: array length becomes non-writable.
   - Freeze: can’t modify contents, push/pop fail.

5. **Descriptors locked**

   - Seal → `configurable:false`.
   - Freeze → `configurable:false, writable:false`.
   - Cannot revert.

6. **`Object.defineProperty` restrictions**

   - On sealed object: cannot add/remove, can update writable values.
   - On frozen object: throws if trying to redefine.

7. **Symbols**

   - Symbols also subject to same restrictions.

8. **Prototype unaffected**

   - Sealing/freezing only locks own properties, not prototype chain.

9. **PreventExtensions**

   - Existing props untouched; can still be reconfigured unless sealed/frozen.

---

### Best Practices

- ✅ Use **freeze** for constants and configuration objects.
- ✅ Use **seal** when structure should be fixed but values editable.
- ✅ Use **preventExtensions** for controlled object growth.
- ✅ For true immutability, implement **deep freeze utility**.
- ❌ Don’t overuse on large/critical perf objects.

---

👉 Mnemonic:

- **PreventExtensions** → 🚫 add new
- **Seal** → 🚫 add/delete, ✅ edit values
- **Freeze** → 🚫 add/delete/edit

---

Would you like me to now create that **Deep Freeze Utility cheat card** (showing recursive freezing with `Object.freeze`), since interviewers often ask “but freeze is shallow, how do you make it deep?”

---

Here’s your **Concept Mastery Cheat Sheet** for the **Prototype Chain** in JavaScript — compact, thorough, with **all gotchas grouped together**.

---

# 🔗 JavaScript Prototype Chain — Cheat Card

**Concept:**
Inheritance in JS is prototype-based. Objects delegate property lookups to their `[[Prototype]]` (aka `__proto__`), forming a **chain**.

---

### Basics

```js
const animal = { eats: true };
const dog = Object.create(animal);
dog.bark = () => "woof";

dog.eats; // true (from prototype)
dog.bark(); // "woof" (own)
```

- Every object (except `Object.create(null)`) has a prototype.
- Prototype chain = object → its prototype → … → `Object.prototype` → `null`.
- Functions have `.prototype` property (used when called with `new`).

---

### Core Methods

- `Object.getPrototypeOf(obj)` → current prototype.
- `Object.setPrototypeOf(obj, proto)` → change prototype (⚠️ slow).
- `obj.__proto__` → legacy accessor, avoid.
- `Object.create(proto, props)` → create with prototype + descriptors.

---

### Inheritance Example

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  return "Hi " + this.name;
};

const p = new Person("Kalidas");
p.sayHi(); // "Hi Kalidas"
```

---

### Prototype Chain Resolution

1. Access property on object.
2. If missing, JS looks up prototype chain.
3. Stops at `null`.
4. Methods like `toString` come from `Object.prototype`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Shadowing**

   - Own property overrides prototype property.

   ```js
   dog.eats = false; // shadows animal.eats
   ```

2. **Deletion**

   - `delete obj.prop` removes only **own prop**. Inherited stays visible.

3. **Performance**

   - Long chains slow property lookup.

4. **Constructor confusion**

   - `obj.constructor` is just a property, not reliable type check.
   - Safer: `obj instanceof Constructor`.

5. **instanceof quirks**

   - Checks prototype chain, not type string.
   - Cross-frame objects may fail (`[] instanceof Array` false if from different realm).

6. **Object.create(null)**

   - No prototype; lacks methods like `toString`, `hasOwnProperty`.
   - Great for dicts/maps.

7. **Prototype mutation risk**

   - Changing prototypes after object creation is slow + unsafe.
   - Adding/removing properties from prototypes affects all inheritors.

8. **Prototype pollution**

   - Malicious code adding props to `Object.prototype` → leaks into all objects.

9. **Class syntax sugar**

   - ES6 `class` is syntactic sugar over prototype chain.
   - Methods go to `.prototype`. Static methods go to constructor function.

10. **JSON.stringify**

    - Only serializes own enumerable string keys, not inherited props.

11. **Symbols**

    - Inherited symbol keys not shown in `Object.keys`, but still accessible.

---

### Best Practices

- ✅ Use `Object.create(null)` for safe dictionaries.
- ✅ Use `class` syntax for clarity; remember it’s prototype-based.
- ✅ Don’t mutate `Object.prototype`.
- ✅ Keep prototype chains shallow for perf.
- ✅ Use `Object.hasOwn(obj,key)` to check for own props.
- ✅ Avoid relying on `__proto__`.

---

👉 Mnemonic: **“Prototype Chain = Lookup ladder: own → parent → Object → null.”**

---

Would you like me to also prepare a **dedicated cheat card on ES6 `class` syntax vs Prototype-based inheritance** (since interviews often ask “is class really different in JS?”)?

---

Here’s your **Concept Mastery Cheat Sheet** for
**`Object.create`, `Object.getPrototypeOf`, `Object.setPrototypeOf`** — complete, crisp, with **all gotchas grouped together**.

---

# 🏗️ Prototype Helpers — Cheat Card

**Concept:** Tools for creating and inspecting/modifying prototype chains in JavaScript.

---

### `Object.create(proto, [props])`

```js
const animal = { eats: true };
const dog = Object.create(animal, {
  bark: { value: () => "woof", writable: true },
});
dog.eats; // true (inherited)
```

- Creates new object with given prototype.
- Optional 2nd arg = property descriptors.
- `Object.create(null)` → object with **no prototype** (dict).

---

### `Object.getPrototypeOf(obj)`

```js
const proto = Object.getPrototypeOf(dog);
console.log(proto === animal); // true
```

- Returns the prototype (`[[Prototype]]`) of object.
- Same as `obj.__proto__` (legacy).

---

### `Object.setPrototypeOf(obj, proto)`

```js
Object.setPrototypeOf(dog, { runs: true });
console.log(dog.runs); // true
```

- Replaces object’s prototype.
- ⚠️ **Slow** (affects optimization). Avoid in hot code.

---

### Gotchas ⚠️ (Exhaustive)

1. **Performance hit**

   - `Object.setPrototypeOf` de-optimizes objects.
   - Better: use `Object.create` during construction.

2. **Null prototype objects**

   - `Object.create(null)` has no `toString`, `hasOwnProperty`.
   - Great for dictionaries, but must handle carefully.

3. **Circular prototypes**

   - Setting an object’s prototype to itself or a cycle throws `TypeError`.

   ```js
   const a = {};
   Object.setPrototypeOf(a, a); // ❌
   ```

4. **Descriptors default to false**

   - In `Object.create(proto, props)`, unspecified attributes default:
     `{ writable:false, enumerable:false, configurable:false }`.

5. **Object.getPrototypeOf restrictions**

   - Passing non-objects (primitives) throws `TypeError`.
   - Except `Object.getPrototypeOf(null/undefined)` → ❌ always error.

6. **Inheritance pitfalls**

   - Changing prototype after object creation doesn’t retroactively copy props.
   - Only future lookups are affected.

7. **ES6 `class` interop**

   - Classes use `Object.setPrototypeOf` internally for inheritance (`extends`).

8. **Cross-realm issues**

   - Prototypes from different iframes/windows may not match expected constructors.

9. **Security**

   - Prototype pollution attacks target `Object.prototype`. Using `Object.create(null)` avoids this.

10. **Engine differences**

    - Early JS engines had partial/non-performant support for `setPrototypeOf`.

---

### Best Practices

- ✅ Prefer `Object.create(proto)` when designing inheritance.
- ✅ Use `Object.create(null)` for safe, dictionary-like objects.
- ✅ Use `Object.getPrototypeOf` for safe prototype inspection (instead of `__proto__`).
- ❌ Avoid `Object.setPrototypeOf` in performance-sensitive code.
- ✅ Use `class` syntax if you need clean inheritance semantics.

---

👉 Mnemonic:

- **Create** → make with prototype.
- **Get** → inspect prototype.
- **Set** → change prototype (⚠️ slow).

---

Would you like me to next make a **cheat card comparing `Object.assign`, spread `{...}`, and `Object.defineProperties`** — since they often get confused with `Object.create` in interviews?

---

Here’s your **Concept Mastery Cheat Sheet** for **ES6 `class` syntax** — clean, practical, and with **all gotchas grouped together**.

---

# 🏫 JavaScript ES6 `class` — Cheat Card

**Concept:**
Syntactic sugar over prototypes for defining constructors + methods in a cleaner way.

---

### Basic Syntax

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `Hi, ${this.name}`;
  } // instance method
  static species() {
    return "Homo sapiens";
  } // static method
}

const p = new Person("Kalidas");
p.sayHi(); // "Hi, Kalidas"
Person.species(); // "Homo sapiens"
```

---

### Features

- **Constructor** → initializes instance (only one per class).
- **Instance methods** → defined on `Class.prototype`.
- **Static methods** → called on class itself.
- **Getters/Setters** → via `get` / `set`.
- **Fields (ES2022)** → define properties directly.

  ```js
  class Example {
    x = 10;
    #secret = 42;
  }
  ```

- **Private fields** → `#field` (true privacy).
- **Inheritance** → `extends` for subclassing.
- **`super`** → call parent constructor/method.

---

### Inheritance Example

```js
class Animal {
  speak() {
    return "generic";
  }
}
class Dog extends Animal {
  speak() {
    return super.speak() + " woof";
  }
}
new Dog().speak(); // "generic woof"
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Syntactic sugar only**

   - Classes still use prototypes under the hood.

2. **Not hoisted**

   - Unlike functions, `class` declarations are **not hoisted**.

   ```js
   new C(); // ❌ ReferenceError
   class C {}
   ```

3. **Strict mode always on**

   - All class bodies run in `"use strict"`.

4. **Constructor rules**

   - Subclass must call `super()` **before using `this`**.
   - Missing `super()` in subclass → ReferenceError.

5. **Static vs instance confusion**

   - Instance methods go on `prototype`.
   - Static methods only available on class itself.

   ```js
   p.species(); // ❌
   Person.species(); // ✅
   ```

6. **Private fields `#`**

   - Not accessible outside class.
   - Not dynamically accessible (`obj["#field"]` ❌).

7. **Class fields**

   - Declared outside constructor, run per-instance.
   - Be cautious with reference types (arrays, objects) — each instance gets its own.

8. **Inheritance quirks**

   - Extending built-ins (`Array`, `Error`) may behave inconsistently across engines.
   - Need `super()` call to properly initialize.

9. **Binding `this`**

   - Methods aren’t auto-bound. Passing to callbacks loses context.
   - Fix: arrow functions in class fields or manual `bind`.

10. **Performance**

- Defining methods inside constructor instead of `prototype` creates per-instance copies → memory bloat.

11. **`new.target`**

- Inside constructor, `new.target` refers to constructor called.
- Useful for abstract base classes.

12. **Symbol properties**

- Class methods can use computed names (`[Symbol.iterator]() {}`), but not private + symbol together.

---

### Best Practices

- ✅ Use `class` for clear OOP-like structures.
- ✅ Prefer **fields + private `#`** for encapsulation.
- ✅ Keep methods on prototype (default) for memory efficiency.
- ❌ Don’t treat JS classes as classical OOP — still prototype-based.
- ✅ Always call `super()` in subclass constructors.
- ✅ Use arrow functions in class fields for auto-bound callbacks if needed.

---

👉 Mnemonic: **“Class = Constructor + Methods + Static + Inherit + Sugar.”**

---

Would you like me to follow this with a **cheat card comparing `class` vs prototype-based inheritance** (to show how sugar translates under the hood)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Constructors, Fields, and Methods** in ES6+ `class` syntax — crisp, complete, and with **all gotchas grouped together**.

---

# 🏗️ JavaScript Classes: Constructors, Fields, Methods — Cheat Card

**Concept:**
Classes define objects with **initialization (constructors)**, **data (fields)**, and **behavior (methods)**.

---

### Constructors

```js
class Person {
  constructor(name, age = 18) {
    this.name = name;
    this.age = age;
  }
}
const p = new Person("Kalidas", 27);
```

- Special method for initialization.
- Only **one constructor** per class.
- Subclass must call `super()` before `this`.
- Supports default parameters.

---

### Fields (ES2022+)

```js
class Example {
  x = 10; // public field
  #secret = 42; // private field
}
const e = new Example();
console.log(e.x); // 10
console.log(e.#secret); // ❌ SyntaxError
```

- Declared directly in class body.
- Public fields accessible via instance.
- Private fields `#name` → true privacy.
- Initialized **per instance**.

---

### Methods

```js
class Calculator {
  add(a, b) {
    return a + b;
  } // instance method
  static multiply(a, b) {
    return a * b;
  } // static method
  get double() {
    return this.val * 2;
  } // getter
  set double(v) {
    this.val = v / 2;
  } // setter
}
```

- **Instance methods** → live on `Class.prototype`.
- **Static methods** → belong to the class itself, not instances.
- **Getters/Setters** → computed accessors.
- Can use **computed names** (`[Symbol.iterator](){...}`).

---

### Gotchas ⚠️ (Exhaustive)

1. **Constructor rules**

   - If subclass defines constructor → must call `super()`.
   - Omitting `super()` before using `this` → ReferenceError.

2. **Multiple constructors ❌**

   - Only one allowed. Overloads not supported (simulate with defaults or static factories).

3. **Class fields quirks**

   - Each instance gets a fresh copy of fields.
   - Reference types shared by all instances only if defined on prototype.

4. **Private fields `#`**

   - Not accessible via bracket notation.
   - `obj["#secret"]` ❌ invalid.
   - Breaks JSON serialization (ignored).

5. **Static vs instance confusion**

   - Instance methods can’t call static ones via `this`, use `ClassName.method()`.

6. **Inheritance**

   - `super` works for methods and constructors.
   - Missing `super()` in subclass constructor → crash.

7. **Method binding**

   - Class methods are **not auto-bound**. Passing to callbacks loses `this`:

     ```js
     class A {
       log() {
         console.log(this);
       }
     }
     setTimeout(new A().log, 1000); // ❌ undefined/global
     ```

   - Fix: arrow fields → `log=()=>console.log(this);`

8. **Hoisting**

   - Classes are not hoisted. Cannot use before declaration.

9. **Strict mode**

   - Class bodies are always strict (`"use strict"`).

10. **Performance**

- Defining methods in constructor (vs prototype) creates new function per instance → memory heavy.

---

### Best Practices

- ✅ Use constructor for setup only; keep logic in methods.
- ✅ Use **fields** for default values and privacy (`#private`).
- ✅ Put reusable logic in prototype methods, not per-instance.
- ✅ Use arrow fields for callbacks to preserve `this`.
- ✅ Use static methods for utilities related to class but not tied to instance.
- ❌ Don’t abuse private `#` if simpler closure-based privacy suffices.

---

👉 Mnemonic: **“Constructor = setup, Fields = data, Methods = behavior.”**

---

Would you like me to now expand this into a **cheat card comparing Instance Methods vs Static Methods vs Prototype Methods** (since that’s a common interview confusion)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Static Methods, Getters & Setters** in JavaScript classes — sharp, compact, and with **all gotchas grouped together**.

---

# ⚙️ JavaScript Static Methods, Getters & Setters — Cheat Card

**Concept:**

- **Static methods** → belong to the class itself, not instances.
- **Getters/Setters** → define computed accessors for properties.

---

### Static Methods

```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}
MathUtils.add(2, 3); // 5
```

- Called on class, not instance.
- Useful for utilities, factories, helpers.
- Inherited by subclasses.

---

### Getters & Setters

```js
class Person {
  constructor(name) {
    this._name = name;
  }
  get name() {
    return this._name.toUpperCase();
  }
  set name(v) {
    this._name = v.trim();
  }
}
const p = new Person("Kalidas");
p.name; // "KALIDAS" (getter)
p.name = " KM "; // setter updates _name
```

- **Getter**: accessed like a property, no `()`.
- **Setter**: assign via `=`.
- Can be combined with validation, formatting.

---

### Combined Example

```js
class Circle {
  constructor(r) {
    this._r = r;
  }
  get area() {
    return Math.PI * this._r ** 2;
  }
  set radius(r) {
    if (r > 0) this._r = r;
  }
  static unit() {
    return new Circle(1);
  }
}
Circle.unit().area; // 3.14...
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Static vs instance confusion**

   - `MathUtils.add()` ✅
   - `new MathUtils().add()` ❌ TypeError
   - Instances don’t get static methods.

2. **Inheritance quirks**

   - Static methods are inherited:

     ```js
     class A {
       static hi() {
         return "hi";
       }
     }
     class B extends A {}
     B.hi(); // "hi"
     ```

   - But can be shadowed in subclass.

3. **`this` in static methods**

   - Refers to the class itself, not instance.

   ```js
   class A {
     static who() {
       return this.name;
     }
   }
   A.who(); // "A"
   ```

4. **Getters with side effects**

   - Avoid heavy logic in getters (unexpected slowdowns).
   - Avoid getters that mutate state.

5. **Setters pitfalls**

   - Must accept exactly **1 parameter**.
   - Recursive setter if not careful:

     ```js
     set name(v){ this.name=v; } // ❌ infinite loop
     ```

   - Use backing field (`_name`).

6. **Property descriptors**

   - `get/set` cannot coexist with `value`/`writable`.
   - Configurable/enumerable apply as usual.

7. **JSON.stringify ignores getters**

   - Only serializes data properties, not computed ones.

8. **Static private fields** (ES2022)

   - Allowed:

     ```js
     class A {
       static #count = 0;
       static inc() {
         return ++A.#count;
       }
     }
     ```

   - Accessible only within class.

9. **Performance**

   - Overusing getters for hot properties may slow down compared to direct access.

---

### Best Practices

- ✅ Use **static methods** for class-level utilities.
- ✅ Use **getters** for computed properties, formatting, lazy evaluation.
- ✅ Use **setters** for validation + sanitization.
- ✅ Backing fields (`_name`, `#private`) to avoid recursion.
- ❌ Don’t put heavy logic or side effects in getters.
- ✅ Keep static & instance methods conceptually separate.

---

👉 Mnemonic: **“Static = class tools, Getter = computed read, Setter = controlled write.”**

---

Do you want me to also prepare a **cheat card on Private Fields & Static Private Methods (`#field`, `static #method`)** since they extend this concept and are common in ES2022+ interviews?

---

Here’s your **Concept Mastery Cheat Sheet** for **Private Fields (`#field`)** in JavaScript — compact, thorough, and with **all gotchas grouped together**.

---

# 🔒 JavaScript Private Fields (`#field`) — Cheat Card

**Concept:**
ES2022+ feature: Class fields prefixed with `#` are truly private — accessible only inside the class body.

---

### Syntax

```js
class Person {
  #ssn; // private field
  constructor(ssn) {
    this.#ssn = ssn;
  }
  getSSN() {
    return this.#ssn;
  } // access inside
}
const p = new Person("123-45-6789");
p.#ssn; // ❌ SyntaxError
p.getSSN(); // "123-45-6789"
```

---

### Features

- Defined with `#name` inside class body.
- Accessible only from inside that class (including methods).
- Separate from normal props (`obj.#x` ≠ `obj.x`).
- Supported for:

  - Instance fields: `#count = 0;`
  - Static fields: `static #registry = new Map();`
  - Methods: `#helper(){...}`
  - Getters/Setters: `get #id(){...}`

---

### Gotchas ⚠️ (Exhaustive)

1. **Hard privacy**

   - Cannot be accessed/reflected externally.
   - Not enumerable, not in `Object.keys`, `for...in`, `JSON.stringify`.

2. **Different identity**

   - `this.#x` and `this["#x"]` are unrelated.
   - No way to dynamically compute private field names.

3. **Inheritance rules**

   - Subclasses cannot access parent’s private fields.
   - Each class has its own set.

4. **Mixing public & private**

   ```js
   class C {
     #val = 1;
     val = 2;
   }
   new C(); // has both `#val` and `val`
   ```

   - Different properties, no conflict.

5. **Performance**

   - Private fields slightly slower in some engines (hidden class ops).

6. **Weak encapsulation workaround**

   - Old-style privacy used `WeakMap` or closures; `#` fields replace these.

7. **Reflection API**

   - Cannot get descriptors:

     ```js
     Object.getOwnPropertyNames(obj); // no private fields
     ```

8. **Copying objects**

   - Spread/rest, `Object.assign`, `structuredClone` → skip private fields.
   - Only class methods can re-expose them.

9. **Debugging**

   - Harder to inspect in dev tools unless supported.

10. **Static private fields**

    - Scoped to class, not instances.
    - Not inherited by subclasses.

---

### Best Practices

- ✅ Use `#private` for true encapsulation.
- ✅ Use getters/setters to expose controlled access.
- ✅ Document private fields clearly to avoid confusion.
- ✅ Use when API must guarantee no external tampering.
- ❌ Don’t overuse; sometimes `_underscore` convention suffices for internal props.
- ❌ Don’t rely on JSON/serialization to persist private fields.

---

👉 Mnemonic: **“#field = hard private, not enumerable, not inheritable, not reflective.”**

---

Would you like me to now create a **cheat card comparing `#private fields` vs `_underscore convention` vs `WeakMap-based privacy`**, since interviewers often ask why `#` is better than old patterns?

---

Here’s your **Concept Mastery Cheat Sheet** for **Inheritance & `super`** in JavaScript — concise, interview-ready, and with **all gotchas grouped together**.

---

# 🧬 JavaScript Inheritance & `super` — Cheat Card

**Concept:**
Inheritance in JS uses the **prototype chain**. ES6 `class` syntax adds `extends` and `super` for clarity.

---

### Basic Inheritance

```js
class Animal {
  speak() {
    return "generic sound";
  }
}
class Dog extends Animal {
  speak() {
    return "woof";
  }
}
new Dog().speak(); // "woof"
```

---

### Using `super`

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    return `${this.name} makes a noise`;
  }
}
class Dog extends Animal {
  constructor(name, breed) {
    super(name); // call parent constructor
    this.breed = breed;
  }
  speak() {
    return super.speak() + " woof";
  }
}
const d = new Dog("Rocky", "Labrador");
d.speak(); // "Rocky makes a noise woof"
```

- `super(...)` → calls parent constructor (must be first in subclass constructor).
- `super.method(...)` → calls parent’s method.
- Works in both **constructor** and **methods**.

---

### Prototype Links

- `Dog.prototype.__proto__ === Animal.prototype`
- `Dog.__proto__ === Animal`

---

### Gotchas ⚠️ (Exhaustive)

1. **Constructor rules**

   - Subclass **must** call `super()` before `this`.
   - Omit → ReferenceError.
   - If no constructor defined, subclass auto-inherits and calls parent’s.

2. **Arrow functions & `super`**

   - Arrows don’t bind `super`. They use the enclosing scope’s `super`.

3. **Static inheritance**

   - Static methods/properties also inherited:

     ```js
     class A {
       static hi() {
         return "hi";
       }
     }
     class B extends A {}
     B.hi(); // "hi"
     ```

4. **Shadowing methods**

   - Subclass method with same name overrides parent.
   - Can still call parent with `super.method()`.

5. **Multiple inheritance ❌**

   - JS doesn’t support; simulate with mixins (functions returning classes).

6. **Prototype chain performance**

   - Very deep chains → slower property lookup.

7. **super in non-methods**

   - `super` only valid in constructors/methods, not in nested functions.
   - Fix: use arrow functions or bind context.

8. **super with getters/setters**

   - Works correctly but can be confusing (calls parent’s accessor).

9. **class fields before super**

   ```js
   class Dog extends Animal {
     breed = "lab"; // runs after super()
     constructor(name) {
       super(name);
     }
   }
   ```

   - Field initializers always run **after super()**.

10. **`Object.setPrototypeOf` hazards**

    - Manually altering inheritance chain can break `super`.

---

### Best Practices

- ✅ Use `extends` + `super` for clear inheritance.
- ✅ Keep chains shallow; prefer composition if possible.
- ✅ Call `super()` early in subclass constructor.
- ✅ Use `super.method()` sparingly; don’t overcomplicate overrides.
- ❌ Don’t mutate prototypes after class creation.
- ✅ For multiple behaviors, use **mixins** instead of deep inheritance.

---

👉 Mnemonic: **“`extends` links, `super` calls.”**

---

Would you like me to also prepare a **cheat card on Mixins (multiple inheritance simulation in JS)** since it often follows the inheritance topic in interviews?

---

Here’s your **Concept Mastery Cheat Sheet** for **`this` Binding Rules** in JavaScript — compact, interview-focused, and with **all gotchas grouped together**.

---

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

👉 Mnemonic: **“`new` > explicit > implicit > default. Arrows inherit.”**

---

Would you like me to follow this up with a **cheat card on `call`, `apply`, and `bind`** (since they’re the tools to control `this` explicitly)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Object vs Map** in JavaScript — compact, side-by-side, and with **all gotchas grouped together**.

---

# 🗂️ JavaScript: Object vs Map — Cheat Card

**Concept:**
Both store key–value pairs, but with different design, performance, and use-cases.

---

### Object

```js
const obj = { a: 1 };
obj["b"] = 2;
```

- Keys: strings & symbols only.
- Prototype by default (`__proto__`).
- Not ordered (though ES6+ preserves insertion for string keys in practice).
- Best for structured data, simple records.

---

### Map

```js
const map = new Map();
map.set("a", 1);
map.set(42, "num");
map.set({}, "obj");
```

- Keys: any type (objects, functions, NaN).
- Ordered by insertion.
- Size with `.size`.
- Optimized for frequent add/remove.
- Best for dynamic collections, lookup tables.

---

### API Comparison

| Feature       | Object                                | Map                                                     |
| ------------- | ------------------------------------- | ------------------------------------------------------- |
| Key types     | String, Symbol                        | Any value                                               |
| Iteration     | `for…in`, `Object.keys`               | `map.keys()`, `map.values()`, `map.entries()`, `for…of` |
| Order         | Not guaranteed (but stable since ES6) | Guaranteed insertion order                              |
| Size          | `Object.keys(obj).length`             | `map.size`                                              |
| Prototype     | Inherits from `Object.prototype`      | Pure container                                          |
| Performance   | Slower for frequent adds/removes      | Optimized for adds/removes                              |
| Serialization | JSON.stringify works                  | Must convert manually                                   |

---

### Gotchas ⚠️ (Exhaustive)

1. **Key coercion in Objects**

   - Keys auto-stringified:

     ```js
     const obj = {};
     obj[{}] = "x";
     obj["[object Object]"]; // same key
     ```

   - Map preserves object identity.

2. **Iteration**

   - Object: `for…in` includes inherited props (unless guarded with `hasOwn`).
   - Map: iterates only own entries, safe.

3. **Order guarantees**

   - Objects: keys are ordered → integer keys first (ascending), then strings, then symbols.
   - Maps: insertion order always preserved.

4. **Performance pitfalls**

   - Large dynamic key sets: Map faster.
   - Small static sets: Object simpler.

5. **Prototype pollution**

   - Adding unexpected props to `Object.prototype` can break lookups.
   - Map immune (no prototype).

6. **Serialization**

   - JSON.stringify ignores Map entries. Need `Object.fromEntries(map)` or spread:

     ```js
     JSON.stringify(Object.fromEntries(map));
     ```

7. **Memory usage**

   - Maps generally use more memory overhead for small collections.

8. **Symbols in Objects**

   - Not enumerable in `Object.keys`, but still stored. Map has no such quirk.

9. **Checking existence**

   - Object: `"key" in obj`.
   - Map: `map.has(key)`.

10. **Delete behavior**

    - `delete obj.key` slower, can degrade optimization.
    - `map.delete(key)` efficient.

---

### Best Practices

- ✅ Use **Object** for structured, JSON-like data.
- ✅ Use **Map** for dynamic key sets, frequent inserts/deletes, or non-string keys.
- ✅ Prefer `Object.create(null)` when using object purely as dictionary.
- ✅ Use `Object.hasOwn(obj,key)` to avoid inherited props.
- ❌ Don’t serialize Map directly — convert first.

---

👉 Mnemonic: **“Object = Record, Map = Dictionary.”**

---

Would you like me to also prepare a **cheat card on Map vs WeakMap vs WeakSet** (since that’s a common follow-up in interviews)?

---

Here’s your **Concept Mastery Cheat Sheet** for **Array vs Set** in JavaScript — crisp, side-by-side, and with **all gotchas grouped together**.

---

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

👉 Mnemonic: **“Array = List (allows duplicates). Set = Unique Bag (fast lookup).”**

---

Would you like me to also prepare a **cheat card on Set vs WeakSet** (since that’s the next logical extension and often tested in interviews)?

---
