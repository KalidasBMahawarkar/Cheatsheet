# JavaScript Objects & Prototypes

---

## Concepts:

### Objects

- collections of key–value pairs.

#### Syntax

```js
// Literal
const a = "hi";
const key = "age";
const id = Symbol("id");
const obj2 = { x: 10, y: 20 };
const obj = {
  a, // Shorthand props
  b: 2,

  [key]: 30, // age : 30, Computed property names assigned at runtime as value,
  //so if later key variable changes it will not affect the object key
  //key = "year" but obj.age will still be 30
  //Always coerced to string or symbol
  {[true]: "yes"}; // key "true"
  {[1 + 2]: "val"}, // key "3"

  [id]: 123, // Symbols as keys
  c: function () {
    return "hello"; // Shorthand methods
  },
  ...obj2, // Spread properties (ES2018+)
};
// Constructor
const obj2 = new Object();
// Create with prototype
const obj3 = Object.create(proto, { x: { value: 42 } });
// Create with no prototype (good for dictionaries)
const obj4 = Object.create(null); // (lacks inherited methods like toString, hasOwnProperty).
```

#### Rules

- Keys: strings or symbols.
- Values: any type (including functions → methods).
- `Object.keys`, `Object.values`, `Object.entries`.
- Property attributes: `writable`, `enumerable`, `configurable`.

#### Copying objects

- `...spread` operator

  - Only copies **own enumerable string-keyed props** and ignores non-enumerable, symbol-keyed, accessors.
  - `{...obj}` does not copy symbol properties from computed keys.
  - Only creates shallow copy nested objects are not copied by value but by reference.
  - Use `spread ({...obj})` → when you want a new immutable copy (most common).

  ```js
  const original = { a: 1, b: { x: 10 } };
  const copy1 = { ...original };
  copy1.a = 99; // ✅ independent change
  copy1.b.x = 42; // ❌ affects original, shallow only
  console.log(original.a); // 1
  console.log(original.b.x); // 42 (changed!)
  ```

- `Object.assign`
  - Copies all enumerable own props.
  - Use `Object.assign` → when you intentionally want to mutate a target object or need older ES6 support.
- `structuredClone`

  - structuredClone makes a true deep copy — all own props, including non-enumerable and accessors, all nested objects/arrays/maps/sets cloned.
  - But Skips Functions, DOM nodes, Class instances with methods.

  ```js
  const copy2 = structuredClone(original);
  copy2.a = 99; // ✅ independent
  copy2.b.x = 42; // ✅ independent
  copy2.c.push(4);
  console.log(original.a); // 1
  console.log(original.b.x); // 10
  console.log(original.c); // [1, 2, 3]
  console.log(copy2.c); // [1, 2, 3, 4]
  ```

#### alert⚠️

- `for...in`
  - iterates **all enumerable** (incl. inherited).
  - Use `obj.hasOwnProperty(key)` or `Object.hasOwn(obj,key)`.
- `delete obj.prop` -> operator Removes own props only; inherited props still visible.
- `JSON.stringify` -> Ignores functions and non-enumerable properties.
- `Symbols` -> Not accessible in `for...in` or `Object.keys`. Must use `Object.getOwnPropertySymbols`
- Numeric keys
  - Stored as strings internally.
  - Ordering: integer-like keys iterate first in ascending order.

### Prototypes

- hidden links (`[[Prototype]]`) that provide inheritance in JS.
- Objects delegate property lookups to their `[[Prototype]]` (aka `__proto__`), forming a **chain**.

#### Syntax

- `Object.getPrototypeOf(obj)` → current prototype.
- `Object.setPrototypeOf(obj, proto)` → change prototype (⚠️ slow).
- `obj.__proto__` → legacy accessor, avoid.
- `Object.create(proto, props)` → create with prototype + descriptors.
- `Object.create(null)` → object with **no prototype** (dict).

#### Examples

```js
// Basic Example
const animal = { eats: true };
const dog = Object.create(animal);
dog.barks = true;
console.log(dog.eats); // true (via prototype)
console.log(Object.getPrototypeOf(dog) === animal); // true
```

```js
// Inheritance Example
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  return "Hi " + this.name;
};
const p = new Person("Kalidas");
p.sayHi(); // "Hi Kalidas"
```

#### Rules

- Every object has an internal `[[Prototype]]` (except `Object.create(null)`).
- Accessed via `Object.getPrototypeOf(obj)` / `Object.setPrototypeOf(obj)` or legacy `__proto__`.
- Functions have `prototype` property → used when creating objects with `new`.

#### Prototype Chain

- Lookup order: `obj → obj.[[Prototype]] → … → Object.prototype → null`.
- If not found, returns `undefined`.
- Inheritance in JS = delegation via this chain.
- Adding/removing properties from prototypes affects all inheritors.
- Methods like `.toString()` come from `Object.prototype`.
- Steps:
  1. Access `obj.prop`.
  2. Engine checks own properties.
  3. If missing → looks at `obj.[[Prototype]]`.
  4. Repeats until `null`.
- `null` marks the end → `Object.create(null)` creates “bare” object with no chain.

#### alert⚠️

- `__proto__`
  - Non-standard historically, now standardized but still discouraged.
  - Use `obj.hasOwnProperty(key)` or `Object.hasOwn(obj,key)`.
  - Setting via `obj.__proto__=x` is slow. Use `Object.create` or `Object.setPrototypeOf` instead.
    ```js
    const proto = {
      greet: () => "hi",
    };
    const o2 = { __proto__: proto, x: 10 };
    o2.greet(); // "hi"
    ```
- Object property can Shadow `prototype` methods:
  ```js
  // Overrides inherited toString(method) → may break expectations.
  obj.toString = () => "custom";
  ```
- Setting an object’s prototype to itself or a cycle throws `TypeError`.
  ```js
  const a = {};
  Object.setPrototypeOf(a, a); // ❌
  ```
- Mutating `Object.prototype` leaks into all objects → major security risk.
- `obj.constructor` is just a property; can be reassigned Don’t rely on it for type checks.
- Deep prototype chains slow down property lookups.
- Class
  - ES6 `class` just wraps prototypes; not true classical OOP.
  - Still prototype-based underneath.
  - Methods go to `.prototype`. Static methods go to constructor function.
- Long chains slow property lookup.

---

### Property descriptors & Accessor Properties

- Metadata for object properties (controls behavior).
- Metadata for each property:
  - Property/data descriptors
    - **value** → stored value
    - **writable** → can change value?
  - Accessor Properties
    - **get** → getter function
    - **set** → setter function
  - **enumerable** → shows in loops/`Object.keys`/{...spread}?
  - **configurable** → can delete/modify attributes?
- Default: `writable:true, enumerable:true, configurable:true`.
- `defineProperty`: omitted flags default to `false`.
- A Object is either a data descriptor (value, writable) or an accessor descriptor (get, set).
- Cannot combine `value`/`writable` with `get/set`.

#### Syntax

```js
Object.defineProperty(obj, key, descriptor);
Object.defineProperties(obj, { k1: d1, k2: d2 });
Object.getOwnPropertyDescriptor(obj, key);
Object.getOwnPropertyDescriptors(obj);
Reflect.defineProperty(obj, key, descriptor); // returns boolean

const user = {
  name: "Kalidas", // by default it is true, writable: true, enumerable: true, configurable: true
};
Object.defineProperty(user, "name", {
  // If you don’t explicitly set flags, the defaults here are false
  value: "Kalidas",
  writable: false,
  enumerable: true,
  configurable: false,
});

//Inspecting descriptors:
Object.getOwnPropertyDescriptor(user, "name");
// returns
   {
      "value": "Kalidas",
      "writable": false,
      "enumerable": true,
      "configurable": false
   }
```

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
person.full; // "A B"
person.full = "X Y"; // updates first/last
person.full; // "X Y"
```

#### Enumerable quirks

```js
const proto = {
  a: 1, // enumerable: true
  b: 2, // enumerable: false
};
const obj = Object.create(proto);
obj.c = 3;
obj.d = 4; // enumerable: false
```

- `for...in` → inherited + enumerable props.
  ```js
  for (let k in obj) console.log(k); // "c", "a"
  ```
- `Object.keys` → only own + enumerable props.
  ```js
  Object.keys(obj); // ["c"]
  ```
- Non-enumerable props still accessible directly.
  ```js
  obj.d; // 4 ✅ (even though non-enum)
  obj.b; // 2 ✅ (inherited + non-enum)
  ```

#### Freeze/Seal/PreventExtensions with descriptors

- `Object.freeze(obj)` → sets all props `{writable:false, configurable:false}` (shallow).
- Use `Object.freeze` for deep immutability

  ```js
  const frozen = { a: 1, b: { x: 10 } };
  Object.freeze(frozen);
  frozen.a = 99; // ❌ ignored (TypeError in strict mode)
  delete frozen.a; // ❌ fails
  frozen.c = 3; // ❌ cannot add new props
  console.log(frozen.a); // 1
  // But nested object is still mutable:
  frozen.b.x = 42; // ✅ works
  console.log(frozen.b.x); // 42
  Object.isFrozen(frozen); // true
  ```

- `Object.seal(obj)` → `{configurable:false}`, keeps writable.

  - Prevents adding/removing properties.
  - `Object.isSealed(obj)` → true.

  ```js
  const sealed = { a: 1, b: 2 };
  Object.seal(sealed);
  sealed.a = 99; // ✅ works (still writable)
  delete sealed.b; // ❌ fails
  sealed.c = 3; // ❌ cannot add new props
  console.log(sealed); // { a: 99, b: 2 }
  Object.isSealed(sealed); // true
  ```

- `preventExtensions`

  - Cannot add new properties.
  - Can delete/modify existing ones.
  - `Object.isExtensible(obj)` → false.

  ```js
  const obj = { a: 1 };
  Object.preventExtensions(obj);
  obj.b = 2; // ❌ ignored or throws in strict
  ```

##### alert⚠️

- **Shallow only all 3 methods**
  - Nested objects remain mutable:
    ```js
    const o = { x: { y: 1 } };
    Object.freeze(o);
    o.x.y = 2; // ✅ still changes
    ```
  - Use recursive freeze helper for deep immutability.
- **Performance**
  - Freezing large objects may be slow. Don’t overuse in hot paths.
- **Array quirks**
  - Preventing extensions: array length becomes non-writable.
  - Freeze: can’t modify contents, push/pop fail.
- **Prototype unaffected**
  - Sealing/freezing only locks own properties, not prototype chain.
- **PreventExtensions**
  - Existing props untouched; can still be reconfigured unless sealed/frozen.

#### alert⚠️

- Defaults differ\*\*

  - Object literals → props are `{writable:true, enumerable:true, configurable:true}`.
  - `Object.defineProperty` defaults all flags to **false** unless specified.

- delete operator\*\*

  - Works only if `configurable:true`.
  - On globals: `delete varName` is ignored if declared with `var/let/const`.

- Forgetting descriptors when using `Object.defineProperty` can lock fields.
- Non-writable still mutable (objects):

  ```js
  const obj = {};
  // only x is non-writable but if x is an object then x.y is writable
  Object.defineProperty(obj, "x", { value: {}, writable: false });
  obj.x.y = 1; // works! shallow only
  ```

- Configurable = false:

  - Property cannot be deleted or redefined. Permanent. after false
  - Can still modify writable value if only writable=true.

---

### Object Property Access — Dot vs Bracket

#### Dot Notation

```js
const obj = { name: "Kalidas", age: 28 };
console.log(obj.name); // "Kalidas"
```

- Property name written literally.
- Cleaner & faster to read.
- Only works with **valid identifiers** (letters, digits, `_`, `$`, no spaces).
- Dot fails on keys with spaces, hyphens, digits at start:

  ```js
  obj.first - name; // NaN (parsed as obj.first - name)
  obj["first-name"]; // works
  ```

- Dot cannot use variable key:

  ```js
  const prop = "age";
  obj.prop; // undefined
  obj[prop]; // ✅ 28
  ```

#### Bracket Notation

```js
const key = "age";
console.log(obj[key]); // 28
console.log(obj["name"]); // "Kalidas"

// Symbols are accessible only with bracket notation
const id1 = Symbol("id");
const user = { [id1]: 123 };
console.log(user[id1]); // 123

// accessing value without id1 directly using symbol
console.log(user["Symbol(id)"]); // undefined because Symbol is not a string
// we need to get the symbol property key
const [sym] = Object.getOwnPropertySymbols(user);
console.log(user[sym]); // ✅ 123
```

- Property name given as **string or symbol**.
- Useful for **dynamic keys**.
- Allows invalid identifiers (`obj["first-name"]`).
- Some reserved keywords allowed as keys in old engines if brackets are used:

  ```js
  obj["default"]; // safe
  ```

#### alert⚠️

- Numeric keys `"0"` and `0` are treated same:
  ```js
  const arr = [10, 20];
  arr["0"] === arr[0]; // true
  ```
- Optional chaining works with both:
  ```js
  obj?.name;
  obj?.["name"];
  ```
- Property existence
  - Accessing missing key via either → `undefined`, not error.
  - Exception: accessing on `null`/`undefined` → TypeError (use `?.`).

---

## Best Practices:

- Use object literals for most cases.
- Use `Object.create(null)` for pure maps.
- Use spread for shallow copies, structuredClone for deep.
- Prefer `Object.hasOwn(obj, key)` over `hasOwnProperty`.
- Avoid modifying `Object.prototype`.
- Use `defineProperty` only when controlling flags.
- Prefer `Object.freeze` / `seal` for immutability contracts.
- Use `class` syntax for clarity, but remember it’s prototype sugar.
- Keep prototype chains shallow for perf.
- Use Symbols for truly private keys.
- Prefer **dot notation** when key is a valid identifier.
- Use **bracket notation** for dynamic keys, symbols, invalid identifiers.
- Use optional chaining (`?.`) for safety.
- Use **freeze** for constants and configuration objects.
- Use **seal** when structure should be fixed but values editable.
- Use **preventExtensions** for controlled object growth.
- For true immutability, implement **deep freeze utility**.
- Don’t overuse on large/critical perf objects.

---

## Mnemonics

- “Objects store, Prototypes share.”
- “Dot for static, Bracket for dynamic.”
- “Prototype Chain = Lookup ladder: own → parent → Object → null.”
- PreventExtensions → ❌ add ✅ delete/edit values
- Seal → ❌ add/delete, ✅ edit values
- Freeze → ❌ add/delete/edit

---

---

---

# 🎯 JavaScript

## **Concept:** Two syntaxes to access object properties.

---

---

```js

```

---

# 🏫 JavaScript ES6 `` — Cheat Card

**Concept:**

---

---

### Example

```js

```

---

### Gotchas ⚠️ (Exhaustive)

1. **Syntactic sugar only**
   - Classes still use prototypes under the hood.
2. **Not hoisted**

3. **Strict mode always on**
   - All class bodies run in `"use strict"`.
4. **Constructor rules**

5. **Static vs instance confusion**

   ```js

   ```

6. **Private fields `#`**

7. **Class fields**

8. **Inheritance quirks**

9. **Performance**

- Defining methods inside constructor instead of `prototype` creates per-instance copies → memory bloat.

---

### Best Practices

---

## 👉 Mnemonic: \*\*\*\*

## Would you like me to follow this with a **cheat card comparing `class` vs prototype-based inheritance** (to show how sugar translates under the hood)?

## Here’s your **Concept Mastery Cheat Sheet** for **Constructors, Fields, and Methods** in ES6+ `class` syntax — crisp, complete, and with **all gotchas grouped together**.

# 🏗️ JavaScript Classes: Constructors, Fields, Methods — Cheat Card

**Concept:**

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
   - Omitting `super()` before using `this`
2. **Multiple constructors ❌**
3. **Class fields quirks**
   - Each instance gets a fresh copy of fields.
   - Reference types shared by all instances only if defined on prototype.
4. ## \*\*
   - Breaks JSON serialization (ignored).
5. **Static vs instance confusion**
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
10. **Performance**

- Defining methods in constructor (vs prototype) creates new function per instance → memory heavy.

---

### Best Practices

- ✅ Put reusable logic in prototype methods, not per-instance.
- ✅ Use static methods for utilities related to class but not tied to instance.
- ❌ Don’t abuse private `#` if simpler closure-based privacy suffices.

---

## 👉 Mnemonic: \*\*\*\*

## Would you like me to now expand this into a **cheat card comparing Instance Methods vs Static Methods vs Prototype Methods** (since that’s a common interview confusion)?

## Here’s your **Concept Mastery Cheat Sheet** for **Static Methods, Getters & Setters** in JavaScript classes — sharp, compact, and with **all gotchas grouped together**.

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

- **Getter**:
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
9. ## **Performance**

---

### Best Practices

---

## 👉 Mnemonic: **“Static = class tools, .”**

## Do you want me to also prepare a **cheat card on Private Fields & Static Private Methods (`#field`, `static #method`)** since they extend this concept and are common in ES2022+ interviews?

## Here’s your **Concept Mastery Cheat Sheet** for **Private Fields (`#field`)** in JavaScript — compact, thorough, and with **all gotchas grouped together**.

# 🔒 JavaScript Private Fields (`#field`) — Cheat Card

**Concept:**
ES2022+ feature: Class fields are truly private — accessible only inside the class body.

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

---

### Gotchas ⚠️ (Exhaustive)

1. **Hard privacy**
   - Cannot be accessed/reflected externally.
   - Not enumerable, not in `Object.keys`, `for...in`, `JSON.stringify`.
2. **Different identity**
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

   -

5. **Performance**
   - Private fields slightly slower in some engines (hidden class ops).
6. **Weak encapsulation workaround**
   - privacy used `WeakMap` or closures; these.
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

## 👉 Mnemonic: **“#field = hard private, not enumerable, not inheritable, not reflective.”**

## Would you like me to now create a **cheat card comparing `#private fields` vs `_underscore convention` vs `WeakMap-based privacy`**, since interviewers often ask why `#` is better than old patterns?

## Here’s your **Concept Mastery Cheat Sheet** for **Inheritance & `super`** in JavaScript — concise, interview-ready, and with **all gotchas grouped together**.

# 🧬 JavaScript Inheritance & `super` — Cheat Card

**Concept:**

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

- ✅ Keep chains shallow; prefer composition if possible.
- ✅ Call `super()` early in subclass constructor.
- ✅ Use `super.method()` sparingly; don’t overcomplicate overrides.
- ❌ Don’t mutate prototypes after class creation.
- ✅ For multiple behaviors, use **mixins** instead of deep inheritance.

---

## 👉 Mnemonic: \*\*\*\*

## Would you like me to also prepare a **cheat card on Mixins (multiple inheritance simulation in JS)** since it often follows the inheritance topic in interviews?

## Here’s your **Concept Mastery Cheat Sheet** for **`this` Binding Rules** in JavaScript — compact, interview-focused, and with **all gotchas grouped together**.

# 🗂️ JavaScript: Object vs Map — Cheat Card

**Concept:**

---

---

### API

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

- ✅ Prefer `Object.create(null)` when using object purely as dictionary.
- ✅ Use `Object.hasOwn(obj,key)` to avoid inherited props.
- ❌ Don’t serialize Map directly — convert first.

---

## 👉 Mnemonic: **“Object = Record, Map = Dictionary.”**

## Would you like me to also prepare a **cheat card on Map vs WeakMap vs WeakSet** (since that’s a common follow-up in interviews)?

## Here’s your **Concept Mastery Cheat Sheet** for **Array vs Set** in JavaScript — crisp, side-by-side, and with **all gotchas grouped together**.
