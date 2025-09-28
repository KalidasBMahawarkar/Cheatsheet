Here’s your **Security in JavaScript Cheat Card** — tailored for backend/frontend interviews, covering the most important pitfalls & defenses.

---

# 🔒 Security in JavaScript — Cheat Card

**Concept:**
JavaScript (browser + Node.js) is highly exposed to **injection, misuse of APIs, and unsafe defaults**. Secure coding = validating input, escaping output, and following least-privilege.

---

## Common Vulnerabilities

### 1) **XSS (Cross-Site Scripting)**

* Injecting malicious JS into web pages.

```html
<input value="<script>alert(1)</script>">
```

* Mitigation:

  * Escape user input before rendering (`DOMPurify`).
  * Use frameworks with auto-escaping (React, Angular).
  * Apply Content Security Policy (CSP).

---

### 2) **CSRF (Cross-Site Request Forgery)**

* Attacker tricks browser into sending requests with cookies/auth.
* Mitigation:

  * Use CSRF tokens.
  * Prefer `SameSite=strict` cookies.
  * Validate origin headers.

---

### 3) **Injection Attacks**

* **SQL Injection** → unescaped input in queries.
* **NoSQL Injection** → untrusted input in Mongo/Prisma queries.

```js
db.users.find({ name: req.body.name }); // unsafe
```

* Mitigation:

  * Always use parameterized queries / ORMs.
  * Validate & sanitize inputs.

---

### 4) **Prototype Pollution**

* Malicious input modifies `Object.prototype`.

```js
JSON.parse('{"__proto__": {"admin": true}}');
```

* Mitigation:

  * Freeze/validate input objects.
  * Use safe deep-merge libraries.

---

### 5) **Eval / Function Constructor Abuse**

```js
eval(userInput); // ❌
```

* Mitigation:

  * Avoid `eval`, `new Function`, `setTimeout(str)`.
  * Use safe parsers.

---

### 6) **Insecure Storage**

* Sensitive info in `localStorage` (accessible by XSS).
* Mitigation:

  * Store tokens in HttpOnly cookies.
  * Encrypt at rest.

---

### 7) **Insecure Deserialization**

* Passing raw JSON into functions.
* Mitigation: Validate schema (`Joi`, `Zod`).

---

### 8) **Timing Attacks**

* Comparing secrets with `===` leaks timing differences.
* Mitigation: Use constant-time compare (`crypto.timingSafeEqual`).

---

### 9) **Denial of Service (DoS)**

* Regex DoS (catastrophic backtracking).
* Huge JSON payloads.
* Mitigation:

  * Limit input size & depth.
  * Use safe regex or libraries (`RE2`).

---

### 10) **Supply Chain Attacks**

* Malicious npm packages.
* Mitigation:

  * Pin dependencies with lockfiles.
  * Run `npm audit`, `snyk`.
  * Use trusted registries.

---

## Node.js Specific

* Disable `x-powered-by` headers.
* Avoid blocking event loop (prevents DoS).
* Use Helmet.js for HTTP headers.
* Rate-limit APIs (`express-rate-limit`).
* Use environment variables for secrets (not hardcoded).
* Rotate keys and tokens regularly.

---

## ✅ Pros & ❌ Cons of JS Security Tools

**✅ Pros**

* Express middlewares (Helmet, rate-limit) easy to drop in.
* ORMs/validators reduce injection risk.
* ESLint security plugins catch unsafe code.

**❌ Cons**

* Many defenses are opt-in.
* Still vulnerable to developer mistakes.
* Supply chain risk outside your control.

---

## Best Practices

* Validate ALL inputs (server + client).
* Escape outputs in HTML, URLs, SQL.
* Use HTTPS everywhere.
* Apply **principle of least privilege** (API keys, DB users).
* Rotate secrets + monitor logs.
* Keep dependencies updated.
* Run static analysis & penetration tests.
* Educate team → security is culture.

---

## Mnemonic / Quick Recall

**“V.E.R.I.F.Y.”**

* **V**alidate input
* **E**scape output
* **R**emove dangerous APIs (`eval`)
* **I**solate secrets (env vars)
* **F**orce HTTPS / CSP / tokens
* **Y**ield least privilege

---

👉 Do you want me to create a **side-by-side table of vulnerabilities (XSS, CSRF, Injection, etc.) → Symptoms → Mitigations** for quick interview recall?


Here’s your **Prototype Pollution Cheat Card** — this is a big one in interviews and real-world audits.

---

# 🧬 JavaScript Prototype Pollution — Cheat Card

**Concept:**
**Prototype pollution** happens when untrusted input modifies the `__proto__` (or `constructor.prototype`) of base objects, affecting all objects that inherit from it → leading to privilege escalation, data leaks, or logic corruption.

---

## Syntax / Structure (Attack Pattern)

```js
// Attacker payload
const payload = JSON.parse('{"__proto__": {"isAdmin": true}}');

// Merge unsafely
Object.assign({}, payload);

// Now ALL objects inherit polluted prop
({}).isAdmin; // true
```

---

## Examples

```js
// 1) Vulnerable deep merge
function unsafeMerge(target, src) {
  for (let k in src) {
    if (typeof src[k] === "object") {
      unsafeMerge(target[k] = target[k] || {}, src[k]);
    } else {
      target[k] = src[k];
    }
  }
  return target;
}
unsafeMerge({}, JSON.parse('{"__proto__": {"polluted": "yes"}}'));
console.log({}.polluted); // "yes" (polluted)

// 2) Exploiting libraries (historical CVEs)
// lodash <4.17.5, jQuery <3.4.0, Hoek <4.2.1
const _ = require("lodash");
_.merge({}, JSON.parse('{"__proto__": {"admin": true}}'));
console.log(({}).admin); // true
```

---

## Visualization

```
Base Object (Object.prototype)
   ↑
   ├─ Polluted (extra keys injected)
   ↑
   └─ All new objects inherit compromised property
```

---

## Gotchas ⚠️

1. **Affects global state**
   New objects inherit polluted properties — security & logic broken.

2. **Hidden attack vector**
   May not crash immediately — stealthy exploitation.

3. **JSON.parse alone is safe**
   Danger comes when blindly merging/spreading into objects.

4. **Not just **proto****
   Attackers can target `prototype`, `constructor`, `toString`, etc.

5. **Can bypass validations**
   e.g. `if (user.isAdmin)` may be true for all objects.

6. **Node.js specific risk**
   Middleware like Express body parsers + unsafe merges = global pollution.

---

## ✅ Pros & ❌ Cons

**✅ Pros (why devs use deep merge libs)**

* Easier nested object manipulation.
* Popular libraries = convenience.

**❌ Cons (why it’s dangerous)**

* Enables **privilege escalation**.
* Breaks **app logic silently**.
* **Global impact** — one request affects others.

---

## Mitigations (Best Practices)

* **Block dangerous keys**: denylist `__proto__`, `constructor`, `prototype`.

  ```js
  if (["__proto__","constructor","prototype"].includes(key)) continue;
  ```
* **Freeze prototypes** at app start:

  ```js
  Object.freeze(Object.prototype);
  Object.freeze(Array.prototype);
  ```
* **Use safe libraries** (post-patch lodash, deepmerge, etc.).
* **Schema validation** (e.g., Joi, Zod) to sanitize input.
* **Run linters/scanners** → ESLint plugin security, npm audit, Snyk.
* **Isolate input handling** → never merge raw client input into objects.

---

## Mnemonic / Quick Recall

**“Polluted Prototype = Poison for All Objects.”**

---

👉 Do you want me to create a **Red Team vs Blue Team reference card** (how prototype pollution is exploited vs how to defend) — useful for interviews in security-heavy companies?

Here’s your **Cross-Site Scripting (XSS) Cheat Card** — one of the most asked JavaScript/web security interview topics.

---

# 💥 Cross-Site Scripting (XSS) — Cheat Card

**Concept:**
XSS is when an attacker injects **malicious JavaScript** into a web page, which then runs in a victim’s browser. It hijacks trust between users and websites.

---

## Types of XSS

1. **Reflected XSS** (non-persistent)

   * Payload comes from request (URL/query params).
   * Executes immediately in response.

   ```url
   https://site.com?q=<script>alert(1)</script>
   ```

2. **Stored XSS** (persistent)

   * Payload saved in DB (comments, profile, chat).
   * Executes for every viewer.

3. **DOM-based XSS**

   * Client-side JS manipulates DOM unsafely.

   ```js
   document.body.innerHTML = location.hash; // if hash has <script>
   ```

---

## Examples

```html
<!-- Vulnerable -->
<input value="<?=$_GET['name']?>">

<!-- Attacker input -->
<script>alert("Hacked")</script>
```

```js
// DOM-based XSS
const input = location.search.split("=")[1];
document.querySelector("#msg").innerHTML = input; // unsafe
```

---

## Visualization

```
User input → [unsanitized] → HTML/DOM → Browser executes attacker JS
```

---

## Payloads (common)

```html
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
"><script>alert(1)</script>
```

---

## Gotchas ⚠️

1. **Not just <script> tags**

   * Handlers: `<img onerror>`, `<svg onload>`, `<a href=javascript:>`.
   * CSS/JSON injection can also lead to XSS.

2. **Encoding ≠ Escaping**

   * `&lt;script&gt;` prevents execution, but must be applied in **correct context** (HTML, attr, URL, JS).

3. **innerHTML is dangerous**

   * Always prefer `textContent` / `createElement`.

4. **Event handlers**
   Inline `onclick="..."` often exploitable.

5. **Polyglot payloads**
   Attackers mix formats (`<img src onerror="...">`).

6. **Framework differences**
   React/Angular auto-escape, but can be bypassed with `dangerouslySetInnerHTML` or `$sce.trustAsHtml`.

---

## ✅ Pros & ❌ Cons

**✅ Why attackers love XSS**

* Steals cookies (if not HttpOnly).
* Keylogging / phishing UI overlays.
* Session hijacking.
* Defacement, crypto-mining, malware drop.

**❌ For defenders**

* Hard to patch everywhere.
* Many input/output contexts.
* Legacy apps vulnerable.

---

## Mitigations (Best Practices)

* **Escape output by context**: HTML, attributes, JS, CSS, URLs.
* **Use safe APIs**: `textContent`, `setAttribute`, `DOMPurify`.
* **Apply CSP (Content Security Policy)**: block inline scripts, restrict origins.
* **HttpOnly cookies**: prevent theft via `document.cookie`.
* **Sanitize user input**: validate and strip dangerous content.
* **Avoid eval/Function/innerHTML**.
* **Framework safe defaults**: rely on React/Angular auto-escaping, avoid unsafe overrides.

---

## Mnemonic / Quick Recall

**“XSS = eXecute Stolen Scripts.”**

* Reflected → in URL.
* Stored → in DB.
* DOM → in JS.

---

👉 Do you want me to also make a **Red Team vs Blue Team XSS card** (how attackers craft payloads vs how defenders stop them)? That’s a favorite in security-heavy interviews.


Here’s your **CSRF (Cross-Site Request Forgery) Cheat Card** — interview-ready and compact with depth.

---

# 🎭 Cross-Site Request Forgery (CSRF) — Cheat Card

**Concept:**
CSRF tricks a victim’s browser into making **unwanted, authenticated requests** to a trusted site where the user is logged in — exploiting implicit trust in cookies/session.

---

## Attack Flow

1. Victim logs into `bank.com` → session cookie stored.
2. Attacker lures victim to `evil.com`.
3. Malicious code auto-submits a request:

```html
<img src="https://bank.com/transfer?amount=1000&to=attacker">
```

4. Browser sends request with session cookies → bank processes it as authenticated.

---

## Types

1. **Stored CSRF** → malicious link/form stored in trusted site.
2. **Reflected CSRF** → victim tricked into clicking crafted URL.
3. **Login CSRF** → attacker forces victim to log into attacker’s account.

---

## Examples

```html
<!-- Malicious form -->
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="amount" value="1000">
  <input type="hidden" name="to" value="attacker">
  <input type="submit">
</form>

<script>document.forms[0].submit()</script>
```

---

## Visualization

```
Victim Browser
   │  (with cookies/session)
   ▼
Trusted Site (bank.com) ← Request forged by evil.com
```

---

## Gotchas ⚠️

1. **CSRF relies on cookies being auto-sent**

   * LocalStorage/sessionStorage tokens are NOT auto-sent, but XSS can steal them.

2. **SameSite cookies** mitigate but not always enabled.

   * `SameSite=Lax` default (modern browsers).
   * Must explicitly use `SameSite=Strict` for sensitive ops.

3. **GET requests can be exploited** if state-changing ops use them.

4. **CORS is NOT a defense**

   * CSRF doesn’t need response access, only request sending.

5. **Multi-step flows** still vulnerable if tokens aren’t checked on each step.

---

## Mitigations ✅

* **CSRF tokens**

  * Unique per session/request.
  * Stored server-side, validated on each state-changing request.

  ```html
  <input type="hidden" name="csrf_token" value="abc123">
  ```

* **SameSite cookies**

  ```http
  Set-Cookie: sessionid=xyz; HttpOnly; Secure; SameSite=Strict
  ```

* **Double-submit cookie pattern** (send token both in cookie + body/header, compare).

* **Re-authentication / CAPTCHA** for critical actions.

* **Don’t use GET for mutations** — restrict to POST/PUT/DELETE.

---

## ✅ Pros & ❌ Cons (of defenses)

**✅ Pros**

* Tokens + SameSite provide strong layered defense.
* Frameworks (Django, Rails, Spring, Express middlewares) handle CSRF tokens automatically.

**❌ Cons**

* Misconfigured SameSite breaks logins across subdomains.
* Token-based defense requires careful sync on AJAX/SPAs.
* Legacy apps often use GET → very exploitable.

---

## Mnemonic / Quick Recall

**“CSRF = Cookie Sends Request Forged.”**

---

👉 Do you want me to also build a **CSRF vs XSS comparison card** (side-by-side table: attack vector, impact, defenses)? That’s a frequent senior-level interview question.

Here’s your **Eval & Function Constructor Dangers Cheat Card** — a must-know in **JavaScript security interviews**.

---

# ⚠️ `eval()` & `Function` Constructor — Dangers Cheat Card

**Concept:**

* `eval(code)`: Executes a string as JavaScript in current scope.
* `new Function(args…, body)`: Creates a new function from string code.
  Both enable **dynamic code execution**, which is **dangerous** if input comes from untrusted sources.

---

## Syntax / Structure

```js
// eval
eval("2 + 3"); // 5

// Function constructor
const add = new Function("a", "b", "return a + b");
add(2, 3); // 5
```

---

## Examples

```js
// 1) Eval in action
const code = "alert('Hacked!')";
eval(code); // 🚨 Dangerous

// 2) Function constructor
const sum = new Function("a", "b", "return a + b");
console.log(sum(1, 2)); // 3

// 3) Injection vulnerability
const userInput = "1); alert('pwnd');//";
const result = eval("calc(" + userInput + ")");
```

---

## Visualization

```
User Input → eval/Function → Executed as Code → Full control over runtime
```

---

## Gotchas ⚠️

1. **Arbitrary Code Execution**

   * Any string passed is executed → RCE (remote code execution).
   * Attackers can steal cookies, modify DOM, exfiltrate data.

2. **Scope Leakage** (`eval`)

   * Accesses and modifies local scope variables.

   ```js
   let x = 10;
   eval("x = 99"); // x now 99
   ```

3. **Function constructor scope**

   * Executes in global scope only (no closure access).
   * Safer than `eval`, but still unsafe for untrusted input.

4. **Performance Hit**

   * Prevents JS engine optimizations (hidden classes, JIT).
   * Slower execution path.

5. **Security bypass**

   * CSP (Content Security Policy) can block `eval`/`Function`.
   * But developers sometimes use `unsafe-eval`, weakening CSP.

6. **Indirect eval**

   ```js
   (0, eval)("2+2"); // runs in global scope
   ```

7. **JSON misuse**

   * ❌ `eval("("+jsonStr+")")`
   * ✅ `JSON.parse(jsonStr)`

---

## ✅ Pros & ❌ Cons

**✅ Pros (legit use-cases)**

* Quick REPL-like eval in controlled environments.
* Dynamic function generation in metaprogramming.
* Polyfills/shims (historically).

**❌ Cons**

* Huge security risk (code injection).
* Performance penalty.
* Harder to debug, maintain, and secure.
* Often violates CSP policies.

---

## Best Practices

* **Avoid `eval` and `Function`** — almost always unnecessary.
* Use **safe alternatives**:

  * Dynamic property access: `obj[prop]` instead of `eval("obj."+prop)`.
  * `JSON.parse()` instead of `eval()`.
  * Lookup tables / maps for dynamic logic.
* Enforce CSP without `unsafe-eval`.
* Use linters (`no-eval` ESLint rule).

---

## Mnemonic / Quick Recall

**“Eval = Evil.”**

* `eval()` → runs in local scope, worst risk.
* `new Function()` → runs in global scope, still unsafe.

---

👉 Want me to also make a **comparison card: `eval` vs `new Function` vs `setTimeout(string)`** — since interviewers love to ask about their differences?


Here’s your **Safe JSON Handling Cheat Card** — tight, practical, and security-minded.

---

# 🛡️ Safe JSON Handling — Cheat Card

**Concept:**
Handle JSON **safely and predictably**: parse/serialize without injections, validate shape, avoid prototype pollution, and cope with big/odd payloads.

---

## Syntax / Structure

```js
// Parse (throwing)
const obj = JSON.parse(text, reviver);

// Stringify (throws on cycles by default)
const str = JSON.stringify(value, replacer, space);
```

---

## Examples (secure patterns)

```js
// 1) Try/catch on untrusted input
let data;
try {
  data = JSON.parse(body);
} catch {
  // 400 Bad Request – invalid JSON
}

// 2) Reviver: convert ISO dates & block dangerous keys
const ISO = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}/;
const safeReviver = (k, v) => {
  if (k === "__proto__" || k === "constructor" || k === "prototype") return undefined;
  return (typeof v === "string" && ISO.test(v)) ? new Date(v) : v;
};
const payload = JSON.parse(body, safeReviver);

// 3) Replacer: strip secrets before logging
const safeReplacer = (k, v) => (k.toLowerCase().includes("password") ? "***" : v);
const logLine = JSON.stringify(payload, safeReplacer);

// 4) Stable output (sorted keys) for hashing/snapshots
const stableStr = JSON.stringify(payload, Object.keys(payload).sort());

// 5) Large JSON off main thread (browser)
const worker = new Worker("parser.js"); // do JSON.parse in worker

// 6) Avoid inline XSS when embedding JSON in HTML
// Prefer <script type="application/json"> with textContent, not innerHTML.
const script = document.createElement("script");
script.type = "application/json";
script.textContent = JSON.stringify(payload);
document.body.append(script);

// 7) Node/Fetch: verify Content-Type and size
const res = await fetch(url);
if (!res.headers.get("content-type")?.includes("application/json")) throw Error("Bad type");
const text = await res.text();
if (text.length > 2_000_000) throw Error("Too big");
const safe = JSON.parse(text);
```

---

## Visualization

```
Input JSON → validate (type, size) → JSON.parse (reviver) → schema check → safe merge/use
Output Obj → JSON.stringify (replacer) → log/emit
```

---

## Gotchas ⚠️ (and fixes)

1. **Prototype pollution via merging**

   * Risk: merging parsed objects can set `__proto__`/`constructor`/`prototype`.
   * Fix: block keys in **reviver** and/or use safe merge libs; never spread raw client JSON into configs.

2. **`eval("("+json+")")` ≠ JSON.parse**

   * Risk: code execution.
   * Fix: **Never** use `eval`; use `JSON.parse`.

3. **BigInt & special types**

   * JSON has no BigInt/Map/Set/Date/RegExp/functions.
   * Fix: encode as strings/arrays, or use custom (de)serialization with reviver/replacer. Convert back explicitly.

4. **Circular references on stringify**

   * `JSON.stringify` throws on cycles.
   * Fix: use a cycle-handling replacer or `structuredClone` (not for JSON I/O).

5. **Huge payloads freeze UI / DoS**

   * Fix (browser): parse in Web Worker; stream or chunk; set server limits.
   * Fix (server): limit body size, timeouts, and depth.

6. **CSP & inline JSON**

   * Embedding JSON inside `<script>` can break on U+2028/U+2029 or be XSS-adjacent if not escaped.
   * Fix: `textContent` + `JSON.stringify`; avoid `innerHTML`.

7. **Content-Type lies**

   * Some servers send JSON with wrong `Content-Type`.
   * Fix: check header **and** attempt safe parse; reject if unexpected.

8. **Schema drift & trust**

   * Parsing ≠ validation.
   * Fix: validate with **Zod/Joi/Ajv**; reject or default missing/extra fields.

9. **NaN/Infinity**

   * Not valid in JSON; `JSON.stringify({x: Infinity})` → `{}` for number? (actually `null` inside arrays, property omitted if replacer filters).
   * Fix: sanitize numbers pre-stringify.

10. **Unicode/BOM quirks**

* Leading BOM `\uFEFF` can break parse.
* Fix: strip BOM before `JSON.parse`.

---

## ✅ Pros & ❌ Cons

**✅ Pros**

* `JSON.parse/stringify` are fast, standard, secure by default (vs `eval`).
* Reviver/Replacer allow controlled (de)serialization.
* Works across environments.

**❌ Cons**

* No native schema/types; needs extra validation.
* No support for complex types (BigInt/Map/Set/Date by default).
* Blocking parse/stringify on huge payloads.

---

## Best Practices (crisp rules)

* **Always** `try/catch` `JSON.parse` on untrusted input.
* Enforce **limits**: `Content-Type`, size, depth, timeouts.
* Use a **reviver** to drop `__proto__/constructor/prototype` and coerce dates safely.
* **Validate** shape with Zod/Joi/Ajv immediately after parsing.
* Avoid inline JSON HTML injection; use `<script type="application/json">` with **textContent**.
* Redact secrets with a **replacer** before logging.
* Parse off-thread (Web Worker) or stream when payloads are large.
* Never use `eval` for JSON.
* Prefer **structuredClone** for deep copies, not JSON round-trips.

---

## Mnemonic / Quick Recall

**“Parse → Validate → Sanitize → Use; Stringify → Redact → Send.”**

---

Want a quick **Zod schema template** and a **safe merge helper** you can drop into Node/Express right now?


Here’s your **Input Validation Cheat Card** — tight, practical, and interview-ready.

---

# 🧰 Input Validation — Cheat Card

**Concept:**
Ensure incoming data is **expected, safe, and usable** by checking **type, shape, range, format, and intent** before using it. Validation ≠ sanitization (cleaning); you usually need both.

---

## Validation Layers (where to enforce)

* **Client/UI:** Fast feedback; never trust alone.
* **API Gateway / Edge:** Basic schema & size limits.
* **Server/App:** Authoritative schema + business rules.
* **DB layer:** Constraints (NOT NULL, CHECK, UNIQUE).
* **Background jobs/webhooks:** Re-validate external inputs.

---

## Approaches & Libraries

* **Schema validators:** Zod, Joi/Hapi, Yup, Ajv (JSON Schema), Superstruct.
* **Type-driven:** TypeScript + Zod (runtime) → `z.infer` for static types.
* **Regex/Custom:** For small formats; beware ReDoS.

---

## Core Checks (what to validate)

* **Presence & Type:** required fields, correct primitives.
* **Format:** email, URL, UUID, phone, regex patterns.
* **Range & Length:** numbers (min/max), strings (min/max), arrays (min/max items).
* **Enums / OneOf:** whitelist allowed values.
* **Cross-field rules:** start ≤ end, password ≠ oldPassword.
* **Business rules:** role permissions, quotas, state transitions.
* **Security:** ban keys `__proto__`, `constructor`, `prototype`; escape/sanitize where needed.
* **Files:** mime-type, extension, size, dimensions, content scanning.
* **Dates:** timezone awareness, future/past windows; parse safely (ISO-8601).

---

## Examples

### Zod (Node/TS)

```ts
import { z } from "zod";

const CreateUser = z.object({
  name: z.string().min(1).max(80).trim(),
  email: z.string().email(),
  age: z.number().int().min(13).max(120).optional(),
  role: z.enum(["user","admin"]),
  tags: z.array(z.string().min(1)).max(10).default([]),
}).strict(); // disallow unknown keys

type CreateUser = z.infer<typeof CreateUser>;

function handler(req, res) {
  const parsed = CreateUser.safeParse(req.body);
  if (!parsed.success) return res.status(400).json({ errors: parsed.error.format() });
  // use parsed.data safely
}
```

### Joi (Express)

```js
const Joi = require("joi");

const schema = Joi.object({
  q: Joi.string().max(100).required(),
  page: Joi.number().integer().min(1).default(1),
}).unknown(false);

app.get("/search", (req, res, next) => {
  const { error, value } = schema.validate(req.query, { abortEarly: false, stripUnknown: true });
  if (error) return res.status(400).json({ errors: error.details });
  req.query = value; next();
});
```

### Ajv (JSON Schema)

```js
const Ajv = require("ajv"); const ajv = new Ajv({ allErrors:true, removeAdditional:"failing" });
const schema = { type:"object", additionalProperties:false,
  properties:{ id:{type:"string", format:"uuid"} }, required:["id"] };
const validate = ajv.compile(schema);
if (!validate(req.body)) return res.status(400).json({ errors: validate.errors });
```

---

## Visualization

```
Request → (Content-Type check, size limits)
       → Parse JSON safely
       → Schema validate (type/shape/range/format)
       → Business rules
       → Sanitization (contextual)
       → Use
```

---

## Gotchas ⚠️ (and fixes)

1. **Trusting client-side only** → Always re-validate on server.
2. **Regex ReDoS** → Avoid catastrophic patterns; use timeouts/safe libs (RE2).
3. **Prototype pollution via merges** → Drop `__proto__`/`constructor`/`prototype`; use `.strict()` schemas; safe merge.
4. **Loose Content-Type** → Require `application/json`; reject others; limit body size.
5. **Coercion surprises** → Disable implicit casts unless intentional (e.g., `"123"` → 123).
6. **Missing normalization** → Trim strings, normalize Unicode, lowercase emails before validate/compare.
7. **Enum drift** → Centralize enums; sync with DB constraints.
8. **Date parsing** → Reliance on locale; prefer ISO 8601 + explicit TZ.
9. **Unknown fields** → Decide: reject (`additionalProperties:false`) or strip. Be consistent.
10. **File validation** → Check MIME by content (magic bytes), not just extension.
11. **Error leakage** → Don’t expose internal error stacks; return structured validation errors only.
12. **i18n & locale** → Formats (number, date) differ by locale; validate normalized forms.
13. **Partial updates (PATCH)** → Separate schemas for create vs update; make fields optional + refine.
14. **Webhook inputs** → Verify signatures (HMAC), timestamps, replay protection.

---

## ✅ Pros & ❌ Cons

**✅ Pros**

* Prevents logic & security bugs early.
* Documents API contract.
* Enables better DX with typed outputs.

**❌ Cons**

* Boilerplate if not centralized.
* Performance cost if over-validating hot paths (optimize).
* Schema drift if not versioned.

---

## Best Practices (crisp rules)

* **Validate everything at trust boundaries** (HTTP, queue, CLI args).
* **Fail fast** with precise, user-friendly error messages.
* **Use strict schemas** (no unknown props) unless you intentionally allow them.
* **Normalize → Validate → Sanitize** (in that order).
* **Share types**: derive TS types from runtime schemas (Zod/Ajv).
* **Rate-limit & size-limit** before parsing; set timeouts.
* **Centralize** validators as composable modules/middlewares.
* **Log** validation failures with safe redaction (no PII/Secrets).
* **Version** schemas with your API; include migration notes.

---

## Mnemonic / Quick Recall

**“T.R.U.S.T.”**

* **T**ype/shape check
* **R**ange/regex/required
* **U**nknowns rejected
* **S**anitize & normalize
* **T**hrottle & size-limit

---

Want a **ready-to-drop Express middleware kit** (Zod + helmeted JSON parser + size limits + error formatter) tailored to your Node stack?
