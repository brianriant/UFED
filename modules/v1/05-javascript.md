# Module 5: JavaScript Fundamentals

**Duration:** Weeks 5-9 | **Difficulty:** Beginner to Intermediate

---

> [!TIP]
> **Learning Tip:** JavaScript is best learned by coding! Open your browser console (`F12` → Console) and try each example as you read.

---

## Overview

JavaScript is the programming language of the web. It powers interactive features, handles logic, and enables both front-end and back-end development.

## Learning Objectives

By the end of this module, you will:

- [ ] Master JavaScript syntax and data types
- [ ] Understand functions, arrays, and objects
- [ ] Work with DOM manipulation
- [ ] Handle asynchronous operations
- [ ] Use modern ES6+ features

---

## Table of Contents

1. [Introduction](#introduction)
2. [Variables and Data Types](#variables-and-data-types)
3. [Operators](#operators)
4. [Control Flow](#control-flow)
5. [Functions](#functions)
6. [Arrays](#arrays)
7. [Objects](#objects)
8. [DOM Manipulation](#dom-manipulation)
9. [Events](#events)
10. [Async JavaScript](#async-javascript)
11. [Modern ES6+](#modern-es6)
12. [Projects](#projects)

---

## Introduction

### Where JavaScript Runs

```javascript
// Browser - open DevTools Console
console.log("Hello from browser!");

// Node.js
// console.log('Hello from Node.js!');
```

> [!TIP]
> Open Chrome DevTools with `F12` or `Cmd+Option+I` (Mac). Try typing `console.log("Hello!")` in the Console tab!

### Adding JavaScript to HTML

```html
<!-- External (recommended) -->
<script src="app.js"></script>

<!-- At the end of body -->
<script>
  // Inline code
</script>

<!-- Async (don't block rendering) -->
<script src="app.js" async></script>

<!-- Deferred (execute after HTML parses) -->
<script src="app.js" defer></script>
```

> [!WARNING]
> ⚠️ Put `<script>` at the end of `<body>` or use `defer` to ensure HTML is loaded first!

---

## Variables and Data Types

### Variables

```javascript
// const - cannot reassign (preferred!)
const name = "John";
const age = 25;

// let - can reassign
let count = 0;
count = 1;

// var - old way (avoid)
var oldWay = "deprecated";

// Block scope
{
  const blockScoped = "only works here";
}
// console.log(blockScoped); // Error!

// Function scope (var only)
function example() {
  var functionScoped = "works inside function";
}
// console.log(functionScoped); // Error!
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** Which keyword should you use for a variable that won't be reassigned?
>
> - [ ] A) var
> - [ ] B) let
> - [ ] C) const
>
> <details>
> <summary>Answer</summary>
> **C) const** - Always prefer `const` unless you need to reassign the variable!
> </details>

### Data Types

```javascript
// Primitive Types
const string = "Hello"; // String
const number = 42; // Number
const bigInt = 9007199254740991n; // BigInt
const boolean = true; // Boolean
const empty = null; // Null
let notDefined; // Undefined
const symbol = Symbol("id"); // Symbol

// Reference Types
const array = [1, 2, 3]; // Array
const object = { key: "value" }; // Object
const function1 = function () {}; // Function

// typeof
console.log(typeof "hello"); // "string"
console.log(typeof 42); // "number"
console.log(typeof []); // "object" (surprise!)
```

> [!NOTE]
> JavaScript has dynamic typing - the same variable can hold different types!

### String Methods

```javascript
const text = "Hello World";

// Length
text.length; // 11

// Case
text.toUpperCase(); // "HELLO WORLD"
text.toLowerCase(); // "hello world"

// Search
text.indexOf("World"); // 6
text.includes("Hello"); // true
text.startsWith("Hello"); // true
text.endsWith("World"); // true

// Slice/Substring
text.slice(0, 5); // "Hello"
text.substring(0, 5); // "Hello"
text.replace("World", "JS"); // "Hello JS"

// Split
text.split(" "); // ["Hello", "World"]

// Template Literals
const name = "John";
const greeting = `Hello, ${name}!`; // "Hello, John!"
```

> [!TIP]
> Try these in your browser console! Type `'Hello World'.toUpperCase()`

---

## Operators

### Arithmetic

```javascript
const sum = 10 + 5; // 15
const diff = 10 - 5; // 5
const product = 10 * 5; // 50
const quotient = 10 / 5; // 2
const remainder = 10 % 3; // 1
const power = 2 ** 3; // 8
```

### Comparison

```javascript
// Equal (loose) - avoid!
5 == "5"; // true
5 != "5"; // false

// Equal (strict - use this!)
5 === "5"; // false
5 !== "5"; // true

// Greater/Less
5 > 3; // true
5 >= 5; // true
3 < 5; // true
3 <= 3; // true
```

> [!WARNING]
> ⚠️ Always use `===` instead of `==`! Loose comparison can cause unexpected bugs.

### Logical

```javascript
true && false; // false (and)
true || false; // true (or)
!true; // false (not)

// Short-circuit evaluation
const result = true && "hello"; // "hello"
const fallback = null || "default"; // "default"
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What is the result of `null || 'default'`?
>
> - [ ] A) null
> - [ ] B) 'default'
> - [ ] C) Error
>
> <details>
> <summary>Answer</summary>
> **B) 'default'** - If left side is falsy (null, undefined, false, 0, ''), returns right side.
> </details>

### Nullish

```javascript
// ?? returns right side if left is null/undefined
null ?? "default"; // "default"
undefined ?? "default"; // "default"
0 ?? "default"; // 0
"" ?? "default"; // ""
```

---

## Control Flow

### If/Else

```javascript
const age = 18;

if (age >= 18) {
  console.log("Adult");
} else if (age >= 13) {
  console.log("Teen");
} else {
  console.log("Child");
}

// Ternary operator
const status = age >= 18 ? "Adult" : "Minor";
```

### Switch

```javascript
const day = "Monday";

switch (day) {
  case "Monday":
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
  case "Friday":
    console.log("Weekday");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend");
    break;
  default:
    console.log("Invalid");
}
```

### Loops

```javascript
// For loop
for (let i = 0; i < 5; i++) {
  console.log(i); // 0,1,2,3,4
}

// While
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

// Do-while (runs at least once)
do {
  console.log(count);
} while (count < 0);

// For...of (arrays) - modern!
const items = ["a", "b", "c"];
for (const item of items) {
  console.log(item);
}

// For...in (objects)
const obj = { a: 1, b: 2 };
for (const key in obj) {
  console.log(key, obj[key]);
}
```

> [!TIP]
> Use `for...of` for arrays and `for...in` for object properties!

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** Which loop is best for iterating over array elements?
>
> - [ ] A) for...in
> - [ ] B) for...of
> - [ ] C) while
>
> <details>
> <summary>Answer</summary>
> **B) for...of** - It's the cleanest way to iterate over array values!
> </details>

---

## Functions

### Function Declaration

```javascript
// Named function
function greet(name) {
  return `Hello, ${name}!`;
}

// Function expression
const greet = function (name) {
  return `Hello, ${name}!`;
};

// Arrow function (preferred in modern JS)
const greet = (name) => `Hello, ${name}!`;

// Arrow with body
const greet = (name) => {
  const message = `Hello, ${name}!`;
  return message;
};
```

### Parameters

```javascript
// Default parameters
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10

// Destructuring
function display({ name, age }) {
  console.log(name, age);
}
display({ name: "John", age: 30 });
```

### Scope

```javascript
// Global scope
const global = "accessible everywhere";

function example() {
  // Function scope
  const inside = "only here";

  // Can access global
  console.log(global);
}

// Closure
function counter() {
  let count = 0;
  return function () {
    return ++count;
  };
}
const increment = counter();
increment(); // 1
increment(); // 2
```

> [!NOTE]
> A closure is a function that remembers its outer scope even after the outer function has returned!

---

## Arrays

### Creating Arrays

```javascript
const fruits = ["apple", "banana", "orange"];
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "hello", true, null];

// Array constructor
const arr = new Array(1, 2, 3);

// Array.of - create array from arguments
Array.of(1, 2, 3); // [1, 2, 3]
```

### Accessing Elements

```javascript
const arr = ["a", "b", "c"];

arr[0]; // 'a'
arr[arr.length - 1]; // 'c'
arr.at(-1); // 'c' (negative index!)
```

### Array Methods

```javascript
const arr = [1, 2, 3];

// Adding/removing
arr.push(4); // add to end -> [1,2,3,4]
arr.pop(); // remove from end -> returns 4
arr.unshift(0); // add to start -> [0,1,2,3]
arr.shift(); // remove from start -> returns 0

// Finding
arr.indexOf(2); // 1
arr.includes(2); // true
arr.find((x) => x > 2); // 3
arr.findIndex((x) => x > 2); // 2

// Transforming
arr.map((x) => x * 2); // [2,4,6]
arr.filter((x) => x > 2); // [3]
arr.reduce((sum, x) => sum + x, 0); // 6

// Other
arr.slice(1, 3); // [2,3]
arr.splice(1, 1, "x"); // remove 1 at index 1, add 'x'
arr.join("-"); // "1-2-3"
arr.reverse(); // [3,2,1]

// Check
arr.every((x) => x > 0); // true
arr.some((x) => x > 2); // true
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What does `arr.map(x => x * 2)` return for `[1, 2, 3]`?
>
> - [ ] A) [1, 2, 3]
> - [ ] B) [2, 4, 6]
> - [ ] C) [1, 4, 9]
>
> <details>
> <summary>Answer</summary>
> **B) [2, 4, 6]** - map transforms each element!
> </details>

### Spread Operator

```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5]; // [1,2,3,4,5]

// Copy array
const copy = [...arr];

// Merge arrays
const merged = [...arr1, ...arr2];
```

---

## Objects

### Creating Objects

```javascript
const person = {
  name: "John",
  age: 30,
  isStudent: false,

  // Method
  greet() {
    return `Hi, I'm ${this.name}`;
  },

  // Computed property
  ["key" + 1]: "value",
};

// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Class syntax (preferred)
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hi, I'm ${this.name}`;
  }
}
```

### Accessing Properties

```javascript
const person = { name: "John", age: 30 };

// Dot notation
person.name; // 'John'

// Bracket notation
person["name"]; // 'John'

// Destructuring
const { name, age } = person;
const { name: fullName } = person; // rename
```

### Object Methods

```javascript
const obj = { a: 1, b: 2 };

Object.keys(obj); // ['a', 'b']
Object.values(obj); // [1, 2]
Object.entries(obj); // [['a',1], ['b',2]]

// Merge objects
const merged = { ...obj, c: 3 };

// Clone
const clone = { ...obj };
const deepClone = structuredClone(obj);
```

---

## DOM Manipulation

### Selecting Elements

```javascript
// Single element
document.getElementById("app");
document.querySelector(".container");

// Multiple elements
document.getElementsByClassName("item");
document.querySelectorAll(".item");
```

### Modifying Elements

```javascript
const el = document.querySelector("#app");

// Content
el.textContent = "New text";
el.innerHTML = "<strong>Bold</strong>";

// Attributes
el.setAttribute("class", "active");
el.getAttribute("class");
el.removeAttribute("id");

// Classes
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");
el.classList.contains("active");

// Styles
el.style.color = "blue";
el.style.fontSize = "16px";
```

> [!TIP]
> Try this in your browser: `document.body.style.backgroundColor = 'lightblue'`

### Creating Elements

```javascript
// Create element
const div = document.createElement("div");
div.textContent = "Hello";
div.className = "container";

// Add to DOM
document.body.appendChild(div);
parent.append(div); // modern

// Insert before
parent.insertBefore(newEl, existingEl);

// Remove
el.remove();
parent.removeChild(el);
```

---

## Events

### Event Listeners

```javascript
const btn = document.querySelector("#btn");

// Add listener
btn.addEventListener("click", function (event) {
  console.log("Clicked!");
});

// Arrow function
btn.addEventListener("click", (e) => {
  console.log(e.target);
});

// Remove listener (needs named function)
function handler() {
  console.log("Clicked");
}
btn.addEventListener("click", handler);
btn.removeEventListener("click", handler);

// Once (auto-remove after first click)
btn.addEventListener("click", handler, { once: true });
```

### Common Events

```javascript
// Mouse
(click, dblclick, mousedown, mouseup, mouseover, mouseout);
(mousemove, contextmenu);

// Keyboard
(keydown, keyup, keypress);

// Form
(submit, change, input, focus, blur);

// Window
(load, resize, scroll, DOMContentLoaded);
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 5:** What's the difference between `click` and `dblclick`?
>
> - [ ] A) There's no difference
> - [ ] B) click fires on single click, dblclick fires on double click
> - [ ] C) dblclick only works on links
>
> <details>
> <summary>Answer</summary>
> **B) click fires on single click, dblclick fires on double click** - But be careful: double-clicking also fires click events!
> </details>

### Event Object

```javascript
btn.addEventListener("click", (e) => {
  e.target; // element that triggered
  e.currentTarget; // element with listener
  e.preventDefault(); // stop default behavior
  e.stopPropagation(); // stop bubbling

  // Keyboard
  e.key; // 'Enter'
  e.code; // 'Enter'
  e.ctrlKey; // boolean

  // Mouse
  e.button; // 0=left, 1=middle, 2=right
  (e.clientX, e.clientY); // coordinates
});
```

---

## Async JavaScript

### Callbacks (Old Way)

```javascript
// Old way
function fetchData(callback) {
  setTimeout(() => {
    callback({ data: "success" });
  }, 1000);
}

fetchData((result) => {
  console.log(result);
});
```

### Promises

```javascript
// Create promise
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve({ data: "success" });
  }, 1000);
});

// Use promise
promise
  .then((data) => console.log(data))
  .catch((error) => console.error(error))
  .finally(() => console.log("Done"));
```

### Async/Await (Modern!)

```javascript
// Async function
async function getData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error:", error);
  }
}

// Call it
getData().then((data) => console.log(data));
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 6:** What keyword makes a function return a Promise?
>
> - [ ] A) await
> - [ ] B) promise
> - [ ] C) async
>
> <details>
> <summary>Answer</summary>
> **C) async** - Adding `async` before a function makes it return a Promise automatically.
> </details>

### Fetch API

```javascript
// GET request
fetch("https://api.example.com/users")
  .then((res) => res.json())
  .then((data) => console.log(data));

// POST request
fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({ name: "John" }),
})
  .then((res) => res.json())
  .then((data) => console.log(data));
```

---

## Modern ES6+

### Arrow Functions

```javascript
// Regular
function add(a, b) {
  return a + b;
}

// Arrow
const add = (a, b) => a + b;

// Single param (no parentheses needed)
const double = (x) => x * 2;
```

> [!NOTE]
> Arrow functions don't have their own `this` - they inherit from parent scope!

### Template Literals

```javascript
const name = "John";
const age = 30;

const message = `My name is ${name} and I'm ${age} years old`;
```

### Destructuring

```javascript
// Array
const [first, second] = [1, 2, 3];
const [first, ...rest] = [1, 2, 3]; // rest = [2, 3]

// Object
const { name, age } = { name: "John", age: 30 };
const { name: fullName } = { name: "John" }; // rename

// Function parameters
function greet({ name, age }) {
  console.log(name, age);
}
```

### Spread and Rest

```javascript
// Spread in arrays
const arr = [1, 2, 3];
const newArr = [...arr, 4];

// Spread in objects
const obj = { a: 1 };
const newObj = { ...obj, b: 2 };

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b);
}
```

### Optional Chaining

```javascript
const user = { profile: { name: "John" } };

// Safe access
user?.profile?.name; // 'John'
user?.address?.city; // undefined (no error)
user?.address?.city ?? "Unknown"; // 'Unknown'
```

### Classes

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks`);
  }
}
```

### Modules

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export default (a, b) => a * b;

// main.js
import { add, subtract } from "./math.js";
import multiply from "./math.js";

console.log(add(1, 2)); // 3
console.log(multiply(2, 3)); // 6
```

---

## Projects

### ⭐ Project 1: Counter App

Build a counter with:

- [ ] Display current count
- [ ] Increment/Decrement buttons
- [ ] Reset button
- [ ] Visual feedback on state

> [!TIP]
> **Try it live:** [Open in CodePen](https://codepen.io/pen/?template=js-counter)

---

### ⭐ Project 2: Todo List

Create a todo app with:

- [ ] Add new todo
- [ ] Mark complete/incomplete
- [ ] Delete todo
- [ ] Filter (all/active/completed)
- [ ] Persist to localStorage

> [!WARNING]
> ⚠️ localStorage stores strings! Use `JSON.stringify()` and `JSON.parse()`

---

### ⭐ Project 3: Weather App

Build a weather app that:

- [ ] Takes user location or city input
- [ ] Fetches weather API data (use Open-Meteo - no API key needed!)
- [ ] Displays temperature, conditions
- [ ] Shows loading/error states

> [!TIP]
> Try the free [Open-Meteo API](https://open-meteo.com/) - no authentication needed!

---

### ⭐ Project 4: Modal Component

Create a modal with:

- [ ] Open/close buttons
- [ ] Click outside to close
- [ ] ESC key to close
- [ ] Animation transitions

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between `let` and `const`?
2. [ ] How do arrow functions differ from regular functions?
3. [ ] What is the purpose of `async/await`?
4. [ ] How does the DOM differ from the virtual DOM?
5. [ ] What is event bubbling?
6. [ ] How do you prevent default form submission?
7. [ ] What's the difference between `==` and `===`?
8. [ ] How do closures work?

<details>
<summary>Click to reveal all answers</summary>

1. `let` allows reassignment, `const` doesn't
2. Arrow functions don't have their own `this`, shorter syntax
3. Cleaner syntax for working with Promises
4. DOM is the actual HTML, virtual DOM is a JS representation (React uses this)
5. Events propagate from target up to parent elements
6. `event.preventDefault()` in the submit handler
7. `==` converts types, `===` compares without conversion
8. A function that "remembers" variables from its outer scope even after the outer function returns

</details>

---

## Resources

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
- [FreeCodeCamp JS Course](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)

---

## Progress Checklist

- [ ] Read through the entire module
- [ ] Completed Project 1 (Counter App)
- [ ] Completed Project 2 (Todo List)
- [ ] Completed Project 3 (Weather App - Optional)
- [ ] Answered all quiz questions
- [ ] Ready for Module 6: TypeScript

---

## Next Module

[Module 6: TypeScript →](./06-typescript.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Understanding For Every Developer_
