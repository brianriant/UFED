# Module 6: TypeScript

**Duration:** Week 10 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** TypeScript is JavaScript with type annotations! Start with plain JS and gradually add types.

---

## Overview

TypeScript is JavaScript with syntax for types. It helps catch errors early and improves code maintainability through static type checking.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand TypeScript fundamentals
- [ ] Work with types, interfaces, and generics
- [ ] Configure TypeScript projects
- [ ] Use advanced type patterns
- [ ] Integrate with modern frameworks

---

## Table of Contents

1. [Introduction to TypeScript](#introduction-to-typescript)
2. [Basic Types](#basic-types)
3. [Type Inference](#type-inference)
4. [Interfaces and Types](#interfaces-and-types)
5. [Functions](#functions)
6. [Classes](#classes)
7. [Generics](#generics)
8. [Utility Types](#utility-types)
9. [Configuration](#configuration)
10. [Projects](#projects)

---

## Introduction to TypeScript

### Why TypeScript?

```javascript
// JavaScript - errors caught at runtime
function greet(user) {
  return user.name.toUpperCase();  // Crashes if user is null!
}

// TypeScript - errors caught at compile time!
function greet(user: User): string {
  return user.name.toUpperCase();
}
```

> [!NOTE]
> TypeScript adds an extra compilation step that catches bugs before your code runs in production!

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** When are JavaScript errors typically caught vs TypeScript errors?
>
> - [ ] A) Both at runtime
> - [ ] B) JavaScript at runtime, TypeScript at compile time
> - [ ] C) Both at compile time
>
> <details>
> <summary>Answer</summary>
> **B) JavaScript at runtime, TypeScript at compile time** - This is TypeScript's main advantage!
> </details>

### Installation

```bash
# Install globally
npm install -g typescript

# Install in project
npm install -D typescript

# Install with types
npm install @types/node
```

### Setup

```bash
# Initialize TypeScript
tsc --init

# Compile file
tsc file.ts

# Watch mode
tsc --watch file.ts
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

---

## Basic Types

### Primitive Types

```typescript
// String
let name: string = "John";

// Number (integers and floats)
let age: number = 30;
let price: number = 19.99;

// Boolean
let isActive: boolean = true;

// Null and Undefined
let nothing: null = null;
let notDefined: undefined = undefined;
```

### Arrays

```typescript
// Array of strings
let fruits: string[] = ["apple", "banana"];
let numbers: number[] = [1, 2, 3];

// Generic syntax
let names: Array<string> = ["John", "Jane"];

// Tuple - fixed length with specific types
let point: [number, number] = [10, 20];
let user: [string, number] = ["John", 30];
```

### Object Types

```typescript
// Inline
let user: { name: string; age: number } = {
  name: "John",
  age: 30,
};

// Optional property
let person: { name: string; age?: number } = {
  name: "Jane",
};

// Readonly
let config: { readonly apiKey: string } = {
  apiKey: "secret",
};
```

### Special Types

```typescript
// Any (avoid when possible!)
let anything: any = "hello";
anything = 42;
anything = true;

// Unknown (safer than any)
let unknown: unknown = "hello";
if (typeof unknown === "string") {
  console.log(unknown.toUpperCase());
}

// Void (no return value)
function log(message: string): void {
  console.log(message);
}

// Never (never returns)
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}

// Type assertion
let value: unknown = "hello";
let length: number = (value as string).length;
let length2: number = (<string>value).length;
```

> [!WARNING]
> ⚠️ Avoid `any` - it defeats the purpose of TypeScript! Use `unknown` when you truly don't know the type.

---

## Type Inference

### TypeScript Inference

```typescript
// Type is inferred from value
let name = "John"; // type: string
let age = 30; // type: number
let isActive = true; // type: boolean

// Function return types inferred
function add(a: number, b: number) {
  return a + b; // return type: number
}

// Best common type
let items = [1, 2, 3]; // number[]
let mixed = [1, "hello"]; // (string | number)[]
```

> [!TIP]
> Let TypeScript infer types when possible - less code, same safety!

### Contextual Typing

```typescript
// Type inferred from context
document.addEventListener("click", (event) => {
  // event is MouseEvent - TypeScript knows this!
  console.log(event.clientX);
});

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  // e is typed based on context
};
```

---

## Interfaces and Types

### Interfaces

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // optional
  readonly createdAt: Date; // can't modify
}

// Extending interfaces
interface Admin extends User {
  role: "admin" | "superadmin";
  permissions: string[];
}

// Interface with methods
interface Calculator {
  add(a: number, b: number): number;
  subtract(a: number, b: number): number;
}
```

### Type Aliases

```typescript
// Simple alias
type ID = string | number;
type Status = "pending" | "active" | "completed";

// Object type
type Point = {
  x: number;
  y: number;
};

// Function type
type Callback = (error: Error | null, result?: string) => void;

// Tuple type
type Pair<T> = [T, T];
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** When should you use `interface` vs `type`?
>
> - [ ] A) Always use interface
> - [ ] B) Interface for objects, type for unions/primitives
> - [ ] C) Always use type
>
> <details>
> <summary>Answer</summary>
> **B) Interface for objects (can be extended), type for unions, primitives, tuples** - Both work for objects, but they have different strengths.
> </details>

### Interface vs Type

```typescript
// Interface - better for objects, can be extended/merged
interface Animal {
  name: string;
}

interface Animal {
  age: number;
}
// Animal now has both properties!

// Type - better for unions, primitives, tuples
type Status = "pending" | "approved" | "rejected";
type Point = { x: number; y: number };
type Callback = () => void;
```

---

## Functions

### Function Types

```typescript
// Named function
function add(a: number, b: number): number {
  return a + b;
}

// Arrow function
const add = (a: number, b: number): number => a + b;

// Function type variable
let add: (a: number, b: number) => number;
add = (a, b) => a + b;

// Optional parameters
function greet(name: string, greeting?: string): string {
  return greeting ? `${greeting}, ${name}!` : `Hello, ${name}!`;
}

// Default parameters
function greet(name: string, greeting = "Hello"): string {
  return `${greeting}, ${name}!`;
}

// Rest parameters
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### Overloads

```typescript
// Function overloads - multiple signatures
function getItem(id: string): Item | undefined;
function getItem(id: number): Item | undefined;
function getItem(id: string | number): Item | undefined {
  if (typeof id === "string") {
    return items.find((item) => item.id === id);
  }
  return items.find((item) => item.id === id);
}
```

---

## Classes

### Class Syntax

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hi, I'm ${this.name}`;
  }
}

const person = new Person("John", 30);
```

### Access Modifiers

```typescript
class Person {
  public name: string; // accessible everywhere (default)
  private age: number; // only inside class
  protected email: string; // inside class and subclasses

  constructor(name: string, age: number, email: string) {
    this.name = name;
    this.age = age;
    this.email = email;
  }
}

// Readonly
class Config {
  readonly apiKey: string;

  constructor(apiKey: string) {
    this.apiKey = apiKey;
  }
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What access modifier makes a property accessible in the class and its subclasses but not outside?
>
> - [ ] A) public
> - [ ] B) private
> - [ ] C) protected
>
> <details>
> <summary>Answer</summary>
> **C) protected** - It's like private but allows subclass access!
> </details>

### Inheritance

```typescript
class Animal {
  constructor(public name: string) {}

  speak(): void {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name);
    this.breed = breed;
  }

  speak(): void {
    console.log(`${this.name} barks`);
  }
}
```

### Abstract Classes

```typescript
abstract class Shape {
  abstract area(): number;
  abstract perimeter(): number;

  describe(): void {
    console.log(`Area: ${this.area()}`);
  }
}

class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }

  area(): number {
    return Math.PI * this.radius ** 2;
  }

  perimeter(): number {
    return 2 * Math.PI * this.radius;
  }
}
```

---

## Generics

### Generic Functions

```typescript
function identity<T>(value: T): T {
  return value;
}

identity<string>("hello"); // type: string
identity(42); // type: number

// Multiple type parameters
function pair<K, V>(key: K, value: V): [K, V] {
  return [key, value];
}
```

### Generic Interfaces

```typescript
interface Container<T> {
  value: T;
  getValue(): T;
}

interface Repository<T> {
  find(id: string): Promise<T | null>;
  save(item: T): Promise<T>;
  delete(id: string): Promise<boolean>;
}
```

### Generic Classes

```typescript
class Box<T> {
  private content: T;

  constructor(content: T) {
    this.content = content;
  }

  getContent(): T {
    return this.content;
  }
}

const box = new Box<string>("hello");
const numberBox = new Box(42); // type inferred!
```

### Constraints

```typescript
// Extend constraint - must have certain properties
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): void {
  console.log(item.length);
}

logLength("hello"); // OK - strings have length
logLength([1, 2, 3]); // OK - arrays have length
logLength({ length: 5 }); // OK - object has length
// logLength(42);     // Error: number doesn't have length
```

> [!TIP]
> Use `extends` to constrain generics to certain types!

---

## Utility Types

### Common Utilities

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Partial - all properties optional
type PartialUser = Partial<User>;
// { id?: number; name?: string; ... }

// Required - all properties required
type RequiredUser = Required<PartialUser>;

// Readonly - all properties readonly
type ReadonlyUser = Readonly<User>;

// Pick - select specific properties
type UserPreview = Pick<User, "id" | "name">;
// { id: number; name: string }

// Omit - remove specific properties
type UserWithoutEmail = Omit<User, "email">;
// { id: number; name: string; age: number }
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** Which utility type would you use to create a type with only `id` and `name` from User?
>
> - [ ] A) Partial
> - [ ] B) Pick
> - [ ] C) Omit
>
> <details>
> <summary>Answer</summary>
> **B) Pick** - `Pick<User, 'id' | 'name'>` selects those specific properties.
> </details>

### Union Types

```typescript
// Extract - extract from union
type Status = "pending" | "active" | "completed";
type ActiveStatus = Extract<Status, "active" | "pending">;

// Exclude - remove from union
type NonActiveStatus = Exclude<Status, "active">;

// NonNullable - remove null/undefined
type NonNull = NonNullable<string | null | undefined>;
```

### Other Utilities

```typescript
// ReturnType - get return type of function
type HandlerReturn = ReturnType<typeof fetch>;

// Parameters - get parameter types
type HandlerParams = Parameters<typeof fetch>;

// InstanceType - get instance type of class
type UserInstance = InstanceType<typeof User>;
```

---

## Configuration

### Compiler Options

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM"],
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "sourceMap": true
  }
}
```

### Important Flags

| Option             | Description                             |
| ------------------ | --------------------------------------- |
| `strict`           | Enable all strict checks (recommended!) |
| `noImplicitAny`    | Error on implicit any                   |
| `strictNullChecks` | Nullable type safety                    |
| `noUnusedLocals`   | Error on unused vars                    |
| `esModuleInterop`  | CommonJS interop                        |

> [!TIP]
> Always set `"strict": true` - it catches the most bugs!

---

## Projects

### ⭐ Project 1: Type-Safe API Client

Create a typed API client:

- [ ] Generic request function with type parameters
- [ ] Typed response handling
- [ ] Error types with discriminated unions
- [ ] Type-safe endpoints

---

### ⭐ Project 2: Form Validation

Build a form with:

- [ ] Type-safe form state using generics
- [ ] Validation schema with Zod or Yup
- [ ] Error type definitions
- [ ] Type guards for validation

---

### ⭐ Project 3: Generic Data Store

Create a store with:

- [ ] Generic CRUD operations
- [ ] Type-safe filtering with predicate functions
- [ ] Type-safe sorting comparators
- [ ] Event typing for changes

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between `interface` and `type`?
2. [ ] What is type inference?
3. [ ] How do generics work?
4. [ ] What are utility types?
5. [ ] How do you handle nullable types?
6. [ ] What are conditional types?
7. [ ] How do you create a custom type guard?

<details>
<summary>Click to reveal all answers</summary>

1. Interface for objects (can merge), type for unions/primitives/tuples
2. TypeScript automatically figuring out the type from context
3. Generic types let you write reusable, type-safe code that works with multiple types
4. Built-in types like Partial, Pick, Omit that transform other types
5. Use optional properties (`?`), null checks, or strictNullChecks
6. Types that depend on other types - e.g., `T extends U ? X : Y`
7. Functions returning boolean that narrow types - `function isString(x: unknown): x is string`

</details>

---

## Resources

- [TypeScript Docs](https://www.typescriptlang.org/docs/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [TypeScript Exercises](https://exercism.org/tracks/typescript)
- [Type Challenges](https://github.com/type-challenges/type-challenges)

---

## Progress Checklist

- [ ] TypeScript installed and configured
- [ ] Read through all sections
- [ ] Completed Project 1 (API Client)
- [ ] Completed Project 2 (Form Validation)
- [ ] Answered all quiz questions
- [ ] Ready for Module 7: React

---

## Next Module

[Module 7: React →](./07-react.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Understanding For Every Developer_
