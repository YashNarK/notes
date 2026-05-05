# TypeScript

## Table of Contents

- [Prerequisites](#prerequisites)
- [1. Introduction to TypeScript](#1-introduction-to-typescript)
- [2. Setting Up TypeScript](#2-setting-up-typescript)
- [3. Basic Types](#3-basic-types)
- [4. Functions](#4-functions)
- [5. Interfaces](#5-interfaces)
- [6. Type Aliases](#6-type-aliases)
- [7. Enums](#7-enums)
- [8. Generics](#8-generics)
- [9. Classes](#9-classes)
- [10. Type Narrowing & Type Guards](#10-type-narrowing--type-guards)
- [11. Utility Types](#11-utility-types)
- [12. Advanced Types](#12-advanced-types)
- [13. Modules & Declaration Files](#13-modules--declaration-files)
- [14. Decorators](#14-decorators)
- [15. TypeScript with React (TSX)](#15-typescript-with-react-tsx)
- [16. Error Handling in TypeScript](#16-error-handling-in-typescript)
- [17. tsconfig Deep Dive](#17-tsconfig-deep-dive)
- [18. Best Practices & Common Pitfalls](#18-best-practices--common-pitfalls)

---

## Prerequisites

TypeScript is a superset of JavaScript. Strong JS fundamentals are required before learning TS.

- **[JavaScript Notes](./JavaScript.md)**

---

## 1. Introduction to TypeScript

### What is TypeScript?

TypeScript is a **statically typed superset of JavaScript** developed and maintained by Microsoft. It compiles down to plain JavaScript and runs anywhere JS runs — browsers, Node.js, Deno, Bun, etc.

```
TypeScript = JavaScript + Static Types + Modern Language Features
```

All valid JavaScript is valid TypeScript. You can adopt TypeScript gradually.

### TypeScript vs JavaScript

| Feature | JavaScript | TypeScript |
|---|---|---|
| Typing | Dynamic (runtime) | Static (compile-time) |
| Error detection | At runtime | At compile time |
| IDE support | Basic | Excellent (IntelliSense, autocomplete) |
| Interfaces | ❌ | ✅ |
| Generics | ❌ | ✅ |
| Enums | ❌ | ✅ |
| Access modifiers | ❌ | ✅ |
| Decorators | Stage 3 proposal | ✅ supported |
| Compilation step | Not needed | Required (`tsc`) |
| Learning curve | Low | Medium |

### Why TypeScript?

1. **Catch bugs at compile time** — not in production at 2am
2. **Self-documenting code** — types serve as inline documentation
3. **Better IDE experience** — autocomplete, refactoring, go-to-definition
4. **Safer refactoring** — the compiler tells you exactly what breaks
5. **Team scalability** — contracts between modules are explicit and enforced
6. **Industry standard** — React, Angular, Vue 3, NestJS, Next.js all use TypeScript

### How TypeScript Works

```
.ts / .tsx  →  tsc (TypeScript Compiler)  →  .js  →  browser / Node.js
```

TypeScript performs **type erasure** — all type annotations are stripped at compile time. Zero runtime overhead from types.

### Type Inference

TypeScript infers types automatically — you don't need to annotate everything:

```typescript
let name = "Alice";       // inferred: string
let age = 30;             // inferred: number
let active = true;        // inferred: boolean
let items = [1, 2, 3];   // inferred: number[]
const pi = 3.14;          // inferred: 3.14 (literal type)
```

---

## 2. Setting Up TypeScript

### Installation

```bash
# Global install (for CLI use)
npm install -g typescript

# Project-local (recommended)
npm install --save-dev typescript

# Check version
tsc --version
```

### Initialize tsconfig.json

```bash
tsc --init
```

### Key tsconfig.json Options

```json
{
  "compilerOptions": {
    // --- Compilation Target ---
    "target": "ES2020",               // JS version to output
    "lib": ["ES2020", "DOM"],         // Type definitions to include

    // --- Module System ---
    "module": "ESNext",               // Output module format
    "moduleResolution": "bundler",    // How imports are resolved (use "bundler" with Vite)

    // --- Directories ---
    "outDir": "./dist",               // Where compiled JS goes
    "rootDir": "./src",               // Where source TS files are

    // --- Strictness ---
    "strict": true,                   // Enable all strict checks (highly recommended)
    "noUnusedLocals": true,           // Error on unused local variables
    "noUnusedParameters": true,       // Error on unused parameters
    "noImplicitReturns": true,        // Error when function paths don't return

    // --- Interop ---
    "esModuleInterop": true,          // Allow default import from CJS modules
    "skipLibCheck": true,             // Skip type-checking .d.ts files (speeds up build)
    "forceConsistentCasingInFileNames": true,

    // --- Path Aliases ---
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"]
    },

    // --- Output ---
    "declaration": true,              // Emit .d.ts files
    "sourceMap": true                 // Emit source maps for debugging
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Running TypeScript

```bash
# Compile once
tsc

# Watch mode (recompile on change)
tsc --watch

# Run directly with ts-node (no separate compile step)
npx ts-node src/index.ts

# Run directly with tsx (faster, ESM-friendly)
npx tsx src/index.ts
```

---

## 3. Basic Types

### Primitives

```typescript
let username: string = "Alice";
let age: number = 25;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;
let bigNum: bigint = 9007199254740991n;
let sym: symbol = Symbol("uniqueId");
```

### Special Types

```typescript
// any — opt out of type checking entirely (avoid when possible)
let data: any = "hello";
data = 42;       // OK
data = [];       // OK — no type safety at all

// unknown — safe alternative to any; must narrow before using
let input: unknown = getUserInput();
if (typeof input === "string") {
  console.log(input.toUpperCase()); // OK after narrowing
}

// never — values that never occur; unreachable code
function throwError(msg: string): never {
  throw new Error(msg);
}

// void — function returns nothing meaningful
function log(msg: string): void {
  console.log(msg);
}

// object — any non-primitive (rarely useful; prefer specific shapes)
let obj: object = { a: 1 };
```

### Arrays

```typescript
let nums: number[] = [1, 2, 3];
let strs: Array<string> = ["a", "b"];       // generic syntax
let mixed: (string | number)[] = [1, "a"];  // union array
let matrix: number[][] = [[1, 2], [3, 4]];  // nested
```

### Tuples

Fixed-length arrays with known types at each position:

```typescript
let point: [number, number] = [10, 20];
let entry: [string, number] = ["Alice", 30];

// Named tuples (TS 4.0+) — improves readability
type Range = [start: number, end: number];
let r: Range = [0, 100];

// Optional tuple elements
type Config = [host: string, port?: number];
let c1: Config = ["localhost"];
let c2: Config = ["localhost", 8080];

// Rest elements in tuples
type StringsAndNumber = [...string[], number];
```

### Type Assertions

```typescript
// as keyword (use in TSX files — preferred)
const input = document.getElementById("name") as HTMLInputElement;
console.log(input.value);

// Angle bracket syntax (NOT usable in .tsx files)
const input2 = <HTMLInputElement>document.getElementById("name");

// Double assertion — escape hatch (use sparingly)
const x = someValue as unknown as TargetType;
```

### Literal Types

```typescript
type Direction = "north" | "south" | "east" | "west";
type StatusCode = 200 | 201 | 400 | 404 | 500;

let dir: Direction = "north";
// dir = "diagonal"; // Error

// as const — freeze values to their literal types
const ROLES = ["admin", "user", "guest"] as const;
type Role = typeof ROLES[number]; // "admin" | "user" | "guest"

const CONFIG = {
  host: "localhost",
  port: 3000,
} as const;
// CONFIG.port is 3000 (literal), not number
```

---

## 4. Functions

### Parameter and Return Types

```typescript
function add(a: number, b: number): number {
  return a + b;
}

const greet = (name: string): string => `Hello, ${name}`;
```

### Optional, Default & Rest Parameters

```typescript
// Optional parameter (must come after required)
function greet(name: string, greeting?: string): string {
  return `${greeting ?? "Hello"}, ${name}`;
}

// Default parameter
function createUser(name: string, role: string = "user") {
  return { name, role };
}

// Rest parameters
function sum(...nums: number[]): number {
  return nums.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3, 4); // 10
```

### Function Types & Callbacks

```typescript
// Type alias for a function signature
type Comparator<T> = (a: T, b: T) => number;
type Predicate<T> = (item: T) => boolean;
type Transform<T, U> = (value: T) => U;

// Higher-order function with typed callback
function filter<T>(arr: T[], predicate: Predicate<T>): T[] {
  return arr.filter(predicate);
}

const evens = filter([1, 2, 3, 4], (n) => n % 2 === 0);
```

### Function Overloads

Define multiple call signatures for the same function:

```typescript
function format(value: string): string;
function format(value: number, decimals: number): string;
function format(value: string | number, decimals?: number): string {
  if (typeof value === "string") return value.trim();
  return value.toFixed(decimals ?? 2);
}

format("  hello  ");   // "hello"
format(3.14159, 2);    // "3.14"
```

### `void` vs `never`

```typescript
// void: function completes normally but returns no meaningful value
function logToConsole(msg: string): void {
  console.log(msg);
}

// never: function NEVER returns — throws or loops forever
function fail(reason: string): never {
  throw new Error(reason);
}

function infiniteLoop(): never {
  while (true) {}
}
```

---

## 5. Interfaces

### Basic Interface

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

const alice: User = { id: 1, name: "Alice", email: "alice@example.com" };
```

### Optional, Readonly & Index Signatures

```typescript
interface Product {
  readonly id: number;       // cannot be changed after creation
  name: string;
  price: number;
  description?: string;      // optional
  tags?: string[];
}

// Index signature: object with dynamic string keys
interface StringMap {
  [key: string]: string;
}

const headers: StringMap = {
  "Content-Type": "application/json",
  "Authorization": "Bearer token",
};
```

### Extending Interfaces

```typescript
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  breed: string;
}

// Multiple inheritance
interface ServiceDog extends Dog, Trained {
  certificationId: string;
}
```

### Function & Constructor Signatures

```typescript
interface Formatter {
  (input: string): string;
}

interface Constructor<T> {
  new (arg: string): T;
}
```

### Declaration Merging

Interfaces with the same name are automatically merged — useful for augmenting third-party types:

```typescript
interface Window {
  myAnalytics: { track: (event: string) => void };
}

// Now window.myAnalytics is typed everywhere
```

### Interface vs Type Alias

| Feature | `interface` | `type` |
|---|---|---|
| Object shapes | ✅ | ✅ |
| Primitives/unions/tuples | ❌ | ✅ |
| Extending | `extends` keyword | `&` intersection |
| Declaration merging | ✅ | ❌ |
| `implements` in class | ✅ | ✅ (structural) |
| Recursive types | ✅ | ✅ |
| Computed property names | ❌ | ✅ |
| **Use for** | Object shapes, public APIs | Unions, computed types, primitives |

---

## 6. Type Aliases

### Defining Type Aliases

```typescript
type ID = string | number;
type Coordinates = { x: number; y: number };
type Callback = () => void;
type AsyncOperation<T> = () => Promise<T>;
```

### Union Types (`|`)

```typescript
type Status = "active" | "inactive" | "suspended";
type Input = string | number | boolean;
type Nullable<T> = T | null;
type Optional<T> = T | null | undefined;

function render(content: string | number) {
  console.log(String(content));
}
```

### Intersection Types (`&`)

Combine multiple types — result must satisfy ALL of them:

```typescript
type Named = { name: string };
type Timestamped = { createdAt: Date; updatedAt: Date };
type AuditedEntity = Named & Timestamped & { id: number };

const record: AuditedEntity = {
  id: 1,
  name: "Alice",
  createdAt: new Date(),
  updatedAt: new Date(),
};
```

### Template Literal Types

```typescript
type Direction = "top" | "bottom" | "left" | "right";
type Margin = `margin-${Direction}`;
// "margin-top" | "margin-bottom" | "margin-left" | "margin-right"

type EventHandler<T extends string> = `on${Capitalize<T>}`;
type ClickHandler = EventHandler<"click">;   // "onClick"
type ChangeHandler = EventHandler<"change">; // "onChange"

// String manipulation types
type Upper = Uppercase<"hello">;        // "HELLO"
type Lower = Lowercase<"WORLD">;        // "world"
type Cap = Capitalize<"hello">;         // "Hello"
type Uncap = Uncapitalize<"Hello">;     // "hello"
```

### Recursive Types

```typescript
type JSONValue =
  | string
  | number
  | boolean
  | null
  | JSONValue[]
  | { [key: string]: JSONValue };

type TreeNode<T> = {
  value: T;
  children?: TreeNode<T>[];
};
```

---

## 7. Enums

### Numeric Enums

```typescript
enum Direction {
  Up = 1,
  Down,   // 2
  Left,   // 3
  Right,  // 4
}

console.log(Direction.Up);     // 1
console.log(Direction[1]);     // "Up" — reverse mapping (numeric only)
```

### String Enums

```typescript
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING",
}

console.log(Status.Active); // "ACTIVE"
// No reverse mapping for string enums
```

### `const` Enums (zero runtime overhead)

```typescript
const enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE",
}

const c = Color.Red; // compiled to: const c = "RED";
// The Color object doesn't exist at runtime
```

### When to Use Enums vs Union Types

```typescript
// Enum — prefer when you need runtime iteration or reverse mapping
enum Permission { Read = 1, Write = 2, Admin = 4 }
const allPermissions = Object.values(Permission);

// Union literal — prefer for simple, compile-time-only variants
type Role = "admin" | "user" | "guest";
// Simpler, no runtime object, tree-shakeable
```

---

## 8. Generics

### Why Generics?

Without generics, you either lose type safety (using `any`) or duplicate code for every type:

```typescript
// Bad — loses type info
function identity(value: any): any { return value; }

// Good — T is captured at call site and preserved
function identity<T>(value: T): T { return value; }

const s = identity("hello");  // T inferred as string, returns string
const n = identity(42);       // T inferred as number, returns number
```

### Generic Functions

```typescript
function first<T>(arr: T[]): T | undefined {
  return arr[0];
}

function last<T>(arr: T[]): T | undefined {
  return arr[arr.length - 1];
}

function zip<A, B>(a: A[], b: B[]): [A, B][] {
  return a.map((item, i) => [item, b[i]]);
}

const pairs = zip([1, 2, 3], ["a", "b", "c"]);
// [number, string][]
```

### Generic Interfaces

```typescript
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
  timestamp: string;
}

interface Paginated<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
  hasNextPage: boolean;
}

// Compose
type PaginatedUsersResponse = ApiResponse<Paginated<User>>;
```

### Generic Classes

```typescript
class Repository<T extends { id: number }> {
  private store = new Map<number, T>();

  add(item: T): void {
    this.store.set(item.id, item);
  }

  get(id: number): T | undefined {
    return this.store.get(id);
  }

  getAll(): T[] {
    return Array.from(this.store.values());
  }

  remove(id: number): boolean {
    return this.store.delete(id);
  }
}

const userRepo = new Repository<User>();
userRepo.add({ id: 1, name: "Alice", email: "a@a.com" });
```

### Generic Constraints

```typescript
// Constrain T to have a specific property
function logLength<T extends { length: number }>(value: T): T {
  console.log(`Length: ${value.length}`);
  return value;
}

logLength("hello");    // OK
logLength([1, 2, 3]);  // OK
// logLength(42);      // Error — number has no length

// Constrain T to keys of another type
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const name = getProperty({ name: "Alice", age: 30 }, "name"); // string
// getProperty({ name: "Alice" }, "email"); // Error
```

### Default Type Parameters

```typescript
interface Box<T = string> {
  value: T;
  label?: string;
}

const strBox: Box = { value: "hello" };     // T = string (default)
const numBox: Box<number> = { value: 42 };  // T = number
```

### `keyof` and `typeof` Operators

```typescript
// keyof — produces union of object type's keys
type UserKeys = keyof User;         // "id" | "name" | "email"
type AnyKey = keyof any;            // string | number | symbol

// typeof — get the type of a runtime value
const config = { host: "localhost", port: 3000 };
type Config = typeof config;        // { host: string; port: number }

// Combined usage
function pluck<T, K extends keyof T>(items: T[], key: K): T[K][] {
  return items.map((item) => item[key]);
}

const names = pluck(users, "name"); // string[]
```

---

## 9. Classes

### Basic Class with Access Modifiers

```typescript
class BankAccount {
  public owner: string;           // accessible everywhere (default)
  private _balance: number;       // only within this class
  protected accountType: string;  // this class + subclasses
  readonly id: string;            // set once, never changed

  constructor(owner: string, initialBalance: number) {
    this.owner = owner;
    this._balance = initialBalance;
    this.accountType = "checking";
    this.id = crypto.randomUUID();
  }

  deposit(amount: number): void {
    if (amount <= 0) throw new Error("Amount must be positive");
    this._balance += amount;
  }

  get balance(): number {
    return this._balance;
  }
}
```

### Constructor Shorthand (Parameter Properties)

```typescript
// TypeScript creates & assigns properties automatically
class User {
  constructor(
    public readonly id: number,
    public name: string,
    private email: string,
    protected role: string = "user"
  ) {}

  getEmail(): string { return this.email; }
}

const u = new User(1, "Alice", "alice@example.com");
```

### Inheritance

```typescript
class Animal {
  constructor(public name: string) {}

  speak(): string {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name: string, public breed: string) {
    super(name); // must call super before accessing this
  }

  speak(): string {            // override
    return `${this.name} barks`;
  }

  fetch(): string {
    return `${this.name} fetches the ball`;
  }
}

const dog = new Dog("Rex", "Labrador");
console.log(dog.speak()); // "Rex barks"
```

### Abstract Classes

```typescript
abstract class Shape {
  abstract area(): number;       // subclass MUST implement
  abstract perimeter(): number;

  describe(): string {            // concrete — shared implementation
    return `Area: ${this.area().toFixed(2)}, Perimeter: ${this.perimeter().toFixed(2)}`;
  }
}

class Circle extends Shape {
  constructor(public radius: number) { super(); }
  area() { return Math.PI * this.radius ** 2; }
  perimeter() { return 2 * Math.PI * this.radius; }
}

class Rectangle extends Shape {
  constructor(public width: number, public height: number) { super(); }
  area() { return this.width * this.height; }
  perimeter() { return 2 * (this.width + this.height); }
}

// const s = new Shape(); // Error — cannot instantiate abstract class
```

### Static Members

```typescript
class IdGenerator {
  private static counter = 0;

  static generate(): number {
    return ++IdGenerator.counter;
  }

  static reset(): void {
    IdGenerator.counter = 0;
  }
}

IdGenerator.generate(); // 1
IdGenerator.generate(); // 2
```

### Implementing Interfaces

```typescript
interface Serializable {
  serialize(): string;
}

interface Loggable {
  log(): void;
}

class Config implements Serializable, Loggable {
  constructor(private data: Record<string, unknown>) {}

  serialize(): string { return JSON.stringify(this.data); }
  log(): void { console.log(this.serialize()); }
}
```

---

## 10. Type Narrowing & Type Guards

TypeScript narrows the type within a block when it can prove a value must be a specific type.

### `typeof` Narrowing

```typescript
function process(input: string | number | boolean) {
  if (typeof input === "string") {
    return input.toUpperCase(); // string
  } else if (typeof input === "number") {
    return input.toFixed(2);   // number
  }
  return input ? "yes" : "no"; // boolean
}
```

### `instanceof` Narrowing

```typescript
function handleError(error: unknown) {
  if (error instanceof TypeError) {
    console.error("Type error:", error.message);
  } else if (error instanceof RangeError) {
    console.error("Range error:", error.message);
  } else if (error instanceof Error) {
    console.error("General error:", error.message);
  } else {
    console.error("Unknown:", error);
  }
}
```

### `in` Operator Narrowing

```typescript
interface Admin { role: "admin"; permissions: string[] }
interface Guest { role: "guest" }

function getPermissions(user: Admin | Guest): string[] {
  if ("permissions" in user) {
    return user.permissions; // Admin
  }
  return [];                 // Guest
}
```

### Discriminated Unions

The cleanest, most scalable narrowing pattern. Each union member has a common literal field (`kind`, `type`, `status`):

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; side: number }
  | { kind: "rectangle"; width: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":    return Math.PI * shape.radius ** 2;
    case "square":    return shape.side ** 2;
    case "rectangle": return shape.width * shape.height;
    // TypeScript knows all cases are handled — no default needed
  }
}
```

### User-Defined Type Guards

```typescript
interface Cat { meow(): void }
interface Dog { bark(): void }

// Return type `pet is Cat` is a type predicate
function isCat(pet: Cat | Dog): pet is Cat {
  return "meow" in pet;
}

const pet = getPet();
if (isCat(pet)) {
  pet.meow(); // TypeScript knows it's Cat here
} else {
  pet.bark(); // TypeScript knows it's Dog here
}
```

### Exhaustiveness Checking with `never`

```typescript
type Color = "red" | "green" | "blue";

function handleColor(color: Color): string {
  switch (color) {
    case "red":   return "#FF0000";
    case "green": return "#00FF00";
    case "blue":  return "#0000FF";
    default:
      // If you add "yellow" to Color and forget a case,
      // TypeScript errors here: Type '"yellow"' is not assignable to 'never'
      const _exhaustiveCheck: never = color;
      throw new Error(`Unhandled color: ${_exhaustiveCheck}`);
  }
}
```

---

## 11. Utility Types

TypeScript ships with built-in generic utility types that transform existing types.

### `Partial<T>` — make all properties optional

```typescript
interface User { id: number; name: string; email: string; }

function updateUser(id: number, updates: Partial<User>) {
  // updates can have any subset of User properties
}

updateUser(1, { name: "Bob" }); // OK — doesn't need id or email
```

### `Required<T>` — make all properties required

```typescript
interface Config { host?: string; port?: number; }
type StrictConfig = Required<Config>; // { host: string; port: number }
```

### `Readonly<T>` — prevent mutation

```typescript
function freezeUser(user: User): Readonly<User> {
  return Object.freeze(user);
}

const u = freezeUser({ id: 1, name: "Alice", email: "a@a.com" });
// u.name = "Bob"; // Error: cannot assign to readonly property
```

### `Record<K, V>` — object with typed keys and values

```typescript
type PageVisits = Record<string, number>;
const visits: PageVisits = { home: 100, about: 50 };

type StatusMap = Record<"pending" | "active" | "done", User[]>;
```

### `Pick<T, K>` — select subset of properties

```typescript
type UserCard = Pick<User, "id" | "name">;
// { id: number; name: string }
```

### `Omit<T, K>` — exclude certain properties

```typescript
type PublicUser = Omit<User, "email" | "passwordHash">;
```

### `Exclude<T, U>` — remove union members

```typescript
type T = Exclude<"a" | "b" | "c" | "d", "a" | "c">; // "b" | "d"
type NonNull<T> = Exclude<T, null | undefined>;
```

### `Extract<T, U>` — keep only assignable members

```typescript
type T = Extract<"a" | "b" | "c", "a" | "c">; // "a" | "c"
```

### `NonNullable<T>` — remove null and undefined

```typescript
type T = NonNullable<string | null | undefined>; // string
```

### `ReturnType<T>` — get function's return type

```typescript
function createSession() {
  return { token: "abc", expiresAt: new Date() };
}
type Session = ReturnType<typeof createSession>;
// { token: string; expiresAt: Date }
```

### `Parameters<T>` — get function parameter types as tuple

```typescript
function createUser(name: string, role: string, age: number) {}
type CreateUserArgs = Parameters<typeof createUser>;
// [name: string, role: string, age: number]
```

### `Awaited<T>` — unwrap Promise type

```typescript
type T = Awaited<Promise<string>>;           // string
type U = Awaited<Promise<Promise<number>>>;  // number
```

### `InstanceType<T>` — get constructor's instance type

```typescript
class Logger { log(msg: string) {} }
type LoggerInstance = InstanceType<typeof Logger>; // Logger
```

---

## 12. Advanced Types

### Mapped Types

Iterate over keys and transform their types:

```typescript
// Make all properties mutable (remove readonly)
type Mutable<T> = { -readonly [K in keyof T]: T[K] };

// Make all properties nullable
type Nullable<T> = { [K in keyof T]: T[K] | null };

// Make all functions return Promise
type Promisify<T> = {
  [K in keyof T]: T[K] extends (...args: infer A) => infer R
    ? (...args: A) => Promise<R>
    : T[K];
};

// Key remapping with `as` (TS 4.1+)
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};
type UserGetters = Getters<User>;
// { getId: () => number; getName: () => string; getEmail: () => string }
```

### Conditional Types

```typescript
type IsArray<T> = T extends any[] ? true : false;

type A = IsArray<string[]>; // true
type B = IsArray<number>;   // false

// Distributive conditional types (distributes over unions)
type ToArray<T> = T extends any ? T[] : never;
type StrOrNumArray = ToArray<string | number>; // string[] | number[]
```

### `infer` Keyword

Extract a type from within a conditional type:

```typescript
// Unpack array element type
type ElementType<T> = T extends (infer E)[] ? E : never;
type E = ElementType<User[]>;  // User

// Unpack Promise value
type Awaited<T> = T extends Promise<infer V> ? Awaited<V> : T;

// Extract first function argument
type FirstArg<T> = T extends (first: infer F, ...rest: any[]) => any ? F : never;
type F = FirstArg<(x: number, y: string) => void>; // number

// Extract function return type
type Return<T> = T extends (...args: any[]) => infer R ? R : never;
```

### Indexed Access Types

```typescript
interface Post {
  id: number;
  title: string;
  author: { name: string; email: string };
  tags: string[];
}

type TitleType  = Post["title"];           // string
type AuthorType = Post["author"];          // { name: string; email: string }
type EmailType  = Post["author"]["email"]; // string
type TagType    = Post["tags"][number];    // string (element type)

// Works with unions
type IdOrTitle = Post["id" | "title"];     // number | string
```

---

## 13. Modules & Declaration Files

### ES Module Syntax in TypeScript

```typescript
// Named exports
export const PI = 3.14159;
export function add(a: number, b: number): number { return a + b; }
export interface User { id: number; name: string; }
export type Status = "active" | "inactive";

// Default export
export default class App { /* ... */ }

// Re-exports
export { something } from "./other";
export * from "./utils";
export * as mathUtils from "./math";
```

### Type-Only Imports/Exports

```typescript
// Type-only import — completely erased at runtime
import type { User } from "./types";
import type { FC, ReactNode } from "react";

// Mixed import
import { fetchUser, type UserResponse } from "./api";

// Type-only export
export type { User, Status };
```

### Declaration Files (`.d.ts`)

Describe the shape of JavaScript libraries so TypeScript understands them:

```typescript
// myLib.d.ts
declare module "my-untyped-lib" {
  export function process(input: string): string;
  export const VERSION: string;
  export interface Options { timeout?: number; }
}
```

### Ambient Declarations

```typescript
// Global variables injected by your environment/build tool
declare const __DEV__: boolean;
declare const __VERSION__: string;

// Augment existing module types (e.g., add to Express Request)
declare module "express-serve-static-core" {
  interface Request {
    user?: { id: string; role: "admin" | "user" };
  }
}
```

### `@types` Packages

```bash
npm install --save-dev @types/node      # Node.js types
npm install --save-dev @types/express   # Express types
npm install --save-dev @types/lodash    # Lodash types
```

Most modern packages ship with built-in types (no `@types` needed).

---

## 14. Decorators

Enable in `tsconfig.json`:
```json
{ "experimentalDecorators": true, "emitDecoratorMetadata": true }
```

### Class Decorator

```typescript
function singleton<T extends { new(...args: any[]): {} }>(Base: T) {
  let instance: InstanceType<T>;
  return class extends Base {
    constructor(...args: any[]) {
      super(...args);
      if (!instance) instance = this as InstanceType<T>;
      return instance;
    }
  };
}

@singleton
class Database {
  connect() { return "connected"; }
}
```

### Method Decorator

```typescript
function logCall(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`[${key}] called with:`, args);
    const result = original.apply(this, args);
    console.log(`[${key}] returned:`, result);
    return result;
  };
  return descriptor;
}

class MathService {
  @logCall
  add(a: number, b: number) { return a + b; }
}
```

### Decorator Factory (with arguments)

```typescript
function validate(minLength: number) {
  return function (target: any, propertyKey: string) {
    let value: string;
    Object.defineProperty(target, propertyKey, {
      get: () => value,
      set: (v: string) => {
        if (v.length < minLength)
          throw new Error(`${propertyKey} must be at least ${minLength} chars`);
        value = v;
      },
    });
  };
}

class User {
  @validate(3)
  name: string = "";

  @validate(8)
  password: string = "";
}
```

---

## 15. TypeScript with React (TSX)

### Typing Props

```tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
  variant?: "primary" | "secondary" | "danger";
  size?: "sm" | "md" | "lg";
}

// Plain arrow function — preferred over React.FC
const Button = ({
  label,
  onClick,
  disabled = false,
  variant = "primary",
  size = "md",
}: ButtonProps) => (
  <button
    className={`btn btn-${variant} btn-${size}`}
    onClick={onClick}
    disabled={disabled}
  >
    {label}
  </button>
);
```

### Typing Children

```tsx
import { ReactNode, PropsWithChildren } from "react";

// Explicit ReactNode
interface CardProps {
  title: string;
  children: ReactNode;
}

// PropsWithChildren helper adds children: ReactNode automatically
type PanelProps = PropsWithChildren<{
  className?: string;
  id?: string;
}>;
```

### Typing `useState`

```tsx
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
const [items, setItems] = useState<string[]>([]);
const [status, setStatus] = useState<"idle" | "loading" | "error">("idle");

// Inference works when initial value is typed
const [name, setName] = useState("");     // string inferred
const [active, setActive] = useState(false); // boolean inferred
```

### Typing `useRef`

```tsx
// DOM refs — initialize with null, access via .current
const inputRef = useRef<HTMLInputElement>(null);
const formRef = useRef<HTMLFormElement>(null);
const canvasRef = useRef<HTMLCanvasElement>(null);

// Mutable value ref (not a DOM node)
const timerRef = useRef<ReturnType<typeof setTimeout>>(undefined);
const prevValueRef = useRef<number>(0);

// Usage
const focusInput = () => inputRef.current?.focus();
```

### Typing Events

```tsx
// Input change
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

// Select change
const handleSelect = (e: React.ChangeEvent<HTMLSelectElement>) => {
  setSelected(e.target.value);
};

// Form submit
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
};

// Click
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.stopPropagation();
};

// Keyboard
const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
  if (e.key === "Enter") submit();
};

// Drag
const handleDrop = (e: React.DragEvent<HTMLDivElement>) => {
  const data = e.dataTransfer.getData("text");
};
```

### Typing `useReducer`

```tsx
type Action =
  | { type: "increment" }
  | { type: "decrement" }
  | { type: "reset"; payload: number }
  | { type: "setStep"; payload: number };

interface CounterState {
  count: number;
  step: number;
}

function reducer(state: CounterState, action: Action): CounterState {
  switch (action.type) {
    case "increment": return { ...state, count: state.count + state.step };
    case "decrement": return { ...state, count: state.count - state.step };
    case "reset":     return { ...state, count: action.payload };
    case "setStep":   return { ...state, step: action.payload };
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0, step: 1 });
dispatch({ type: "setStep", payload: 5 });
```

### Typing `useContext`

```tsx
interface AuthContextType {
  user: User | null;
  login: (credentials: { email: string; password: string }) => Promise<void>;
  logout: () => void;
  isLoading: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

// Type-safe custom hook — throws if used outside provider
export function useAuth(): AuthContextType {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error("useAuth must be inside AuthProvider");
  return ctx;
}
```

### Generic Components

```tsx
interface SelectProps<T> {
  options: T[];
  value: T;
  onChange: (value: T) => void;
  getLabel: (option: T) => string;
  getValue: (option: T) => string;
}

function Select<T>({ options, value, onChange, getLabel, getValue }: SelectProps<T>) {
  return (
    <select
      value={getValue(value)}
      onChange={(e) => {
        const selected = options.find((o) => getValue(o) === e.target.value);
        if (selected) onChange(selected);
      }}
    >
      {options.map((option) => (
        <option key={getValue(option)} value={getValue(option)}>
          {getLabel(option)}
        </option>
      ))}
    </select>
  );
}
```

### Typing Custom Hooks

```tsx
// Typed return tuple
function useToggle(initial = false): [boolean, () => void, (v: boolean) => void] {
  const [value, setValue] = useState(initial);
  const toggle = useCallback(() => setValue((v) => !v), []);
  return [value, toggle, setValue];
}

// Typed return object
function useCounter(initial = 0) {
  const [count, setCount] = useState(initial);
  return {
    count,
    increment: () => setCount((c) => c + 1),
    decrement: () => setCount((c) => c - 1),
    reset:     () => setCount(initial),
    set:       (n: number) => setCount(n),
  } as const; // as const preserves the tuple/object structure
}
```

---

## 16. Error Handling in TypeScript

### `unknown` in catch (TS 4.0+)

```typescript
try {
  await fetchData();
} catch (error: unknown) {
  if (error instanceof Error) {
    console.error("Message:", error.message);
    console.error("Stack:", error.stack);
  } else {
    console.error("Non-error thrown:", String(error));
  }
}
```

### Result Pattern (Railway-Oriented Programming)

```typescript
type Ok<T>  = { ok: true;  value: T };
type Err<E> = { ok: false; error: E };
type Result<T, E = Error> = Ok<T> | Err<E>;

function ok<T>(value: T): Ok<T>   { return { ok: true, value }; }
function err<E>(error: E): Err<E> { return { ok: false, error }; }

async function getUser(id: number): Promise<Result<User>> {
  try {
    const res = await fetch(`/api/users/${id}`);
    if (!res.ok) return err(new Error(`HTTP ${res.status}`));
    return ok(await res.json() as User);
  } catch (e) {
    return err(e instanceof Error ? e : new Error(String(e)));
  }
}

// Usage — exhaustive, no try/catch at call site
const result = await getUser(1);
if (result.ok) {
  console.log(result.value.name); // fully typed
} else {
  console.error(result.error.message);
}
```

---

## 17. tsconfig Deep Dive

### What `"strict": true` Enables

| Flag | Effect |
|---|---|
| `strictNullChecks` | `null`/`undefined` are not assignable to other types |
| `noImplicitAny` | Error when type is implicitly `any` |
| `strictFunctionTypes` | Stricter checking of function parameter types (contravariance) |
| `strictBindCallApply` | Strict checking of `bind`, `call`, `apply` arguments |
| `strictPropertyInitialization` | Class properties must be initialized in constructor |
| `noImplicitThis` | Error when `this` has implicit `any` type |
| `alwaysStrict` | Parse in strict mode, emit `"use strict"` |
| `useUnknownInCatchVariables` | Catch clause variables typed as `unknown` (TS 4.4+) |

### Path Aliases Setup

**tsconfig.json:**
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

**vite.config.ts (also needed for bundler):**
```typescript
import { defineConfig } from "vite";
import path from "path";

export default defineConfig({
  resolve: {
    alias: { "@": path.resolve(__dirname, "./src") },
  },
});
```

---

## 18. Best Practices & Common Pitfalls

### ✅ Do This

```typescript
// 1. Always enable strict mode
// tsconfig.json: "strict": true

// 2. Prefer unknown over any for unsafe input
function parseJSON(raw: unknown): Record<string, unknown> {
  if (typeof raw !== "object" || raw === null) throw new Error("Not an object");
  return raw as Record<string, unknown>;
}

// 3. Use discriminated unions to model state
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; message: string };

// 4. Use as const for enumerations without runtime cost
const HTTP_METHODS = ["GET", "POST", "PUT", "DELETE"] as const;
type HttpMethod = typeof HTTP_METHODS[number]; // "GET" | "POST" | ...

// 5. Derive types from data — don't duplicate
const COLORS = { red: "#FF0000", green: "#00FF00", blue: "#0000FF" } as const;
type ColorName = keyof typeof COLORS;  // "red" | "green" | "blue"
type ColorHex  = typeof COLORS[ColorName]; // "#FF0000" | "#00FF00" | "#0000FF"

// 6. Use ReturnType instead of duplicating return shapes
function buildConfig() { return { port: 3000, host: "localhost" }; }
type Config = ReturnType<typeof buildConfig>;
```

### ❌ Avoid These Mistakes

```typescript
// 1. Don't abuse any — it turns off TypeScript entirely
let data: any = fetchResponse; // avoid
let data: unknown = fetchResponse; // better

// 2. Don't use type assertion to lie to TypeScript
const user = {} as User; // will crash when you access user.name

// 3. Don't suppress errors silently
// @ts-ignore                   // bad — no explanation, hides issues
// @ts-expect-error JIRA-1234   // better — documented, fails if error disappears

// 4. Don't annotate what TypeScript can infer
const name: string = "Alice"; // redundant annotation
const name = "Alice";          // let TypeScript infer

// 5. Don't use the broad `object` or `Function` types
function run(fn: Function) {}                          // too broad
function run(fn: (...args: unknown[]) => unknown) {}   // specific

// 6. Don't create meaningless type aliases for primitives
type MyString = string; // pointless
```

### Mental Model Summary

| Question | Answer |
|---|---|
| Unknown data from API/user? | Use `unknown`, then validate + narrow |
| Object shape contract? | `interface` (supports merging) |
| Union / complex transformation? | `type` alias |
| Multiple types same shape? | Generics `<T>` |
| State with variants? | Discriminated union |
| Exclude/pick properties? | Utility types (`Omit`, `Pick`, `Partial`) |
| Runtime exists, type needed? | `typeof value` |
| Type exists, key union needed? | `keyof Type` |
