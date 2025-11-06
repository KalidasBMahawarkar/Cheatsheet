Classes

```js
class C extends P {
  static s = 1; // public static field
  #p = 0; // private field
  x = 1; // public field
  constructor(a) {
    super();
    this.a = a;
  }
  get v() {
    return this.#p;
  }
  set v(n) {
    this.#p = n;
  }
  m() {
    return this.a;
  }
  static of(...args) {
    return new C(...args);
  }
}
```

export function f() {}
export default class {}

import x, { a as b } from "./mod.js";
import \* as M from "./mod.js";
const m = await import("./mod.js"); // dynamic

```js
const p = new Promise((res, rej) => {
  res(1);
});
p.then((x) => console.log(x));
p.catch((e) => console.error(e));
p.finally(() => console.log("done"));
await Promise.all([p1, p2]); // waits for all to resolve;
await Promise.allSettled([p1, p2]); // waits for all to settle;
await Promise.race([p1, p2]); // waits for the first to settle;
await Promise.any([p1, p2]); // waits for the first to resolve;
```

JSON.stringify(obj, replacer?, space?);
JSON.parse(s, reviver?);
