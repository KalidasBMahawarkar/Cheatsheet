Here’s your **Testing & Tooling Cheat Card** — covering the ecosystem essentials for senior-level JavaScript/Node interviews.
---
# 🧪 JavaScript Testing & Tooling — Cheat Card
**Concept:**
Testing ensures correctness, confidence, and maintainability. Tooling (linters, formatters, bundlers, CI) enforces quality, consistency, and automation in modern JS development.
---
## Testing Types & Frameworks
### 1) **Unit Tests**
* Smallest testable pieces (functions, utils).
* Tools: **Jest, Mocha, Vitest**.
```js
test("adds numbers", () => {
  expect(2 + 3).toBe(5);
});
```
---
### 2) **Integration Tests**
* Modules/services working together.
* Tools: Jest + Supertest (for APIs), Playwright/TestCafe.
```js
const request = require("supertest");
await request(app).get("/health").expect(200);
```
---
### 3) **End-to-End (E2E)**
* Full system workflow (UI ↔ API ↔ DB).
* Tools: **Cypress, Playwright, Puppeteer**.
```js
cy.visit("/login");
cy.get("input#user").type("kalidas");
cy.get("button#login").click();
cy.url().should("include", "/dashboard");
```
---
### 4) **Mocks & Stubs**
* **jest.fn()**, **sinon**, or manual.
* Replace network/db calls for isolation.
```js
const mockFetch = jest.fn().mockResolvedValue({data:"ok"});
```
---
### 5) **Code Coverage**
* Measures % of code exercised by tests.
* Tools: **nyc (Istanbul), Jest --coverage**.
---
## Tooling Essentials
### 1) **Linters & Formatters**
* ESLint: catch bugs, enforce style.
* Prettier: consistent formatting.
* Combine: ESLint plugin for Prettier.
---
### 2) **Type Checking**
* TypeScript → compile-time safety.
* `tsc --noEmit` or `ts-jest` for testing typed code.
---
### 3) **Bundlers / Builders**
* Webpack, Rollup, Parcel, Vite.
* Tree-shaking, code splitting, asset pipeline.
---
### 4) **Package & Dependency Management**
* npm, yarn, pnpm.
* Lockfiles + `npm audit` for vulnerabilities.
---
### 5) **Version Control / CI/CD**
* Git + GitHub Actions, Jenkins, GitLab CI.
* Run tests, lint, build, and deploy pipelines automatically.
---
### 6) **Debugging Tools**
* Node.js inspector: `node --inspect`.
* VSCode debugger, Chrome DevTools.
* Logging libs: Winston, Pino.
---
### 7) **Performance & Profiling**
* Lighthouse (frontend).
* Node: `clinic.js`, `0x`, `perf_hooks`.
---
### 8) **Static Analysis & Security**
* SonarLint / SonarQube → code quality.
* Snyk / npm audit → security scans.
---
## Visualization
```
Testing Pyramid
   ▲
   │   E2E (UI/system tests)
   │   Integration (services)
   │   Unit (functions/modules)
   ▼
   More tests, faster, smaller
```
---
## Gotchas ⚠️
1. **Over-mocking** → fragile tests; mock only externals.
2. **Async tests** → must return/await Promises; forgetting leads to false positives.
3. **Flaky E2E tests** → caused by timing, network, or async race conditions.
4. **Coverage ≠ quality** → 100% coverage can still miss logic bugs.
5. **Tool conflicts** → ESLint vs Prettier config must be aligned.
6. **CI/CD environment mismatch** → tests may pass locally but fail in pipeline (different env vars, DB states).
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Confidence in code changes.
* Catches regressions early.
* Standardized formatting + linting → cleaner PRs.
* Automation reduces human error.
**❌ Cons**
* Writing + maintaining tests adds overhead.
* Flaky or brittle tests erode trust.
* Misconfigured tooling can block productivity.
---
## Best Practices
* **Follow testing pyramid**: Many unit tests, fewer integration, fewer E2E.
* Keep tests **isolated and deterministic**.
* Always test **critical paths** (auth, payments, DB ops).
* Enforce lint + format in CI/CD.
* Use **Husky** + **lint-staged** for pre-commit checks.
* Monitor coverage, but prioritize meaningful assertions.
* Automate with CI/CD for every push/PR.
---
## Mnemonic / Quick Recall
**“Test → Lint → Build → Ship.”**
* Test correctness.
* Lint & format for consistency.
* Build & bundle for delivery.
* Automate with CI/CD for safety.
---
👉 Do you want me to expand this into a **Testing Tools Comparison Table** (Jest vs Mocha vs Vitest vs Cypress vs Playwright) with pros/cons? That’s a common panel-round interview request.

Here’s your **Unit Testing with Jest & Mocha Cheat Card** — deep, interview-ready, and structured.
---
# 🧪 Unit Testing with Jest & Mocha — Cheat Card
**Concept:**
Unit testing isolates the smallest units (functions, modules) to verify correctness.
* **Jest:** All-in-one (test runner + assertions + mocks).
* **Mocha:** Flexible test runner; often paired with Chai (assertions) & Sinon (mocks).
---
## Syntax / Structure
### Jest
```js
describe("suite", () => {
  beforeAll(() => {});
  afterAll(() => {});
  beforeEach(() => {});
  afterEach(() => {});
  test("adds", () => {
    expect(1+2).toBe(3);
  });
  it("supports async", async () => {
    await expect(Promise.resolve(42)).resolves.toBe(42);
  });
});
```
### Mocha + Chai
```js
const { expect } = require("chai");
describe("suite", () => {
  before(() => {});
  after(() => {});
  beforeEach(() => {});
  afterEach(() => {});
  it("adds", () => {
    expect(1+2).to.equal(3);
  });
  it("async/await works", async () => {
    const res = await Promise.resolve(42);
    expect(res).to.equal(42);
  });
});
```
---
## Examples
```js
// 1) Function test
function add(a,b){ return a+b; }
test("add numbers", () => {
  expect(add(2,3)).toBe(5); // Jest
});
// Mocha+Chai
it("add numbers", () => {
  expect(add(2,3)).to.equal(5);
});
// 2) Async test
async function fetchData(){ return "ok"; }
test("fetchData async", async () => {
  await expect(fetchData()).resolves.toBe("ok");
});
// Mocha
it("fetchData async", async () => {
  const res = await fetchData();
  expect(res).to.equal("ok");
});
// 3) Mocks & spies
const fetch = jest.fn().mockResolvedValue("data");
test("mock fn", async () => {
  const res = await fetch();
  expect(fetch).toHaveBeenCalled();
  expect(res).toBe("data");
});
// Mocha + Sinon
const sinon = require("sinon");
it("spy fn", () => {
  const spy = sinon.spy();
  spy(42);
  expect(spy.calledOnceWith(42)).to.be.true;
});
```
---
## Visualization
```
Test Suite
 ├─ Lifecycle hooks: beforeAll / beforeEach / afterEach / afterAll
 ├─ Tests: it() / test()
 └─ Assertions: expect(actual).matcher(expected)
```
---
## Gotchas ⚠️
1. **Async tests must return/await**
   Forgetting → test passes silently before completion.
   * Jest: `return promise` or `await`.
   * Mocha: must `return`/`await` or use `done()`.
2. **Jest’s fake timers**
   Need `jest.useFakeTimers()` and `jest.advanceTimersByTime(ms)` for timers.
3. **Mocking modules**
   * Jest: `jest.mock("./dep")`.
   * Mocha: no built-in mocking → use Sinon.
4. **Globals differ**
   * Jest has `expect` globally.
   * Mocha requires explicit imports.
5. **Coverage**
   * Jest: built-in (`jest --coverage`).
   * Mocha: external tool (`nyc mocha`).
6. **Parallelism**
   * Jest runs tests in parallel.
   * Mocha runs sequentially (parallel mode experimental).
7. **Snapshot testing**
   Jest supports `expect(val).toMatchSnapshot()`. Mocha doesn’t (needs plugin).
---
## ✅ Pros & ❌ Cons
**Jest**
✅ All-in-one (assertions, mocks, spies, coverage).
✅ Snapshots, fake timers, parallel runner.
❌ Slower in huge repos.
**Mocha**
✅ Highly flexible, minimal core.
✅ Works well with Chai/Sinon/custom libs.
❌ More setup (assertion, mocks, coverage not included).
❌ Sequential by default → slower large suites.
---
## Best Practices
* Keep unit tests small & isolated.
* Use **mocks** for external deps (DB, APIs).
* Favor **deterministic tests** — no time/network randomness.
* Write both **happy path & edge case** tests.
* Keep naming consistent: `should <expected behavior>`.
* Don’t test implementation — test behavior.
* Run tests in CI/CD with coverage gates.
---
## Mnemonic / Quick Recall
**“Jest = Batteries included; Mocha = Bring your own stack.”**
---
👉 Do you want me to build a **comparison cheat table (Jest vs Mocha vs Vitest)** — with speed, ecosystem, features, and interview use-cases?

Here’s your **Test Doubles Cheat Card** — covering **Spies, Mocks, and Stubs** (the holy trinity of unit testing).
---
# 🎭 Test Doubles — Spies, Mocks, Stubs
**Concept:**
*Test doubles* replace real objects/functions in tests. They isolate units, simulate behavior, and verify interactions.
---
## Types
### 1) **Spy**
* Records calls, arguments, and return values.
* Doesn’t change behavior unless told.
```js
// Jest
const spy = jest.fn();
spy(42);
expect(spy).toHaveBeenCalledWith(42);
// Sinon
const spy = sinon.spy();
spy("x");
expect(spy.calledOnceWith("x")).to.be.true;
```
---
### 2) **Stub**
* Replaces a function with controlled behavior.
* Defines what to return/throw.
```js
// Jest
const stub = jest.fn().mockReturnValue("fixed");
stub(); // "fixed"
// Sinon
const stub = sinon.stub().returns(123);
stub(); // 123
```
---
### 3) **Mock**
* A spy/stub with **expectations** (predetermined behavior + assertions).
* More strict → test fails if expectations not met.
```js
// Jest auto-mock
jest.mock("./api");
api.getData.mockResolvedValue("mocked");
// Sinon mock
const obj = {add(x,y){return x+y;}};
const mock = sinon.mock(obj);
mock.expects("add").once().withArgs(2,3).returns(99);
obj.add(2,3); // 99
mock.verify(); // passes
```
---
## Visualization
```
Spy  → Watch: did it get called? with what?
Stub → Replace: force a return/throw
Mock → Expect: pre-program + verify interactions
```
---
## Gotchas ⚠️
1. **Overusing mocks** → tests become brittle & tied to implementation, not behavior.
2. **Spies don’t isolate** → real side effects may still run unless stubbed.
3. **Stubs need cleanup** → restore originals after test (Sinon: `stub.restore()`).
4. **Mocks with expectations** → tests fail if not called exactly as specified.
5. **Global state** → leaving mocks active can pollute other tests.
6. **Framework differences**
   * Jest: `jest.fn()`, `jest.spyOn`, `jest.mock`.
   * Sinon: `spy`, `stub`, `mock` separately.
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Isolate units from external deps.
* Control edge cases easily.
* Verify interactions in complex flows.
**❌ Cons**
* Too many mocks → fragile tests.
* Easy to test *implementation* instead of behavior.
* Maintenance overhead if APIs change.
---
## Best Practices
* **Prefer spies/stubs** → simpler, less brittle.
* Use mocks sparingly, only for critical interaction contracts.
* Always **reset/restore** between tests.
* Don’t mock what you don’t own (e.g., third-party libs) — wrap them.
* Focus on **observable behavior**, not internal calls.
---
## Mnemonic / Quick Recall
**“Spy = Watch, Stub = Replace, Mock = Expect.”**
---
👉 Want me to also build a **side-by-side Jest vs Sinon cheat table** for spies, stubs, and mocks? That’s a classic interview trap.

Here’s your **ESLint Cheat Card** — tailored for interviews and practical dev use.
---
# 📏 Code Linting with ESLint — Cheat Card
**Concept:**
**ESLint** is a pluggable static analysis tool for JavaScript/TypeScript. It enforces coding standards, detects errors, and improves consistency across teams.
---
## Syntax / Structure
```bash
# Init config
npx eslint --init
# Run
npx eslint src/ --fix
```
**Config file:** `.eslintrc.{js,json,yml}`
```js
module.exports = {
  env: { browser: true, es2021: true, node: true },
  extends: ["eslint:recommended", "plugin:@typescript-eslint/recommended", "prettier"],
  parserOptions: { ecmaVersion: "latest", sourceType: "module" },
  rules: {
    "no-unused-vars": "warn",
    "eqeqeq": "error",
  },
};
```
---
## Examples
```js
// Without ESLint
x = 10 // no 'let', semicolon missing
// With ESLint
// ✖  1:1  'x' is not defined.   no-undef
// ✖  1:1  Missing semicolon.   semi
```
```js
// Custom rule: enforce triple equals
if (a == b) {} // ESLint: "Expected '===' and instead saw '=='"
```
---
## Visualization
```
Your Code
   ↓
ESLint Parser → AST → Rules Engine
   ↓
Reports / Auto-fix (--fix)
```
---
## Gotchas ⚠️
1. **Prettier conflicts**
   Formatting rules (indent, quotes) may clash with Prettier → solve with `eslint-config-prettier`.
2. **Performance**
   Large repos slow → use caching: `eslint --cache`.
3. **Ignored files**
   Controlled via `.eslintignore`. Don’t forget `dist/` or `node_modules/`.
4. **Plugins required**
   For TypeScript, React, Vue → need `@typescript-eslint/eslint-plugin`, `eslint-plugin-react`, etc.
5. **Rules precedence**
   * Local `rules` override `extends`.
   * Multiple extends are merged left → right.
6. **False positives**
   Sometimes needs config tweaks per project (globals, env).
---
## ✅ Pros & ❌ Cons
**✅ Pros**
* Catches bugs early.
* Enforces consistent style.
* Highly configurable (rules, plugins).
* Integrates with IDEs/CI/CD.
**❌ Cons**
* Config can be complex.
* Strict rules may slow dev if misconfigured.
* Can overlap with Prettier (style vs logic).
---
## Best Practices
* Start with `eslint:recommended`.
* Add framework-specific plugins (`react`, `vue`, `@typescript-eslint`).
* Use `eslint-config-prettier` if also using Prettier.
* Run in **pre-commit hooks** (Husky + lint-staged).
* Run in **CI/CD** for consistency across team.
* Gradually enforce strict rules (begin with warnings).
---
## Mnemonic / Quick Recall
**“Lint Early, Fix Fast.”**
---
👉 Do you want me to also create a **side-by-side ESLint vs Prettier comparison card** (purpose, scope, overlap, when to use) since that’s a common interview curveball?

Got it 👍 — here’s a **3-in-1 Cheat Card** covering **Type Checking, Bundlers, and Transpilers** — compact, but deep enough for senior-level interviews.
---
# 🛠️ Type Checking, Bundlers, Transpilers — Cheat Card
---
## 🔍 Type Checking (TypeScript mindset)
**Concept:**
TypeScript (and tools like Flow) add **static type checking** on top of JS → catches errors at compile time, enforces contracts.
**Syntax / Examples**
```ts
// Basic
function greet(name: string): string {
  return `Hello, ${name}`;
}
// Interfaces & generics
interface User { id: number; name: string; }
function identity<T>(val: T): T { return val; }
// Strict null checks
let x: string | null = null;
```
**Gotchas ⚠️**
* **Structural typing**: compatible if shapes match, not just names.
* **Any** weakens type safety (avoid unless intentional).
* **Type erasure**: runtime has no type info → only compile-time.
* **Config matters** (`strictNullChecks`, `noImplicitAny`).
**Best Practices**
* Prefer `unknown` over `any`.
* Use strict mode always.
* Define interfaces for contracts, not just inline types.
* Use `tsc --noEmit` in pipelines for type-only checks.
---
## 📦 Bundlers (Webpack, Rollup, esbuild)
**Concept:**
Bundle multiple JS/TS modules into optimized assets for the browser/server.
**Key Players**
* **Webpack** → feature-rich, loaders/plugins ecosystem, most used.
* **Rollup** → tree-shaking & libraries (smaller output).
* **esbuild** → blazing-fast, written in Go, modern toolchains (Vite).
**Syntax / Config**
```js
// Webpack config snippet
module.exports = {
  entry: "./src/index.js",
  output: { filename: "bundle.js" },
  module: {
    rules: [{ test: /\.ts$/, use: "ts-loader" }]
  }
};
```
```js
// Rollup
export default {
  input: "src/index.js",
  output: { file: "bundle.js", format: "esm" }
};
```
**Gotchas ⚠️**
* Webpack config complexity.
* Rollup may need plugins for CommonJS.
* esbuild is fast but less customizable (not all plugins).
* Bundler vs no-bundler: modern browsers support `import` but bundlers still needed for legacy/optimization.
**Best Practices**
* Use Webpack for complex apps (React, Angular).
* Use Rollup for libraries.
* Use esbuild (or Vite) for dev speed.
* Enable tree-shaking, code-splitting.
---
## 🔄 Transpilers (Babel)
**Concept:**
Convert modern JS/TS → compatible older JS for wider browser support.
**Syntax / Usage**
```bash
# CLI
npx babel src --out-dir dist --presets=@babel/preset-env
# .babelrc
{
  "presets": ["@babel/preset-env", "@babel/preset-typescript"],
  "plugins": ["@babel/plugin-transform-runtime"]
}
```
**Gotchas ⚠️**
* Only syntax downleveling; doesn’t polyfill APIs unless configured (`@babel/polyfill` or `core-js`).
* Can bloat bundles if blindly including polyfills.
* Slower build vs esbuild/tsc (but more mature ecosystem).
**Best Practices**
* Use `@babel/preset-env` with `targets` to reduce bundle size.
* Pair with bundlers for full pipeline.
* For TypeScript: use `tsc` for type checks + Babel for transpilation.
* Consider replacing Babel with esbuild/SWC in modern stacks for speed.
---
## ✅ Pros & ❌ Cons Summary
**Type Checking**
* ✅ Catches errors early, self-docs code.
* ❌ Adds compile step, learning curve.
**Bundlers**
* ✅ Optimization, code splitting, polyfills.
* ❌ Config complexity, slower builds.
**Transpilers**
* ✅ Modern JS features anywhere.
* ❌ Doesn’t enforce types, only syntax-level.
---
## Mnemonic / Quick Recall
**“Types prevent, Bundlers collect, Transpilers convert.”**
---
👉 Do you want me to expand this into **comparison tables** (Webpack vs Rollup vs esbuild, Babel vs tsc vs swc) so you have quick interview-ready contrasts?
