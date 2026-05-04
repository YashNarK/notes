- [MERN Interview Preparation](#mern-interview-preparation)
  - [JavaScript](#javascript)
    - [Currying and `fn.length`](#currying-and-fnlength)
    - [Objects, maps, and lookup structures](#objects-maps-and-lookup-structures)
    - [Sets and weak collections](#sets-and-weak-collections)
    - [`JSON.stringify`, replacers, and circular references](#jsonstringify-replacers-and-circular-references)
    - [Shallow copy, deep copy, and `deepClone`](#shallow-copy-deep-copy-and-deepclone)
    - [Type checks, coercion, and number quirks](#type-checks-coercion-and-number-quirks)
    - [Event loop: call stack, microtasks, and macrotasks](#event-loop-call-stack-microtasks-and-macrotasks)
    - [Classes, prototypes, private fields, and `new`](#classes-prototypes-private-fields-and-new)
    - [Iterators, iterables, and generators](#iterators-iterables-and-generators)
    - [`this`, arrow functions, and explicit binding](#this-arrow-functions-and-explicit-binding)
    - [Promises, async/await, and combinators](#promises-asyncawait-and-combinators)
    - [Debounce, memoization, flatten, and core array operations](#debounce-memoization-flatten-and-core-array-operations)
  - [TypeScript](#typescript)
    - [Interface vs type](#interface-vs-type)
    - [Generics, constraints, and reusable typed data structures](#generics-constraints-and-reusable-typed-data-structures)
    - [Mapped types, utility types, conditional types, and `infer`](#mapped-types-utility-types-conditional-types-and-infer)
    - [Practical React and backend typing patterns](#practical-react-and-backend-typing-patterns)
  - [React](#react)
    - [JSX, `createElement`, `createRoot`, and rendering basics](#jsx-createelement-createroot-and-rendering-basics)
    - [Hooks, re-renders, and memoization](#hooks-re-renders-and-memoization)
    - [Render vs commit, reconciliation, and Fiber](#render-vs-commit-reconciliation-and-fiber)
    - [Hydration, CSR/SSR/SSG/ISR/streaming, and RSC](#hydration-csrssrssgisrstreaming-and-rsc)
    - [State architecture: local, shared, global, and server state](#state-architecture-local-shared-global-and-server-state)
    - [Zustand mental model](#zustand-mental-model)
    - [Redux Toolkit, middleware, async flows, and ducks](#redux-toolkit-middleware-async-flows-and-ducks)
    - [Router, layouts, and protected routes](#router-layouts-and-protected-routes)
    - [Recursive components and nested UI trees](#recursive-components-and-nested-ui-trees)
    - [Microfrontends, Module Federation, Single-SPA, and extension-style hosts](#microfrontends-module-federation-single-spa-and-extension-style-hosts)
    - [Frontend performance](#frontend-performance)
    - [Error boundaries, Suspense, testing, and code quality](#error-boundaries-suspense-testing-and-code-quality)
  - [Node](#node)
    - [Browser vs Node event loops](#browser-vs-node-event-loops)
    - [`process.nextTick`, Promise microtasks, `setTimeout`, and `setImmediate`](#processnexttick-promise-microtasks-settimeout-and-setimmediate)
    - [Authentication, authorization, sessions, JWT, and OAuth](#authentication-authorization-sessions-jwt-and-oauth)
    - [Streams, buffers, logging, profiling, and worker threads](#streams-buffers-logging-profiling-and-worker-threads)
    - [Express middleware, request pipeline, and why Express exists](#express-middleware-request-pipeline-and-why-express-exists)
    - [Architecture patterns with Node and Express](#architecture-patterns-with-node-and-express)
    - [Testing and code quality](#testing-and-code-quality)
  - [Competitive programming](#competitive-programming)
    - [Group anagrams](#group-anagrams)
    - [Move zeroes](#move-zeroes)
    - [Two sum](#two-sum)
    - [Valid parentheses](#valid-parentheses)
    - [LRU cache](#lru-cache)
    - [LFU cache](#lfu-cache)
    - [Maximum subarray and Kadane's algorithm](#maximum-subarray-and-kadanes-algorithm)
    - [Binary search](#binary-search)
    - [Rotate array](#rotate-array)
    - [Longest substring without repeating characters](#longest-substring-without-repeating-characters)



# MERN Interview Preparation

Polished interview notes for JavaScript, TypeScript, React, Node, and common coding problems. The emphasis is on mental models, interview-safe definitions, practical code, pitfalls, and the follow-up questions that usually appear after the first answer.

## JavaScript

### Currying and `fn.length`

**Definition.** Currying converts a function with multiple parameters into a chain of functions that keep collecting arguments until there are enough to run the original function.

**Mental model.** Think of currying like filling a bucket. The bucket size is the function arity. Each call adds more values; once the bucket is full, the function runs.

**Key rules.**

- `fn.length` tells you how many declared parameters a function expects.
- The standard curry pattern checks `args.length >= fn.length`.
- `fn.apply(this, args)` preserves the current `this` when the original function finally executes.
- Interview gotcha: `fn.length` only counts parameters before the first default parameter and ignores rest parameters.

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) return fn.apply(this, args);
    return (...more) => curried(...args, ...more);
  };
}

const curriedAdd = curry((a, b, c) => a + b + c);

curriedAdd(1)(2)(3);    // 6
curriedAdd(1, 2)(3);    // 6
curriedAdd(1, 2, 3);    // 6
```

**Interview note.** A good one-line answer is: “Currying turns a function of many arguments into a sequence of functions that keep collecting arguments until the original function can execute.”

### Objects, maps, and lookup structures

**Definition.**

- `Object` is the default container for structured JSON-like data.
- `Map` is a dynamic key-value store where any value can be a key.

**Mental model.**

- `Object` = labeled box of named fields.
- `Map` = dictionary / locker system for lookups.

**Object rules worth remembering.**

- Dot notation is for known keys: `user.name`
- Bracket notation is for dynamic keys: `user[key]`
- Object keys are strings or symbols; numeric keys are converted to strings.
- `Object.keys`, `Object.values`, and `Object.entries` are everyday interview tools.

```js
const user = {
  name: "Narendran",
  age: 26,
  city: "Chennai",
};

const key = "name";

user.name;            // "Narendran"
user[key];            // "Narendran"
Object.keys(user);    // ["name", "age", "city"]
Object.values(user);  // ["Narendran", 26, "Chennai"]
Object.entries(user); // [["name", "Narendran"], ["age", 26], ["city", "Chennai"]]
```

**Useful object patterns.**

```js
const nextUser = { ...user, age: 27 }; // immutable update
const { age, ...withoutAge } = user;   // immutable removal

const apiUser = {};
const city = apiUser.address?.city ?? "Unknown";
```

**Map rules worth remembering.**

- Use `Map` when keys are dynamic, unknown ahead of time, or not strings.
- `Map` preserves insertion order.
- Core methods: `set`, `get`, `has`, `delete`, `clear`, `size`

```js
const scores = new Map([
  ["apple", 10],
  ["banana", 20],
]);

const objKey = { id: 1 };
scores.set(objKey, "User Data");

scores.get("apple"); // 10
scores.get(objKey);  // "User Data"
scores.size;         // 3
```

**Classic `Map` interview uses.**

```js
// Frequency counting
const arr = ["a", "b", "a", "c", "b", "a"];
const freq = new Map();

for (const item of arr) {
  freq.set(item, (freq.get(item) || 0) + 1);
}

// a -> 3, b -> 2, c -> 1

// Grouping
const people = [
  { name: "Alice", age: 20 },
  { name: "Bob", age: 20 },
  { name: "Charlie", age: 25 },
];

const groups = new Map();
for (const person of people) {
  if (!groups.has(person.age)) groups.set(person.age, []);
  groups.get(person.age).push(person);
}
```

**Quick decision rule.**

- Use `Object` for structured data, configs, DTOs, and JSON.
- Use `Map` for caches, frequency counters, object keys, and frequent add/remove operations.

### Sets and weak collections

**Definition.**

- `Set` stores unique values.
- `WeakSet` stores object references weakly.
- `WeakMap` maps object keys to values weakly.

**Mental model.**

- `Set` = unique list.
- `WeakSet` / `WeakMap` = object tracking that should not keep objects alive in memory.

**Set basics.**

```js
const set = new Set([1, 2, 2, 3]);
console.log([...set]); // [1, 2, 3]

set.add(4);
set.has(2);    // true
set.delete(1); // true
set.size;      // 3
```

**Common `Set` patterns.**

```js
const uniqueUsers = [...new Set(["A", "B", "A", "C"])];
// ["A", "B", "C"]

const a = new Set([1, 2]);
const b = new Set([2, 3]);

const union = new Set([...a, ...b]);                     // {1, 2, 3}
const intersection = new Set([...a].filter((x) => b.has(x))); // {2}
const difference = new Set([...a].filter((x) => !b.has(x)));  // {1}
```

**Reference semantics.**

```js
const refs = new Set();
refs.add({ a: 1 });
refs.add({ a: 1 });
console.log(refs.size); // 2
```

Two objects with the same shape are still different references.

**Weak collections.**

- Only objects can be keys/values.
- They are not iterable.
- No reliable `size`.
- Best use cases: visited-object tracking, private metadata, DOM bookkeeping, circular-reference detection.

```js
const visited = new WeakSet();

function traverse(value) {
  if (value === null || typeof value !== "object" || visited.has(value)) return;
  visited.add(value);

  for (const key in value) {
    traverse(value[key]);
  }
}
```

```js
const privateData = new WeakMap();

function setSecret(obj, value) {
  privateData.set(obj, value);
}

function getSecret(obj) {
  return privateData.get(obj);
}
```

**Interview note.** Strong references (`Map`, `Set`) keep data alive; weak references (`WeakMap`, `WeakSet`) allow garbage collection.

### `JSON.stringify`, replacers, and circular references

**Definition.** `JSON.stringify(value, replacer, space)` serializes a value to JSON text.

**Rules that matter.**

- Functions are skipped during JSON serialization.
- Circular references throw.
- The replacer can be:
  - a function `(key, value) => nextValue`
  - an array of keys to keep
- The replacer sees the root value first with an empty key: `""`.

```js
const obj = {
  name: "Narendran",
  greet() {
    return "Hello";
  },
};

JSON.stringify(obj); // '{"name":"Narendran"}'
```

**Replacer function.**

```js
const user = {
  name: "Narendran",
  age: 26,
  city: "Chennai",
};

JSON.stringify(user, (key, value) => {
  if (key === "age") return undefined;
  return value;
});
// '{"name":"Narendran","city":"Chennai"}'
```

**Replacer array.**

```js
JSON.stringify(user, ["name", "city"]);
// '{"name":"Narendran","city":"Chennai"}'
```

**Pretty printing.**

```js
JSON.stringify(user, null, 2);
```

**Circular references.**

```js
const circular = {};
circular.self = circular;

// JSON.stringify(circular);
// TypeError: Converting circular structure to JSON
```

**Safe stringify with `WeakMap`.**

```js
function safeStringify(value) {
  const seen = new WeakMap();

  return JSON.stringify(value, (key, current) => {
    if (typeof current === "object" && current !== null) {
      if (seen.has(current)) return "[Circular]";
      seen.set(current, true);
    }
    return current;
  });
}

safeStringify(circular); // '{"self":"[Circular]"}'
```

**Interview note.** The replacer is a filter/transform step that runs on every key-value pair during serialization.

### Shallow copy, deep copy, and `deepClone`

**Core rule.**

- Primitives are copied by value.
- Objects, arrays, functions, `Map`, and `Set` are copied by reference unless you clone them.

**Shallow copy.**

```js
const user = {
  name: "Narendran",
  address: { city: "Chennai" },
};

const copy = { ...user };
copy.address.city = "Mumbai";

console.log(user.address.city); // "Mumbai"
```

Spread copied the outer object, but both objects still share the same nested `address`.

**Deep copy.**

```js
const original = {
  name: "Narendran",
  address: { city: "Chennai" },
};

const deepCopy = structuredClone(original);
deepCopy.address.city = "Mumbai";

console.log(original.address.city); // "Chennai"
console.log(deepCopy.address.city); // "Mumbai"
```

**`structuredClone` is the modern default** for plain deep-copy needs. It handles objects, arrays, dates, maps, sets, and circular references better than the old JSON trick.

**Custom recursive `deepClone`.**

```js
function deepClone(value, seen = new WeakMap()) {
  if (value === null || typeof value !== "object") return value;
  if (seen.has(value)) return seen.get(value);

  if (value instanceof Date) return new Date(value);

  if (value instanceof Map) {
    const copy = new Map();
    seen.set(value, copy);
    for (const [k, v] of value) {
      copy.set(deepClone(k, seen), deepClone(v, seen));
    }
    return copy;
  }

  if (value instanceof Set) {
    const copy = new Set();
    seen.set(value, copy);
    for (const item of value) {
      copy.add(deepClone(item, seen));
    }
    return copy;
  }

  if (Array.isArray(value)) {
    const copy = [];
    seen.set(value, copy);
    for (const item of value) {
      copy.push(deepClone(item, seen));
    }
    return copy;
  }

  const copy = {};
  seen.set(value, copy);
  for (const key of Reflect.ownKeys(value)) {
    copy[key] = deepClone(value[key], seen);
  }
  return copy;
}
```

**Why interviewers dislike `JSON.parse(JSON.stringify(obj))`.**

- It drops `undefined` and functions.
- It converts `Date` to strings.
- It does not preserve `Map` / `Set`.
- It breaks on circular references.

**React pitfall.** Spreading nested state is still shallow:

```js
const state = { user: { name: "Narendran" } };
const nextState = { ...state };
nextState.user.name = "Alex";

console.log(state.user.name); // "Alex"
```

For nested React updates, copy each nested layer or use Immer / a reducer toolkit that handles immutable updates for you.

### Type checks, coercion, and number quirks

**Reliable checks.**

```js
const value = { a: 1 };

value !== null && typeof value === "object"; // true
Array.isArray([1, 2, 3]);                    // true
typeof (() => {});                           // "function"

Object.prototype.toString.call({});          // "[object Object]"
Object.prototype.toString.call([]);          // "[object Array]"
Object.prototype.toString.call(() => {});    // "[object Function]"
```

**Classic interview traps.**

```js
typeof null;           // "object"
typeof [1, 2, 3];      // "object"
typeof NaN;            // "number"
NaN === NaN;           // false
Number.isNaN(NaN);     // true
Object.is(NaN, NaN);   // true
```

**Coercion quirks.**

```js
true + true; // 2
"5" - 3;     // 2
"5" + 3;     // "53"
[] + [];     // ""
```

**Floating-point quirk.**

```js
0.1 + 0.2; // 0.30000000000000004
Number((0.1 + 0.2).toFixed(2)); // 0.3
```

**Mental shortcut.**

- Use `typeof` for functions.
- Use `Array.isArray` for arrays.
- Use `value !== null && typeof value === "object"` for objects.
- Use `Object.prototype.toString.call(value)` when you need library-level certainty.

### Event loop: call stack, microtasks, and macrotasks

**Mental model.** JavaScript is a single chef in a kitchen:

- call stack = the chef
- queues = waiting orders
- event loop = the manager deciding what comes next

**The key browser rule.**

1. Run synchronous code on the call stack.
2. When the stack is empty, drain all microtasks.
3. Then run one macrotask.
4. Browser may render between turns.

```js
console.log("1 — sync");

setTimeout(() => console.log("4 — macrotask"), 0);
Promise.resolve().then(() => console.log("3 — microtask"));

console.log("2 — sync");

// Output:
// 1 — sync
// 2 — sync
// 3 — microtask
// 4 — macrotask
```

**Microtasks.**

- `Promise.then`
- `queueMicrotask`
- `MutationObserver`

**Macrotasks.**

- `setTimeout`
- `setInterval`
- DOM events
- I/O callbacks

**Interview rule to say out loud.** “Synchronous code runs first, then the engine drains microtasks, then it processes the next macrotask.”

### Classes, prototypes, private fields, and `new`

**Definition.** A JavaScript class is syntactic sugar over prototype-based inheritance.

**Mental model.**

- class = blueprint
- `new` = factory machine
- prototype = shared toolbox

```js
class Car {
  #miles = 0;

  constructor(brand) {
    this.brand = brand;
  }

  drive() {
    this.#miles += 1;
    console.log(`${this.brand} is driving`);
  }

  getMiles() {
    return this.#miles;
  }

  static isCar(value) {
    return value instanceof Car;
  }
}

const c1 = new Car("BMW");
const c2 = new Car("Audi");

console.log(typeof Car);            // "function"
console.log(c1.drive === c2.drive); // true
```

**What `new` does internally.**

1. Create an empty object.
2. Link it to `Constructor.prototype`.
3. Call the constructor with `this` set to that object.
4. Return the object.

**Why prototypes matter.** Class methods live on `Car.prototype`, not inside each instance. That saves memory.

**Static methods.** They belong to the class, not to instances:

```js
Car.isCar(c1); // true
```

**Private fields.** `#field` is true language-level privacy; it is not just a naming convention.

**Important pitfalls.**

- Class methods are not auto-bound.
- Arrow class fields are auto-bound, but each instance gets its own function copy.
- Classes are in the temporal dead zone and are not usable before declaration.
- Class methods are non-enumerable.

### Iterators, iterables, and generators

**Definitions.**

- Iterator = object with `next()`
- Iterable = object with `[Symbol.iterator]()`
- Generator = function that creates an iterator and can pause/resume execution

```js
const arr = [10, 20, 30];
const iterator = arr[Symbol.iterator]();

iterator.next(); // { value: 10, done: false }
iterator.next(); // { value: 20, done: false }
iterator.next(); // { value: 30, done: false }
iterator.next(); // { value: undefined, done: true }
```

**Features that secretly use iterators.**

- `for...of`
- spread syntax
- array destructuring

```js
const nums = [1, 2, 3];

for (const n of nums) console.log(n);
console.log([...nums]); // [1, 2, 3]

const [a, b] = nums;
console.log(a, b); // 1 2
```

**Custom iterable.**

```js
const counter = {
  start: 1,
  end: 3,
  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;

    return {
      next() {
        if (current <= end) return { value: current++, done: false };
        return { value: undefined, done: true };
      },
    };
  },
};

console.log([...counter]); // [1, 2, 3]
```

**Generators.**

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

console.log([...numbers()]); // [1, 2, 3]
```

`yield` pauses execution, and `next()` resumes it.

**Two-way generator communication.**

```js
function* greet() {
  const name = yield "What is your name?";
  yield `Hello ${name}`;
}

const gen = greet();
console.log(gen.next().value);            // "What is your name?"
console.log(gen.next("Narendran").value); // "Hello Narendran"
```

**Interview rule.** “Iterable gives you an iterator; iterator gives you `{ value, done }`.”

### `this`, arrow functions, and explicit binding

**Golden rule.**

- Regular function: `this` is decided by the call-site.
- Arrow function: `this` is captured from the surrounding scope when the arrow is created.

```js
function foo() {
  console.log(this);
}

foo(); // undefined in strict mode, global object in sloppy mode
```

**Object method with regular function.**

```js
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);

    const inner = () => console.log(this.name);
    inner();
  },
};

obj.greet();
// Alice
// Alice
```

**Detached method pitfall.**

```js
const detached = obj.greet;
detached(); // undefined in strict mode
```

**Why arrow methods are dangerous in object literals.**

```js
const bad = {
  name: "Alice",
  greet: () => console.log(this.name),
};

bad.greet(); // usually undefined
```

That arrow does not get `bad` as `this`; it inherits from the outer scope.

**Explicit binding tools.**

```js
function say(age) {
  console.log(`${this.name} is ${age}`);
}

const person = { name: "Alice" };

say.call(person, 25);      // Alice is 25
say.apply(person, [25]);   // Alice is 25

const bound = say.bind(person);
bound(25);                 // Alice is 25
```

**Interview shortcut.**

- Regular function = caller owns `this`
- Arrow function = creator owns `this`
- `bind` = permanently fix `this`

### Promises, async/await, and combinators

**Promise mental model.** A promise is a value that will settle in the future:

- fulfilled
- rejected
- pending

**Chain flow and the `catch` return-value trap.**

```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then(() => {
    throw new Error("stop");
  })
  .catch(() => 42)
  .then((x) => console.log(x)); // 42
```

If you use braces without returning, the next step receives `undefined`:

```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then(() => {
    throw new Error("stop");
  })
  .catch(() => {
    42;
  })
  .then((x) => console.log(x)); // undefined
```

**Async/await equivalent.**

```js
async function run() {
  try {
    let x = await Promise.resolve(1);
    x = x + 1;
    throw new Error("stop");
  } catch {
    return 42;
  }
}

run().then(console.log); // 42
```

**Combinators to memorize.**

| API | Resolves when | Rejects when | Typical use case |
| --- | --- | --- | --- |
| `Promise.all` | all succeed | first rejection | parallel dependent results |
| `Promise.allSettled` | all finish | never rejects because of member failure | gather all outcomes |
| `Promise.race` | first promise settles | if first settled promise rejects | timeout races |
| `Promise.any` | first promise fulfills | all fail (`AggregateError`) | first successful mirror / fallback |

```js
const [user, posts, comments] = await Promise.all([
  fetchUser(),
  fetchPosts(),
  fetchComments(),
]);
```

```js
const results = await Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("error"),
]);
// [
//   { status: "fulfilled", value: 1 },
//   { status: "rejected", reason: "error" }
// ]
```

```js
const winner = await Promise.race([
  new Promise((resolve) => setTimeout(() => resolve("fast"), 100)),
  new Promise((resolve) => setTimeout(() => resolve("slow"), 1000)),
]);
// "fast"
```

```js
const firstSuccess = await Promise.any([
  Promise.reject("fail"),
  Promise.resolve("success"),
]);
// "success"
```

**Interview staple: implement `Promise.all`.**

```js
function myPromiseAll(values) {
  return new Promise((resolve, reject) => {
    if (values.length === 0) return resolve([]);

    const results = new Array(values.length);
    let completed = 0;

    values.forEach((value, index) => {
      Promise.resolve(value)
        .then((result) => {
          results[index] = result;
          completed += 1;
          if (completed === values.length) resolve(results);
        })
        .catch(reject);
    });
  });
}
```

**Why that implementation matters.**

- preserve input order
- support non-promise values with `Promise.resolve`
- reject immediately on first failure
- resolve `[]` immediately for empty input

### Debounce, memoization, flatten, and core array operations

**Debounce.** Run a function only after calls stop for a given delay.

```js
function debounce(fn, delay) {
  let timer;

  return function (...args) {
    const context = this;
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, delay);
  };
}

const search = debounce((query) => {
  console.log("Searching:", query);
}, 300);

search("a");
search("ab");
search("abc");
// After 300ms: "Searching: abc"
```

**Why `apply` is used.** If the debounced function is used as an object method, this preserves the original `this`.

**Memoization.** Cache the result of a pure computation.

```js
function memoize(fn) {
  const primitiveCache = new Map();
  const objectCache = new WeakMap();

  return function (arg) {
    const isObject =
      arg !== null && (typeof arg === "object" || typeof arg === "function");
    const cache = isObject ? objectCache : primitiveCache;

    if (cache.has(arg)) return cache.get(arg);

    const result = fn(arg);
    cache.set(arg, result);
    return result;
  };
}

const getArea = memoize((shape) => shape.w * shape.h);
const rect = { w: 5, h: 10 };

getArea(rect); // 50
getArea(rect); // 50 (cached)
```

**Why `WeakMap` is useful here.** Object keys can be garbage collected when nothing else references them.

**Flattening nested arrays.**

```js
function flatten(arr, depth = Infinity) {
  const result = [];

  for (const item of arr) {
    if (Array.isArray(item) && depth > 0) {
      result.push(...flatten(item, depth - 1));
    } else {
      result.push(item);
    }
  }

  return result;
}

const arr = [1, [2, [3, [4, [5]]]]];

flatten(arr);    // [1, 2, 3, 4, 5]
flatten(arr, 1); // [1, 2, [3, [4, [5]]]]
flatten(arr, 2); // [1, 2, 3, [4, [5]]]
```

**Iterative flatten follow-up.**

```js
function flattenIterative(arr) {
  const stack = [...arr];
  const result = [];

  while (stack.length) {
    const item = stack.pop();
    if (Array.isArray(item)) stack.push(...item);
    else result.push(item);
  }

  return result.reverse();
}
```

**Core array operations.**

```js
const letters = ["A", "B", "C"];

letters.push("D");    // ["A", "B", "C", "D"]
letters.pop();        // returns "D"
letters.unshift("X"); // ["X", "A", "B", "C"]
letters.shift();      // returns "X"
```

**Performance rule.**

- `push` / `pop` are usually `O(1)`
- `shift` / `unshift` are usually `O(n)` because elements must be reindexed

**Mutating array methods.**

- `push`, `pop`, `shift`, `unshift`
- `splice`, `sort`, `reverse`
- `fill`, `copyWithin`

**Non-mutating array methods.**

- `map`, `filter`, `slice`, `concat`
- `flat`, `flatMap`, `reduce`
- `toSorted`, `toReversed`, `toSpliced`

**Classic interview trap.**

```js
const nums = [1, 2, 3, 4];

nums.slice(1, 3); // [2, 3], original unchanged
nums.splice(1, 2); // removes [2, 3], original becomes [1, 4]
```

**Quick memory trick.**

- `slice` = copy
- `splice` = surgery

## TypeScript

### Interface vs type

**Common ground.** Both describe shapes of data.

```ts
interface User {
  name: string;
  age: number;
}

type UserAlias = {
  name: string;
  age: number;
};
```

**Where `interface` shines.**

- object contracts
- class `implements`
- declaration merging
- public library surface area

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = {
  name: "Narendran",
  age: 26,
};
```

Interfaces merged into one shape.

**Where `type` shines.**

- unions
- primitives
- tuples
- function signatures
- intersections

```ts
type Status = "loading" | "success" | "error";
type ID = string | number;
type Point = [number, number];
type Add = (a: number, b: number) => number;
type AdminUser = { role: string } & { name: string };
```

**Composition rules.**

```ts
interface Animal {
  name: string;
}

type WithBreed = {
  breed: string;
};

interface Dog extends Animal, WithBreed {}
```

**Interview heuristic.** Use `interface` for object-like public contracts, and `type` for unions, utility composition, and everything beyond plain object shapes.

### Generics, constraints, and reusable typed data structures

**Mental model.** Generics are type variables: placeholders for types.

```ts
function first<T>(arr: T[]): T | undefined {
  return arr[0];
}

const a = first([1, 2, 3]);       // number | undefined
const b = first(["a", "b", "c"]); // string | undefined
```

**Multiple generic parameters.**

```ts
function pair<A, B>(a: A, b: B): [A, B] {
  return [a, b];
}

const result = pair(1, "hello"); // [number, string]
```

**Constraint example to memorize.**

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "Narendran", age: 26 };

getProperty(user, "name"); // string
// getProperty(user, "salary"); // compile-time error
```

What that means:

- `T` = object type
- `keyof T` = valid keys of that object
- `K extends keyof T` = key must exist on the object
- `T[K]` = property type at that key

**Generic backend pattern.**

```ts
interface Repository<T extends { id: number }> {
  findById(id: number): Promise<T>;
  findAll(): Promise<T[]>;
  save(entity: T): Promise<T>;
  delete(id: number): Promise<void>;
}
```

**Generic class interview favorite.**

```ts
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items.at(-1);
  }

  get size(): number {
    return this.items.length;
  }
}

const s = new Stack<string>();
s.push("hello");
// s.push(10); // compile-time error
```

**Default generics.**

```ts
type ApiResponse<T = unknown> = {
  data: T;
  error?: string;
};
```

### Mapped types, utility types, conditional types, and `infer`

**Mapped types.** Loop over keys and transform an existing object type.

```ts
type Optional<T> = {
  [K in keyof T]?: T[K];
};

type ReadonlyUser = Readonly<{
  name: string;
  age: number;
}>;
```

**Conditional types.** Type-level `if/else`.

```ts
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

In conditional types, `extends` means “is assignable to?”, not class inheritance.

**`NonNullable` is the must-know example.**

```ts
type NonNullableValue<T> = T extends null | undefined ? never : T;

type Name = NonNullableValue<string | null>; // string
```

Why it works:

- conditional types distribute over unions
- `never` disappears inside unions

So:

- `NonNullableValue<string | null>`
- becomes `NonNullableValue<string> | NonNullableValue<null>`
- becomes `string | never`
- becomes `string`

**`infer`.** Extract a hidden type from another type.

```ts
type ElementType<T> = T extends (infer U)[] ? U : T;
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

type Item = ElementType<number[]>;              // number
type Resolved = UnwrapPromise<Promise<string>>; // string

function greet() {
  return "hello";
}

type Greeting = MyReturnType<typeof greet>; // string
```

**Deep recursive utility.**

```ts
type DeepReadonly<T> =
  T extends object
    ? { readonly [K in keyof T]: DeepReadonly<T[K]> }
    : T;
```

**Interview note.**

- mapped types transform keys
- conditional types choose between types
- `infer` extracts types

### Practical React and backend typing patterns

**Typed React props.**

```ts
interface User {
  id: number;
  name: string;
}

type UserCardProps = {
  user: User;
  onSelect?: (id: User["id"]) => void;
};

function UserCard({ user, onSelect }: UserCardProps) {
  return <button onClick={() => onSelect?.(user.id)}>{user.name}</button>;
}
```

**Generic React list component.**

```ts
type ListProps<T> = {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
};

function List<T>({ items, renderItem }: ListProps<T>) {
  return <>{items.map(renderItem)}</>;
}
```

**Backend DTO / response typing.**

```ts
type ApiResponse<T = unknown> = {
  data: T;
  error?: string;
};

async function getUser(id: number): Promise<ApiResponse<User>> {
  const data = await fetch(`/api/users/${id}`).then((r) => r.json());
  return { data };
}
```

**Repository pattern with constraints.**

```ts
interface Entity {
  id: number;
}

class InMemoryRepository<T extends Entity> implements Repository<T> {
  constructor(private readonly items: T[] = []) {}

  async findById(id: number): Promise<T> {
    const item = this.items.find((x) => x.id === id);
    if (!item) throw new Error("Not found");
    return item;
  }

  async findAll(): Promise<T[]> {
    return this.items;
  }

  async save(entity: T): Promise<T> {
    this.items.push(entity);
    return entity;
  }

  async delete(id: number): Promise<void> {
    const index = this.items.findIndex((x) => x.id === id);
    if (index >= 0) this.items.splice(index, 1);
  }
}
```

**Practical interview line.** “In React, TypeScript mainly protects props, events, hooks, and state shape. In the backend, it protects DTOs, repository/service boundaries, and API contracts.”

## React

### JSX, `createElement`, `createRoot`, and rendering basics

**Definition.** JSX is syntax sugar. React turns it into `React.createElement(...)` calls that produce React element objects.

```jsx
const element = <h1>Hello</h1>;

// roughly becomes:
const elementObject = React.createElement("h1", null, "Hello");
```

**A React element is just a plain object.**

```js
{
  type: "button",
  props: {
    onClick: () => alert("hi"),
    children: "Click me"
  }
}
```

**Modern app entry point.**

```jsx
import { createRoot } from "react-dom/client";
import App from "./App";

const root = createRoot(document.getElementById("root"));
root.render(<App />);
```

**Mental model.**

- JSX -> `createElement`
- `createElement` -> React element objects
- React element tree -> render work -> DOM updates

**Component rule.** A component is a function (or class) that returns a React element tree.

```jsx
function App() {
  return <h1>Hello</h1>;
}
```

### Hooks, re-renders, and memoization

**The rendering rules interviewers expect.**

1. State change -> component re-renders.
2. Parent render -> children render by default.
3. Prop change -> component re-renders.
4. Context change -> consumers re-render.
5. `React.memo` may skip a render if props are shallowly equal.
6. New object/function props can defeat memoization.
7. Changing a `key` remounts the component.
8. `useRef().current` changes do not trigger a render.

**Hook quick definitions.**

- `useState` = local reactive state
- `useEffect` = side effects after commit
- `useRef` = mutable container / DOM reference without re-render

```jsx
function Counter() {
  const [count, setCount] = React.useState(0);
  const inputRef = React.useRef(null);

  React.useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return (
    <>
      <button onClick={() => setCount((c) => c + 1)}>{count}</button>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current?.focus()}>Focus input</button>
    </>
  );
}
```

**`React.memo` vs `useMemo` vs `useCallback`.**

- `React.memo` memoizes a component render based on props.
- `useMemo` memoizes a computed value.
- `useCallback` memoizes a function reference.

```jsx
const Child = React.memo(function Child({ value, onClick }) {
  console.log("Child render");
  return <button onClick={onClick}>{value}</button>;
});

function Parent({ users }) {
  const [count, setCount] = React.useState(0);

  const sortedUsers = React.useMemo(
    () => [...users].sort((a, b) => a.age - b.age),
    [users]
  );

  const handleClick = React.useCallback(() => {
    setCount((c) => c + 1);
  }, []);

  return (
    <>
      <p>{count}</p>
      <Child value={sortedUsers.length} onClick={handleClick} />
    </>
  );
}
```

**Memoization pitfall.** `React.memo` only does a shallow prop comparison. New object literals and inline callbacks usually look “changed” even if their contents are the same.

### Render vs commit, reconciliation, and Fiber

**React update pipeline.**

1. **Render phase**: run components, build the next React tree.
2. **Reconciliation**: compare old tree vs new tree.
3. **Commit phase**: update the real DOM and run effects/refs.

**Important distinction.**

- Render phase = calculate changes
- Commit phase = apply changes

```jsx
function App() {
  const [count, setCount] = React.useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

If `count` goes from `0` to `1`:

- render phase builds `<button>1</button>`
- reconciliation notices only text changed
- commit phase updates the text node

**Reconciliation rules.**

- same element type -> update in place
- different element type -> replace
- `key` helps React match list items correctly

**Fiber mental model.** Fiber is React’s reconciliation engine. It turns rendering into small units of work so React can pause, resume, prioritize, or restart rendering.

A simplified Fiber node stores things like:

- `type`
- `props`
- `memoizedState`
- `child`
- `sibling`
- `return`
- `alternate`

**Why Fiber matters.**

- render phase can be interruptible
- commit phase stays synchronous
- enables concurrent rendering, transitions, Suspense, streaming SSR, and selective hydration

**Interview answer.** “Fiber is the internal linked-tree work model that lets React break rendering into interruptible units instead of blocking the main thread with one huge recursive pass.”

### Hydration, CSR/SSR/SSG/ISR/streaming, and RSC

**Hydration.** Hydration attaches React behavior to HTML that already exists in the DOM, usually after server rendering.

```jsx
import { hydrateRoot } from "react-dom/client";
import App from "./App";

hydrateRoot(document.getElementById("root"), <App />);
```

**Mental model.**

- server-rendered HTML = body
- hydration = nervous system

**Hydration mismatch causes.**

- `Math.random()`
- `Date.now()`
- browser-only APIs during server render
- locale/timezone differences

**Rendering strategy comparison.**

| Strategy | Where HTML is produced | When | Best fit |
| --- | --- | --- | --- |
| CSR | browser | after JS loads | dashboards, internal apps |
| SSR | server | every request | SEO pages, commerce, marketing |
| SSG | build server | build time | docs, blogs, landing pages |
| ISR | build + cache | build time + revalidate | large catalogs, news |
| Streaming | server in chunks | progressively | large pages with partial loading |

**ISR example.**

```js
export const revalidate = 60;
```

**Streaming + Suspense.**

```jsx
<Suspense fallback={<LoadingReviews />}>
  <Reviews />
</Suspense>
```

React can send ready parts of the page first and stream the slow part later.

**React Server Components (RSC).**

- run on the server
- can fetch data directly
- do not ship their own JS to the client

```jsx
export default async function Products() {
  const products = await fetch("https://api.example.com/products").then((r) => r.json());

  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Client components.**

- required for `useState`, `useEffect`, event handlers, browser APIs

```jsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

**Lazy loading.**

```jsx
const Chart = React.lazy(() => import("./Chart"));

function Dashboard() {
  return (
    <Suspense fallback={<p>Loading chart...</p>}>
      <Chart />
    </Suspense>
  );
}
```

**Interview summary.** “Hydration makes server HTML interactive, CSR/SSR/SSG/ISR describe when HTML is created, streaming sends HTML progressively, and RSC moves non-interactive UI work back to the server.”

### State architecture: local, shared, global, and server state

**The biggest architecture mistake.** Not all state belongs in one tool.

**Useful categories.**

| State type | Typical tool | Examples |
| --- | --- | --- |
| local UI state | `useState` | modal open/close, input value |
| complex local state | `useReducer` | checkout wizard, form workflow |
| shared UI state | Context | theme, auth shell, locale |
| global client state | Zustand / Redux | cart, UI preferences, drafts |
| server state | TanStack Query / SWR | products, orders, notifications |

**Server state vs client state.**

- Server state is fetched, cached, refetched, and can become stale.
- Client state is created by the UI and owned by the client.

Even if you use fetched data in `useEffect`, it is still server state:

```jsx
const { data: products } = useQuery({
  queryKey: ["products"],
  queryFn: fetchProducts,
});

React.useEffect(() => {
  doSomething(products);
}, [products]);
```

`products` is still server state because the backend owns it.

**Good interview line.** “Server state is data I fetch; client state is data I create.”

### Zustand mental model

**Mental model.** Zustand is a global object plus a `set()` function plus subscribers.

```js
import { create } from "zustand";

export const useStore = create((set, get) => ({
  count: 0,
  cart: [],
  user: null,

  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  resetCount: () => set({ count: 0 }),

  addToCart: (item) =>
    set((state) => ({ cart: [...state.cart, item] })),

  removeFromCart: (id) =>
    set((state) => ({
      cart: state.cart.filter((item) => item.id !== id),
    })),

  setUser: (user) => set({ user }),
  logout: () => set({ user: null }),
}));
```

**Why `set()` feels familiar.** Conceptually it works like a global `setState`:

```js
let state = { count: 0 };

function set(updater) {
  const next = typeof updater === "function" ? updater(state) : updater;
  state = { ...state, ...next };
  notifySubscribers();
}
```

**Usage in React.**

```js
function Counter() {
  const count = useStore((state) => state.count);
  const increment = useStore((state) => state.increment);

  return <button onClick={increment}>{count}</button>;
}
```

Only components selecting `count` re-render when `count` changes.

**Outside React.**

```js
useStore.setState({ count: 10 });

const state = useStore.getState();
console.log(state.count);

const unsubscribe = useStore.subscribe((state) => {
  console.log("count changed:", state.count);
});

unsubscribe();
```

**Choosing Zustand vs server-state tools.**

- Put cart, modals, drafts, tabs, or filters in Zustand.
- Put fetched products, users, orders, or notifications in TanStack Query.

### Redux Toolkit, middleware, async flows, and ducks

**When Redux beats Zustand.**

- stricter architecture
- time-travel debugging
- predictable reducer-only state updates
- richer middleware ecosystem
- easier large-team coordination

**Core Redux flow.**

- component dispatches action
- reducer computes next state
- store updates
- subscribed UI re-renders

**Redux Toolkit essentials.**

```ts
import { configureStore, createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment(state) {
      state.value++;
    },
    decrement(state) {
      state.value--;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;

export const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});
```

**Why `state.value++` is legal here.** Redux Toolkit uses Immer internally, so “mutating” syntax becomes a safe immutable update.

**Async flow with `createAsyncThunk`.**

```ts
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

type User = { id: number; name: string };

export const fetchUsers = createAsyncThunk("users/fetch", async () => {
  const res = await fetch("/api/users");
  return (await res.json()) as User[];
});

const usersSlice = createSlice({
  name: "users",
  initialState: {
    items: [] as User[],
    status: "idle" as "idle" | "loading" | "succeeded" | "failed",
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.status = "loading";
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      });
  },
});
```

**Middleware you should know by name.**

- **Thunk**: async logic in action creators
- **Logger**: log actions and state transitions
- **Saga**: generator-based orchestration for complex side effects
- **Observable**: RxJS-style action streams

**Ducks pattern.**

- One feature = one slice/module
- Keep reducer, actions, selectors, and feature types together

Example file ownership:

- `features/counter/counterSlice.ts`
- `features/users/usersSlice.ts`

**Redux Toolkit action names are automatically namespaced.**

If two slices both define `increment`, the action types become:

- `counter/increment`
- `cart/increment`

So the action names do not collide.

**Interview rule.** “Use Zustand for lightweight client-global state, but pick Redux when you want strict architecture, middleware, and deep debugging in a large codebase.”

### Router, layouts, and protected routes

**Mental model.** URL -> router -> route match -> component tree.

**React Router building blocks.**

- `BrowserRouter`
- `Routes`
- `Route`
- `Outlet`
- `Navigate`

**Layout + protected route pattern.**

```jsx
import {
  BrowserRouter,
  Navigate,
  Outlet,
  Route,
  Routes,
} from "react-router-dom";

function AppLayout() {
  return (
    <>
      <Header />
      <Outlet />
      <Footer />
    </>
  );
}

function ProtectedRoute() {
  const user = useAuthStore((state) => state.user);
  return user ? <Outlet /> : <Navigate to="/login" replace />;
}

function AppRouter() {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<AppLayout />}>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />

          <Route element={<ProtectedRoute />}>
            <Route path="/dashboard" element={<Dashboard />} />
            <Route path="/settings" element={<Settings />} />
          </Route>
        </Route>

        <Route path="/login" element={<Login />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Route organization tips.**

- group routes by feature
- use layout routes to avoid repeating shells
- use nested routes when the UI is nested
- protect routes at both UI and API layers

**Security reminder.** Protected routes improve UX, but real authorization must still happen on the backend.

### Recursive components and nested UI trees

**Use case.** Trees, folders, nested comments, menus, org charts.

**Mental model.**

- base case = no children -> stop
- recursive case = render the same component for each child

```jsx
const folderTree = {
  name: "root",
  children: [
    { name: "src", children: [{ name: "index.js" }, { name: "App.js" }] },
    { name: "public", children: [{ name: "index.html" }] },
  ],
};

function Folder({ node }) {
  return (
    <div style={{ marginLeft: 20 }}>
      <strong>{node.name}</strong>
      {node.children?.map((child) => (
        <Folder key={child.name} node={child} />
      ))}
    </div>
  );
}
```

**Performance-friendly variation.**

```jsx
function Folder({ node }) {
  const [open, setOpen] = React.useState(false);

  return (
    <div style={{ marginLeft: 20 }}>
      <strong onClick={() => setOpen((v) => !v)}>{node.name}</strong>
      {open &&
        node.children?.map((child) => (
          <Folder key={child.name} node={child} />
        ))}
    </div>
  );
}
```

**Pitfalls.**

- always define a base case
- always use stable keys
- be careful with deeply nested state and unnecessary re-renders

### Microfrontends, Module Federation, Single-SPA, and extension-style hosts

**Definition.** Microfrontends split one large frontend into smaller independently deployable frontend applications.

**Mental model.**

- shell/container app = host
- microfrontends = feature teams’ mini-apps

**Common structure.**

- shell handles routing, layout, auth, shared navigation
- each microfrontend owns one business domain
- each microfrontend can be built and deployed independently

**Integration styles.**

- client-side composition
- server-side composition
- build-time integration

**Common tooling.**

- **Module Federation**: dynamically load remote bundles
- **Single-SPA**: orchestrate multiple frontend apps/frameworks
- **Web Components**: framework-agnostic integration layer

**Communication strategies.**

- custom browser events
- shared state/store
- URL-driven state

**Challenges.**

- versioning shared dependencies
- cross-app routing
- state sharing
- bundle duplication
- debugging distributed UI

**Practical note.** Extension-style platforms such as Azure DevOps (ADO) extensions often look microfrontend-like: isolated feature bundles plug into a host shell, follow host contracts, and ship independently.

### Frontend performance

**Think in three layers.**

1. **Render less** -> memoization, splitting state, minimizing context churn
2. **Load less** -> lazy loading, route splitting, virtualization
3. **Ship less** -> tree shaking, minification, compression, caching

**Render less.**

- `React.memo`, `useMemo`, `useCallback`
- keep state local when possible
- avoid creating fresh object/function props unless needed
- virtualize long lists

**Load less.**

- `React.lazy`
- route-based code splitting
- lazy images/assets
- `Suspense`

**Ship less.**

- tree shaking with ES modules
- vendor splitting
- `contenthash` filenames for caching
- gzip / brotli compression

**Examples.**

```jsx
const Dashboard = React.lazy(() => import("./Dashboard"));
```

```jsx
<img loading="lazy" src="/hero.png" alt="Hero" />
```

```js
output: {
  filename: "[name].[contenthash].js";
}
```

**Measure, don’t guess.**

- React DevTools Profiler
- Lighthouse / Web Vitals
- bundle analyzer
- browser performance panel

**Interview note.** Tree shaking removes unused exports; code splitting changes when code is loaded; memoization changes whether expensive work reruns.

### Error boundaries, Suspense, testing, and code quality

**Error boundaries.** They catch rendering/lifecycle errors in their child tree and show fallback UI instead of crashing the entire app.

```jsx
import React from "react";

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error("ErrorBoundary caught an error", error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

**What they catch.**

- render errors
- lifecycle errors
- constructor errors in descendants

**What they do not catch.**

- event handlers
- async callbacks like `setTimeout`
- server-side rendering errors
- errors inside the boundary itself

**Placement rule.**

- global boundary = app-level safety net
- local boundary = better UX for risky sections

**Suspense.**

```jsx
<ErrorBoundary>
  <Suspense fallback={<Loading />}>
    <LazyComponent />
  </Suspense>
</ErrorBoundary>
```

- `Suspense` handles waiting / loading
- `ErrorBoundary` handles failure

**Testing stack interviewers expect.**

- Jest or Vitest for test runner/assertions
- React Testing Library for component behavior
- mock network boundaries instead of component internals

```jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Counter from "./Counter";

test("increments count", async () => {
  render(<Counter />);

  await userEvent.click(screen.getByRole("button", { name: /increment/i }));

  expect(screen.getByText("1")).toBeInTheDocument();
});
```

**Code quality checklist.**

- prefer small pure components
- keep server state out of Redux/Zustand unless necessary
- use TypeScript for props and state shapes
- lint consistently
- profile before optimizing

## Node

### Browser vs Node event loops

**The environments are different.**

- Browser event loop cares about rendering and user interaction.
- Node event loop is built on libuv and cares about server-side I/O phases.

**Browser mental model.**

- sync code
- microtasks
- macrotasks
- render
- idle work

**Node mental model.**

- timers
- pending callbacks
- idle/prepare
- poll
- check
- close callbacks

**Important correction.** Render queue and idle queue are browser concepts. Node does not have browser paint/render phases.

**Good interview line.** “The browser event loop optimizes responsiveness and painting; the Node event loop optimizes async I/O with libuv phases.”

### `process.nextTick`, Promise microtasks, `setTimeout`, and `setImmediate`

**Priority rule in Node.**

1. current synchronous work
2. `process.nextTick`
3. promise microtasks
4. timers / poll / check phases depending on context

```js
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
Promise.resolve().then(() => console.log("promise"));
process.nextTick(() => console.log("nextTick"));
```

**Guaranteed part of the output.**

```txt
nextTick
promise
```

**About `setTimeout(0)` vs `setImmediate()`.**

- At top level, order can vary.
- Inside an I/O callback, `setImmediate()` usually runs before `setTimeout(0)`.

**Why `process.nextTick` is special.**

- It runs before other microtasks.
- Overusing it can starve I/O because Node keeps draining that queue first.

**Interview note.** If asked to compare them, say:

- `process.nextTick` = highest-priority post-sync callback in Node
- `Promise.then` = standard microtask
- `setTimeout(0)` = timer phase
- `setImmediate()` = check phase, often best for “run after current I/O”

### Authentication, authorization, sessions, JWT, and OAuth

**AuthN vs AuthZ.**

- authentication = who are you?
- authorization = what are you allowed to do?

**Mental model.**

- authentication = showing ID at the entrance
- authorization = guard deciding if you can enter the VIP room

**Sessions, cookies, and tokens.**

| Concept | What it is | Usual storage | Main trade-off |
| --- | --- | --- | --- |
| session | server remembers the user | server store + cookie with session id | easy revocation, server memory cost |
| cookie | browser storage sent automatically with requests | browser | carrier, not auth strategy by itself |
| token/JWT | client carries signed proof | header / cookie / storage | stateless, harder revocation |

**JWT basics.**

- structure: `header.payload.signature`
- server signs token
- client sends it with each request

```http
GET /dashboard
Authorization: Bearer <jwt-token>
```

**Session flow.**

1. user logs in
2. server creates session
3. server sends session id in cookie
4. browser automatically sends cookie later
5. server looks up session data

**JWT flow.**

1. user logs in
2. server signs token
3. client stores token
4. client sends token on later requests
5. server verifies signature

**OAuth.**

- delegated login / authorization
- “Login with Google / GitHub / Facebook”
- your app trusts a third-party identity provider

**Interview shortcut.**

- JWT = “my app says this user is Alice”
- OAuth = “Google says this user is Alice, and I trust Google”

**Backend authorization reminder.** Do not trust frontend-only role checks. Always enforce roles/permissions in the backend.

**Express auth middleware example.**

```js
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) {
    return res.status(401).send("Login first");
  }

  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    return res.status(403).send("Invalid token");
  }
}
```

### Streams, buffers, logging, profiling, and worker threads

**Definitions.**

- `Buffer` = fixed-size memory chunk for binary data
- `Stream` = sequence of chunks processed over time

**Why streams matter.** They let you process large files or network data without loading everything into memory at once.

**Stream types.**

- readable
- writable
- duplex
- transform

```js
const fs = require("fs");

const readStream = fs.createReadStream("largeFile.txt", { encoding: "utf8" });
const writeStream = fs.createWriteStream("copy.txt");

readStream.pipe(writeStream);
```

**Logging.**

- Winston = flexible transports
- Pino = high-performance structured logging

```js
const pino = require("pino");
const logger = pino();

logger.info({ userId: 123 }, "User logged in");
logger.error({ err }, "Unhandled error");
```

**Profiling and performance.**

- `node --inspect app.js`
- Chrome DevTools profiler
- Clinic.js for CPU/heap analysis
- avoid blocking the event loop
- prefer streams for large payloads

**CPU-heavy work.**

- use Worker Threads for CPU-bound tasks
- use Child Processes for separate process isolation

```js
const { Worker } = require("worker_threads");

const worker = new Worker(
  `
    const { parentPort } = require("worker_threads");
    parentPort.postMessage("Hello from worker");
  `,
  { eval: true }
);

worker.on("message", console.log);
```

**Good interview line.** “Node is single-threaded for JS execution, but it can offload I/O through libuv and CPU-heavy work through workers or child processes.”

### Express middleware, request pipeline, and why Express exists

**Why Express exists.** Raw Node HTTP is low-level. Express adds routing, middleware, better request/response helpers, and a huge ecosystem.

**Mental model.**

`Request -> middleware -> router -> controller -> async work -> response`

**Core middleware signature.**

```js
function middleware(req, res, next) {
  next();
}
```

**Types of middleware.**

- application-level
- router-level
- built-in (`express.json`, `express.static`)
- third-party (`cors`, `helmet`, `morgan`, `cookie-parser`)
- error-handling (`err, req, res, next`)

```js
const express = require("express");
const cors = require("cors");
const helmet = require("helmet");
const morgan = require("morgan");

const app = express();

app.use(express.json());
app.use(cors());
app.use(helmet());
app.use(morgan("dev"));

app.get("/health", (req, res) => {
  res.send({ ok: true });
});

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send({ error: err.message });
});
```

**Order matters.** Middleware runs in registration order.

**Async handler pattern.**

```js
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

**Why use Express.**

- fast API setup
- middleware pipeline
- clean routing
- easy modularization
- massive community ecosystem

**Interview answer.** “Express is a lightweight Node web framework that sits on top of the HTTP module and makes it practical to build APIs through middleware, routing, and a consistent request/response model.”

### Architecture patterns with Node and Express

**Patterns Node/Express fits well.**

- monolith
- layered / n-tier app
- client-server backend
- SOA
- microservices
- serverless wrappers
- event-driven services
- BFF (backend for frontend)

**Quick mental models.**

- **Monolith** = one building with everything inside
- **Layered** = floors in one building
- **Microservices** = many independent buildings
- **Serverless** = on-demand workers
- **Event-driven** = broadcast events, subscribers react
- **BFF** = one backend tailored to one frontend client

**Why Node/Express often fits.**

- I/O-heavy workloads
- API gateways
- lightweight services
- real-time apps
- JS across frontend + backend

**Where Express is less ideal.**

- extremely CPU-heavy services without offloading
- ultra-high-performance cases where Fastify or lower-level tuning matters more

**Interview note.** Express is not “only for microservices.” It fits monoliths, layered APIs, BFFs, event-driven HTTP edges, and serverless wrappers too.

### Testing and code quality

**Node testing pyramid.**

- unit tests for pure business logic
- integration tests for routes + middleware + database boundaries
- end-to-end or contract tests for full flows

**Common tools.**

- Jest / Vitest
- Supertest for HTTP assertions
- ESLint
- TypeScript strict mode
- logging + metrics + tracing for production quality

```js
import request from "supertest";
import app from "./app";

test("GET /health returns ok", async () => {
  const res = await request(app).get("/health");

  expect(res.status).toBe(200);
  expect(res.body).toEqual({ ok: true });
});
```

**Good backend quality habits.**

- central error handling
- schema validation at API edges
- structured logs
- health checks
- profiling before optimizing
- avoid sync filesystem/database calls in request paths

## Competitive programming

### Group anagrams

**Pattern.** Hashing / grouping with a canonical key.

**Key idea.** All anagrams collapse to the same signature.

- easy signature: sorted characters
- faster signature: character frequency

```js
function groupAnagrams(strs) {
  const map = new Map();

  for (const word of strs) {
    const key = word.split("").sort().join("");
    if (!map.has(key)) map.set(key, []);
    map.get(key).push(word);
  }

  return [...map.values()];
}
```

**Example.**

```js
groupAnagrams(["eat", "tea", "tan", "ate", "nat", "bat"]);
// [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

**Complexity.**

- sorting version: `O(n * k log k)`
- frequency-key version: `O(n * k)`

**Interview notes.**

- Mention the sorted solution first.
- Then mention the 26-count optimization.
- `map.values()` returns an iterator, not an array, so use `Array.from(...)` or `[...map.values()]`.

### Move zeroes

**Pattern.** Two pointers / in-place compaction.

**Goal.** Move all zeroes to the end while keeping the relative order of non-zero values.

```js
function moveZeroes(nums) {
  let insertPos = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== 0) {
      [nums[i], nums[insertPos]] = [nums[insertPos], nums[i]];
      insertPos++;
    }
  }

  return nums;
}

moveZeroes([0, 1, 0, 3, 12]); // [1, 3, 12, 0, 0]
```

**Complexity.**

- time: `O(n)`
- space: `O(1)`

**Pitfall.** Removing zeroes with `splice` inside a forward loop is usually `O(n^2)` and easy to get wrong because indices shift.

### Two sum

**Pattern.** Hash map for complement lookup.

**Idea.** While scanning the array, ask whether `target - nums[i]` has already been seen.

```js
function twoSum(nums, target) {
  const map = new Map();

  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) {
      return [map.get(complement), i];
    }
    map.set(nums[i], i);
  }

  return [];
}

twoSum([2, 7, 11, 15], 9); // [0, 1]
```

**Complexity.**

- time: `O(n)`
- space: `O(n)`

**Follow-up.** If the array is already sorted, two pointers are often preferred.

### Valid parentheses

**Pattern.** Stack.

**Idea.** Push openings, and when you see a closing bracket, it must match the most recent opening bracket.

```js
function isValid(s) {
  const stack = [];
  const pairs = {
    ")": "(",
    "]": "[",
    "}": "{",
  };

  for (const ch of s) {
    if (pairs[ch]) {
      if (stack.pop() !== pairs[ch]) return false;
    } else {
      stack.push(ch);
    }
  }

  return stack.length === 0;
}

isValid("()[]{}"); // true
isValid("([)]");   // false
```

**Complexity.**

- time: `O(n)`
- space: `O(n)`

**Interview note.** Using a mapping object is cleaner and more scalable than writing separate `if` checks for each bracket type.

### LRU cache

**Pattern.** Hash map + recency tracking.

**Concept.** LRU = least recently used. On access, move the item to “most recent.” On overflow, evict the least recent item.

**JavaScript shortcut.** `Map` preserves insertion order, so JS lets you write a compact LRU without manually coding a doubly linked list.

```js
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.map = new Map();
  }

  get(key) {
    if (!this.map.has(key)) return -1;

    const value = this.map.get(key);
    this.map.delete(key);
    this.map.set(key, value);

    return value;
  }

  put(key, value) {
    if (this.map.has(key)) this.map.delete(key);
    this.map.set(key, value);

    if (this.map.size > this.capacity) {
      const lruKey = this.map.keys().next().value;
      this.map.delete(lruKey);
    }
  }
}
```

**Mental model.**

- map start = least recently used
- map end = most recently used

**Complexity in JavaScript.**

- average `get`: `O(1)`
- average `put`: `O(1)`

**Interview note.** In language-agnostic interviews, the canonical answer is usually “HashMap + Doubly Linked List.”

### LFU cache

**Pattern.** Frequency tracking + eviction policy.

**Concept.** LFU = least frequently used. If frequencies tie, evict the least recently used among those tied keys.

**Data structures for the optimal design.**

- `key -> value`
- `key -> frequency`
- `frequency -> ordered keys`
- `minFreq`

That optimal design supports `O(1)` average `get`/`put`, but it is heavy to whiteboard under time pressure.

**Simplified interview-friendly version.**

```js
class LFUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.values = new Map();
    this.freq = new Map();
  }

  get(key) {
    if (!this.values.has(key)) return -1;

    this.freq.set(key, (this.freq.get(key) ?? 0) + 1);
    return this.values.get(key);
  }

  put(key, value) {
    if (this.capacity === 0) return;

    if (!this.values.has(key) && this.values.size >= this.capacity) {
      let lfuKey;
      let min = Infinity;

      for (const [k, f] of this.freq) {
        if (f < min) {
          min = f;
          lfuKey = k;
        }
      }

      this.values.delete(lfuKey);
      this.freq.delete(lfuKey);
    }

    this.values.set(key, value);
    this.freq.set(key, (this.freq.get(key) ?? 0) + 1);
  }
}
```

**Complexity of the simplified version.**

- `get`: `O(1)`
- `put`: `O(n)` because eviction scans frequencies

**Interview note.** If asked for the optimal solution, explain the three-map structure and `minFreq`, even if you code the simpler version first.

### Maximum subarray and Kadane's algorithm

**Pattern.** Dynamic programming / running best.

**Key idea.** A negative running sum only hurts future subarrays, so drop it and restart.

```js
function maxSubArray(nums) {
  let best = nums[0];
  let sum = nums[0];

  for (let i = 1; i < nums.length; i++) {
    sum = Math.max(nums[i], sum + nums[i]);
    best = Math.max(best, sum);
  }

  return best;
}

maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4]); // 6
```

The best subarray there is `[4, -1, 2, 1]`.

**Complexity.**

- time: `O(n)`
- space: `O(1)`

**Mental shortcut.** “Carry positive momentum; reset negative momentum.”

### Binary search

**Pattern.** Divide and conquer on a sorted range.

**Requirement.** The array must already be sorted.

```js
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }

  return -1;
}

binarySearch([1, 3, 5, 7, 9], 7); // 3
```

**Complexity.**

- time: `O(log n)`
- space: `O(1)`

**Memory trick.**

- target bigger -> move left pointer right
- target smaller -> move right pointer left

### Rotate array

**Pattern.** Array slicing or in-place reversal.

**Simple right-rotation solution.**

```js
function rotate(nums, k) {
  const n = nums.length;
  const steps = k % n;
  const cut = n - steps;
  return [...nums.slice(cut), ...nums.slice(0, cut)];
}

rotate([1, 2, 3, 4, 5, 6, 7], 3);
// [5, 6, 7, 1, 2, 3, 4]
```

**Why `cut = n - steps` works.**

```txt
[1, 2, 3, 4 | 5, 6, 7]
             ^ cut here
```

Move the last `k` elements to the front.

**Complexity.**

- time: `O(n)`
- space: `O(n)`

**Follow-up.** In-place rotation uses the reverse trick:

1. reverse whole array
2. reverse first `k`
3. reverse the rest

### Longest substring without repeating characters

**Pattern.** Sliding window.

**Idea.** Expand the right pointer. If a character repeats inside the current window, move the left pointer just after the previous occurrence.

```js
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map();
  let left = 0;
  let best = 0;

  for (let right = 0; right < s.length; right++) {
    const ch = s[right];

    if (lastSeen.has(ch) && lastSeen.get(ch) >= left) {
      left = lastSeen.get(ch) + 1;
    }

    lastSeen.set(ch, right);
    best = Math.max(best, right - left + 1);
  }

  return best;
}

lengthOfLongestSubstring("abcabcbb"); // 3
lengthOfLongestSubstring("bbbbb");    // 1
lengthOfLongestSubstring("pwwkew");   // 3
```

**Complexity.**

- time: `O(n)`
- space: `O(min(n, charset))`

**Sliding-window interview line.** “Expand while the window is valid; shrink when the constraint breaks.”
