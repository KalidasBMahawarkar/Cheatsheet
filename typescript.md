# 🧠 TypeScript Cheat Sheet — Complete Edition

---

## 🧱 1. **Core Type System**

| Concept                       | Explanation                                    | Example                                               |
| ----------------------------- | ---------------------------------------------- | ----------------------------------------------------- |
| `string`, `number`, `boolean` | Basic primitive types                          | `let name: string = "Kalidas";`                       |
| `any`                         | Disables type checking (use rarely)            | `let data: any = 10; data = "str";`                   |
| `unknown`                     | Safer version of `any`; must narrow before use | `if (typeof input === "string") input.toUpperCase();` |
| `never`                       | Represents impossible value                    | `function fail(): never { throw new Error("err"); }`  |
| `void`                        | For functions returning nothing                | `function log(): void { console.log("done"); }`       |
| `null` / `undefined`          | Empty or missing values                        | `let n: null = null;`                                 |
| `bigint` / `symbol`           | Large integers, unique identifiers             | `const id: symbol = Symbol();`                        |
| Arrays                        | List of same type                              | `let arr: number[] = [1, 2, 3];`                      |
| Tuples                        | Fixed length + types                           | `let tup: [string, number] = ["id", 1];`              |
| Object                        | Shape definition                               | `let user: { name: string; age: number };`            |
| Enum                          | Named constants                                | `enum Role { ADMIN, USER }`                           |
| Literal Type                  | Exact value allowed                            | `let dir: "up"\|"down";`                              |
| Union Type                    | One of several types                           | `let val: string\|number;`                            |
| Intersection Type             | Combine multiple types                         | `type Full = A & B;`                                  |
| Type Alias                    | Custom name for type                           | `type ID = string\|number;`                           |
| Readonly                      | Prevent modification                           | `readonly id: number;`                                |
| Optional                      | May be absent                                  | `age?: number;`                                       |

| Interface                                                                              | Class                                                                                         |
| -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Defines **what** an object should look like                                            | Defines **how** an object behaves                                                             |
| Used for **type-checking and contracts**                                               | Used for **object creation and execution**                                                    |
| Exists **only at compile-time** (erased in JS)                                         | Exists **at runtime** (becomes JS class/function)                                             |
| `interface User { name: string; age: number; greet(): void; // method signature only}` | `class UserClass { constructor(public name: string, public age: number) {}; greet(): void {}` |

⚠️ **Gotchas:**

- `any` disables all safety. Prefer `unknown`.
- `never` means “can’t happen” (e.g., function never returns).

---

## ⚙️ 2. **Functions**

| Concept          | Explanation                   | Example                                                             |
| ---------------- | ----------------------------- | ------------------------------------------------------------------- |
| Function Type    | Type annotation for functions | `let add: (a:number,b:number)=>number;`                             |
| Optional Params  | Parameter may be skipped      | `function log(msg?: string) {}`                                     |
| Default Params   | Provide fallback              | `function greet(name="User") {}`                                    |
| Rest Params      | Variable args                 | `function sum(...nums:number[]) {}`                                 |
| Overloads        | Multiple call signatures      | `function parse(x:string):string; function parse(x:number):number;` |
| Return `void`    | No return value               | `function print(): void {}`                                         |
| Return `never`   | Throws error or infinite loop | `function crash(): never { throw "Err"; }`                          |
| `this` parameter | Explicitly type `this`        | `function f(this:HTMLElement){}`                                    |

⚠️ **Gotchas:**

- Arrow functions don’t have their own `this`.
- Overloads must have **implementation signature** last.

---

## 🧩 3. **Classes & OOP**

| Concept                  | Explanation                  | Example                                         |
| ------------------------ | ---------------------------- | ----------------------------------------------- |
| Class                    | Defines structure & behavior | `class Car { drive(){} }`                       |
| Constructor              | Initializes object           | `constructor(public name:string){}`             |
| Access Modifiers         | Encapsulation                | `private`, `protected`, `public`                |
| Readonly                 | Immutable after init         | `readonly id:number`                            |
| Inheritance              | `extends` base class         | `class Dog extends Animal {}`                   |
| `super()`                | Call parent constructor      | `super(name)`                                   |
| Abstract                 | Must be implemented in child | `abstract class Shape { abstract draw():void }` |
| Interface Implementation | Contract enforcement         | `class Circle implements Shape {}`              |
| Static                   | Shared across all instances  | `static version=1.0`                            |
| Getters/Setters          | Controlled property access   | `get val() {}; set val(v) {};`                  |

⚠️ **Gotchas:**

- Fields can’t be used **before `super()`**.
- `private` members not inherited; `protected` are.

---

## 🧠 4. **Advanced Types**

| Concept                | Explanation           | Example                                    |
| ---------------------- | --------------------- | ------------------------------------------ |
| Type Guard             | Runtime check         | `if (typeof x === "string") ...`           |
| Discriminated Union    | Tagged type pattern   | `{ type: "A" }\| { type: "B" }`            |
| Mapped Types           | Transform all props   | `type Read<T> = { [K in keyof T]: T[K] };` |
| Conditional Types      | If–Else for types     | `T extends U ? X : Y`                      |
| Indexed Access         | Access property type  | `Person["name"]`                           |
| `keyof`                | Keys as union         | `keyof Person → "name"\|"age"`             |
| `infer`                | Extract inferred type | `T extends infer R ? R : never`            |
| Template Literal Types | Dynamic string types  | `` type ID = `user-${number}`; ``          |

---

## 🧰 5. **Utility Types**

| Utility           | Use                     | Example                             |
| ----------------- | ----------------------- | ----------------------------------- |
| `Partial<T>`      | Make all props optional | `Partial<User>`                     |
| `Required<T>`     | Make all props required | `Required<User>`                    |
| `Readonly<T>`     | Make all props readonly | `Readonly<User>`                    |
| `Pick<T,K>`       | Choose specific props   | `Pick<User, "id"\|"name">`          |
| `Omit<T,K>`       | Exclude props           | `Omit<User, "password">`            |
| `Record<K,T>`     | Map keys to values      | `Record<string, number>`            |
| `ReturnType<T>`   | Get return type         | `ReturnType<typeof fn>`             |
| `Parameters<T>`   | Get parameter types     | `Parameters<typeof fn>`             |
| `Awaited<T>`      | Unwrap promise result   | `Awaited<Promise<string>> → string` |
| `NonNullable<T>`  | Remove null/undefined   | `NonNullable<string\|null>`         |
| `InstanceType<T>` | Get class instance type | `InstanceType<typeof MyClass>`      |

---

## 🚀 6. **Generics**

| Concept                    | Explanation                          | Example                            |
| -------------------------- | ------------------------------------ | ---------------------------------- |
| Basic Generic              | Works with any type                  | `function id<T>(x:T):T{return x;}` |
| Generic Constraints        | Limit type                           | `<T extends Lengthwise>`           |
| Default Type               | Set fallback                         | `<T=string>`                       |
| Generic Class              | Type-safe class                      | `class Box<T>{value:T}`            |
| Generic Interface          | Type-safe contracts                  | `interface Resp<T>{data:T}`        |
| Generic Functions + Arrays | `function merge<T,U>(a:T,b:U):T&U{}` |                                    |

⚠️ **Gotchas:**

- Constraints ensure available props.
- Default generic makes flexible APIs.

---

## 🌍 7. **Modules & Imports**

| Concept          | Explanation                         | Example                     |
| ---------------- | ----------------------------------- | --------------------------- |
| Named Export     | Export many                         | `export function fn(){}`    |
| Default Export   | One per file                        | `export default class A {}` |
| Import Named     | `import { fn } from './mod';`       |                             |
| Import Default   | `import A from './mod';`            |                             |
| Re-export        | `export * from './mod';`            |                             |
| Type-only import | `import type {User} from './types'` |                             |
| Namespace (old)  | `namespace Utils { export fn(){} }` |                             |

---

## 🧩 8. **Type Inference & Compatibility**

| Concept               | Explanation                 | Example                       |
| --------------------- | --------------------------- | ----------------------------- |
| Inference             | TS auto-detects type        | `let x = 10;`                 |
| Widening              | Literal → broad             | `"hi"` → `string`             |
| Narrowing             | Based on condition          | `if(typeof x==='string')`     |
| Structural Typing     | Based on shape              | `{a:1}` fits `{a:number}`     |
| Excess Property Check | Warn extra fields           | `{name:'a', extra:1}` invalid |
| Type Compatibility    | Assign if structure matches | `A = B` if shape same         |

---

## 🧩 9. **Decorators (Experimental)**

| Type                | Example        | Purpose              |
| ------------------- | -------------- | -------------------- |
| Class Decorator     | `@Component()` | Attach metadata      |
| Method Decorator    | `@Log()`       | Wrap/track behavior  |
| Property Decorator  | `@Column()`    | Modify fields        |
| Parameter Decorator | `@Inject()`    | Dependency injection |

⚠️ **Used in:** NestJS, Angular
Enable with `"experimentalDecorators": true`

---

## 🔐 10. **Error Handling & Strict Mode**

| Setting                        | Purpose                           |
| ------------------------------ | --------------------------------- |
| `strict`                       | Enables all strict rules          |
| `noImplicitAny`                | Disallow `any` type               |
| `strictNullChecks`             | Treat `null/undefined` separately |
| `strictPropertyInitialization` | Ensure constructor assigns props  |
| `useUnknownInCatchVariables`   | Type-safe catch blocks            |

---

## ⚡ 11. **Async & Promises**

| Concept        | Example                                 |
| -------------- | --------------------------------------- |
| Async Function | `async function f() { return 1; }`      |
| Await Keyword  | `const data = await fetch();`           |
| Promise Type   | `Promise<string>`                       |
| Generic Async  | `async function get<T>(): Promise<T>{}` |
| `Awaited<T>`   | Extract resolved value                  |

⚠️ Always wrap in `try/catch` to handle rejections.

---

## 🧩 12. **TypeScript + JS Interop**

| Concept           | Explanation                   |
| ----------------- | ----------------------------- |
| `.d.ts` files     | Declaration types for JS libs |
| `declare` keyword | Declare globals or functions  |
| DefinitelyTyped   | `npm i @types/express`        |
| JSDoc Annotations | `/** @type {number} */`       |
| `allowJs`         | Compile JS + TS               |
| `checkJs`         | Type-check JS                 |

---

## 🧪 13. **Config & Tooling**

| File                | Purpose                                    |
| ------------------- | ------------------------------------------ |
| `tsconfig.json`     | Compiler config                            |
| `target`            | Output JS version                          |
| `module`            | CommonJS / ESNext                          |
| `strict`            | Enable strict checks                       |
| `outDir`, `rootDir` | Folder structure                           |
| `paths`, `baseUrl`  | Alias imports                              |
| Tools               | `ts-node`, `nodemon`, `eslint`, `prettier` |

---

## 🧭 14. **Practical Patterns**

| Pattern                 | Example                                          |
| ----------------------- | ------------------------------------------------ |
| Type-safe REST DTO      | `interface CreateUserDto { name: string; }`      |
| Express Middleware Type | `(req:Request,res:Response,next:Next)`           |
| Type-safe API Response  | `interface ApiRes<T> { data:T; error?:string; }` |
| Validation Schema Type  | `type LoginReq = z.infer<typeof schema>;`        |
| NestJS Decorators       | `@Body()`, `@Param()`, `@Injectable()`           |

---

## 🧩 15. **TS 5.x+ Modern Features**

| Feature            | Example                            | Benefit                                |
| ------------------ | ---------------------------------- | -------------------------------------- |
| `satisfies`        | `const a = {x:1} satisfies Point;` | Ensures conformity but keeps inference |
| Const Type Params  | `function log<const T>(val:T){}`   | Keeps literal type                     |
| Decorator Standard | `@log accessor name;`              | ECMAScript-standard decorators         |
| Using Declarations | `using file = open("path");`       | Auto-dispose resources                 |

---

# 🧩 Quick Reference Summary

| Category   | Topics Covered              |
| ---------- | --------------------------- |
| Basics     | Types, Objects, Unions      |
| Functions  | Overloads, Params, Return   |
| OOP        | Classes, Access, Abstract   |
| Advanced   | Guards, Mapped, Conditional |
| Utilities  | Partial, Pick, Omit, etc.   |
| Generics   | Function/Class Constraints  |
| Modules    | Import/Export               |
| Inference  | Structural typing           |
| Decorators | Class, Method, Param        |
| Config     | tsconfig setup              |
| Async      | Promise, Awaited            |
| Interop    | JS + TS integration         |
| Modern     | `satisfies`, Decorator API  |

---
