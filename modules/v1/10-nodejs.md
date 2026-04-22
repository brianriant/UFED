# Module 10: Node.js

**Duration:** Weeks 17-18 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** Node.js lets you run JavaScript on the server! Think of it as having the browser's console but on your computer.

---

## Overview

Node.js is a JavaScript runtime built on Chrome's V8 engine. It enables JavaScript to run on the server, allowing developers to build full-stack applications with JavaScript.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand Node.js fundamentals
- [ ] Work with the file system
- [ ] Handle events and streams
- [ ] Create HTTP servers
- [ ] Work with npm and packages

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Modules](#modules)
3. [File System](#file-system)
4. [Events](#events)
5. [Streams](#streams)
6. [HTTP Server](#http-server)
7. [NPM](#npm)
8. [Projects](#projects)

---

## Getting Started

### Installation

```bash
# macOS
brew install node

# Check installation
node --version
npm --version
```

### First Script

```javascript
// hello.js
console.log('Hello, Node.js!');

// Run
node hello.js
```

### Global Objects

```javascript
// __dirname - current directory path
console.log(__dirname);

// __filename - current file path
console.log(__filename);

// process - process information
console.log(process.platform);
console.log(process.env.PATH);
console.log(process.argv);

// setTimeout, setInterval (like browser)
setTimeout(() => console.log("After 1 second"), 1000);
setInterval(() => console.log("Every second"), 1000);
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What does `process.argv` contain?
>
> - [ ] A) Environment variables
> - [ ] B) Command line arguments
> - [ ] C) Current directory
>
> <details>
> <summary>Answer</summary>
> **B) Command line arguments** - First two are node path and script path, then your arguments!
> </details>

---

## Modules

### CommonJS (Node.js default)

```javascript
// math.js - Export
module.exports = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
  multiply: (a, b) => a * b,
  divide: (a, b) => a / b,
};

// Or use exports
exports.add = (a, b) => a + b;

// Import
const math = require("./math");
console.log(math.add(2, 3));
```

### ES Modules

```javascript
// package.json - enable "type": "module"
// Or use .mjs extension

// math.js
export const add = (a, b) => a + b;
export default (a, b) => a - b;

// Import
import { add } from "./math.js";
import subtract from "./math.js";
```

### Built-in Modules

```javascript
const path = require("path");
const fs = require("fs");
const http = require("http");
const url = require("url");
const os = require("os");
const events = require("events");
const crypto = require("crypto");
```

> [!NOTE]
> Need a module? Just `require()` it! No installation needed for built-in modules.

---

## File System

### Reading Files

```javascript
const fs = require("fs");

// Synchronous (blocking)
const content = fs.readFileSync("file.txt", "utf8");
console.log(content);

// Asynchronous (non-blocking)
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promise-based (recommended!)
import { readFile } from "fs/promises";

const content = await readFile("file.txt", "utf8");
console.log(content);
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What's the difference between readFileSync and readFile?
>
> - [ ] A) They're the same
> - [ ] B) Sync blocks execution, async doesn't
> - [ ] C) Async is faster
>
> <details>
> <summary>Answer</summary>
> **B) Sync blocks execution, async doesn't** - Use async for I/O operations to keep your server responsive!
> </details>

### Writing Files

```javascript
const fs = require("fs");

// Write (overwrite)
fs.writeFileSync("output.txt", "Hello, World!");

// Write async
fs.writeFile("output.txt", "Hello, World!", (err) => {
  if (err) throw err;
});

// Append
fs.appendFileSync("output.txt", "\nNew line");
```

### File Operations

```javascript
const fs = require("fs");

// Check if file exists
fs.existsSync("file.txt"); // boolean

// Create directory
fs.mkdirSync("new-dir");

// Delete directory
fs.rmdirSync("old-dir");

// Delete file
fs.unlinkSync("file.txt");

// Rename
fs.renameSync("old.txt", "new.txt");

// Stats
const stats = fs.statSync("file.txt");
console.log(stats.isFile());
console.log(stats.isDirectory());
console.log(stats.size);
console.log(stats.mtime);
```

---

## Events

### EventEmitter

```javascript
const EventEmitter = require("events");

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

// Register event
myEmitter.on("event", (data) => {
  console.log("Event triggered:", data);
});

// Register once (fires only once)
myEmitter.once("once-event", () => {
  console.log("This fires only once");
});

// Emit event
myEmitter.emit("event", { message: "Hello!" });

// Remove listener
const listener = () => console.log("Test");
myEmitter.on("test", listener);
myEmitter.removeListener("test", listener);
```

### Built-in Events

```javascript
// Process events
process.on("exit", () => {
  console.log("About to exit");
});

process.on("uncaughtException", (err) => {
  console.error("Uncaught exception:", err);
});

process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled rejection:", reason);
});
```

---

## Streams

### Types of Streams

| Type          | Description                            |
| ------------- | -------------------------------------- |
| **Readable**  | Read data (e.g., file read stream)     |
| **Writable**  | Write data (e.g., file write stream)   |
| **Duplex**    | Both read and write (e.g., TCP socket) |
| **Transform** | Modify data (e.g., compression)        |

### Reading Streams

```javascript
const fs = require("fs");

const readStream = fs.createReadStream("large-file.txt", "utf8");

readStream.on("data", (chunk) => {
  console.log("Received chunk:", chunk.length);
});

readStream.on("end", () => {
  console.log("Finished reading");
});

readStream.on("error", (err) => {
  console.error("Error:", err);
});
```

### Piping

```javascript
const fs = require("fs");

// Read from one file, write to another
const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);

// HTTP file server
const http = require("http");
const server = http.createServer((req, res) => {
  const stream = fs.createReadStream("file.pdf");
  res.writeHead(200, {
    "Content-Type": "application/pdf",
  });
  stream.pipe(res);
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What does `.pipe()` do in Node.js streams?
>
> - [ ] A) Connects two streams together
> - [ ] B) Deletes a stream
> - [ ] C) Creates a new stream
>
> <details>
> <summary>Answer</summary>
> **A) Connects two streams together** - It pipes data from a readable to a writable stream automatically!
> </details>

---

## HTTP Server

### Basic Server

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello, World!");
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

### Request Object

```javascript
const server = http.createServer((req, res) => {
  // Method
  console.log(req.method); // GET, POST, etc.

  // URL
  console.log(req.url); // /api/users

  // Headers
  console.log(req.headers["content-type"]);

  // Body (for POST/PUT)
  let body = "";
  req.on("data", (chunk) => (body += chunk));
  req.on("end", () => {
    console.log(JSON.parse(body));
  });
});
```

### Response Methods

```javascript
const server = http.createServer((req, res) => {
  // JSON response
  res.writeHead(200, {
    "Content-Type": "application/json",
  });
  res.end(JSON.stringify({ message: "success" }));

  // HTML response
  res.writeHead(200, {
    "Content-Type": "text/html",
  });
  res.end("<h1>Hello</h1>");

  // Status codes
  res.writeHead(404, "Not Found");
  res.writeHead(500, "Internal Server Error");
  res.writeHead(301, "Moved Permanently", {
    Location: "/new-location",
  });
});
```

### Simple Routing

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  const { url, method } = req;

  if (url === "/" && method === "GET") {
    res.end("Home Page");
  } else if (url === "/about" && method === "GET") {
    res.end("About Page");
  } else if (url === "/api/users" && method === "GET") {
    res.writeHead(200, { "Content-Type": "application/json" });
    res.end(JSON.stringify([{ id: 1, name: "John" }]));
  } else {
    res.writeHead(404);
    res.end("Not Found");
  }
});

server.listen(3000);
```

---

## NPM

### Package.json

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  }
}
```

### Commands

```bash
# Initialize package.json
npm init -y

# Install package
npm install express
npm install express --save
npm install express --save-dev  # dev dependency

# Install all dependencies
npm install
npm ci  # clean install (CI/CD)

# Run scripts
npm run start
npm run dev

# List packages
npm list
npm list --depth=0
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What's the difference between `--save` and `--save-dev`?
>
> - [ ] A) They're the same
> - [ ] B) save-dev is for production, save is for development
> - [ ] C) save-dev is for dev tools (testing, linting), save is for runtime
>
> <details>

> <summary>Answer</summary>
> **C) save-dev is for dev tools, save is for runtime** - devDependencies are not installed in production!
> </details>

### npx

```bash
# Run local binary
npx nodemon index.js

# Run package without installing
npx create-react-app my-app
```

---

## Projects

### ⭐ Project 1: File Manager CLI

Build a command-line tool:

- [ ] List files in directory
- [ ] Create/delete files
- [ ] Copy/move files (using fs)
- [ ] Show file info

---

### ⭐ Project 2: Static File Server

Create a server that:

- [ ] Serves static files
- [ ] Handles MIME types
- [ ] Supports common routes
- [ ] Logs requests

---

### ⭐ Project 3: Todo API

Build an API with:

- [ ] GET /todos - list todos
- [ ] POST /todos - create todo
- [ ] PUT /todos/:id - update todo
- [ ] DELETE /todos/:id - delete todo
- [ ] Store in memory (or JSON file)

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between require() and import?
2. [ ] How do you read a file asynchronously in Node.js?
3. [ ] What are EventEmitters used for?
4. [ ] How do streams help with large files?
5. [ ] What's the difference between dependencies and devDependencies?
6. [ ] How do you create a basic HTTP server?

<details>
<summary>Click to reveal all answers</summary>

1. require = CommonJS (default), import = ES Modules (need "type": "module")
2. Use fs.readFile() with a callback, or fs.promises.readFile() with await
3. Emit and handle custom events in your application
4. They process data in chunks instead of loading everything into memory
5. dependencies = runtime (express), devDependencies = development only (nodemon)
6. Use http.createServer() with a callback that handles req/res

</details>

---

## Resources

- [Node.js Docs](https://nodejs.org/docs/)
- [Node.js Guide](https://nodejs.dev/learn)
- [npm Docs](https://docs.npmjs.com)

---

## Progress Checklist

- [ ] Node.js installed
- [ ] Read through all sections
- [ ] Completed Project 1 (File Manager CLI)
- [ ] Completed Project 2 (Static File Server)
- [ ] Answered all quiz questions
- [ ] Ready for Module 11: Express.js

---

## Next Module

[Module 11: Express.js →](./11-express.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Understanding For Every Developer_
