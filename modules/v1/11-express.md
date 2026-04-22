# Module 11: Express.js

**Duration:** Weeks 19-20 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** Express makes building servers much easier than raw Node.js! It handles routing, middleware, and requests so you can focus on your app logic.

---

## Overview

Express.js is a minimal and flexible Node.js web application framework that provides robust features for web and mobile applications.

## Learning Objectives

By the end of this module, you will:

- [ ] Build REST APIs with Express
- [ ] Handle routing and middleware
- [ ] Work with request/response
- [ ] Implement error handling
- [ ] Connect to databases

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Routing](#routing)
3. [Middleware](#middleware)
4. [Request/Response](#request-response)
5. [Error Handling](#error-handling)
6. [Database Integration](#database-integration)
7. [Authentication](#authentication)
8. [Projects](#projects)

---

## Getting Started

### Installation

```bash
# Create project
mkdir my-api && cd my-api
npm init -y

# Install Express
npm install express

# Install dev dependencies
npm install -D nodemon
```

### Basic Server

```javascript
const express = require("express");
const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
  res.send("Hello, Express!");
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What does `app.listen()` return?
>
> - [ ] A) The server instance
> - [ ] B) Nothing
> - [ ] C) The port number
>
> <details>

> <summary>Answer</summary>
> **A) The server instance** - You can store it in a variable to use later (e.g., for websocket)!
> </details>

### Scripts

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
}
```

---

## Routing

### Basic Routes

```javascript
const express = require("express");
const app = express();

// GET - Read
app.get("/api/users", (req, res) => {
  res.json([{ id: 1, name: "John" }]);
});

// POST - Create
app.post("/api/users", (req, res) => {
  res.status(201).json({ message: "User created" });
});

// PUT - Update (full)
app.put("/api/users/:id", (req, res) => {
  res.json({ message: "User updated" });
});

// DELETE - Delete
app.delete("/api/users/:id", (req, res) => {
  res.json({ message: "User deleted" });
});

// PATCH - Partial update
app.patch("/api/users/:id", (req, res) => {
  res.json({ message: "User partially updated" });
});
```

### Route Parameters

```javascript
// URL parameters
app.get("/api/users/:id", (req, res) => {
  const { id } = req.params;
  res.json({ userId: id });
});

// Multiple parameters
app.get("/api/users/:userId/posts/:postId", (req, res) => {
  const { userId, postId } = req.params;
  res.json({ userId, postId });
});

// Optional parameters
app.get("/api/users/:id?", (req, res) => {
  const { id } = req.params;
  if (id) {
    res.json({ userId: id });
  } else {
    res.json({ users: [] });
  }
});
```

### Query Parameters

```javascript
app.get("/api/search", (req, res) => {
  const { q, page = 1, limit = 10 } = req.query;

  res.json({
    query: q,
    page: Number(page),
    limit: Number(limit),
    results: [],
  });
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What's the difference between route params and query params?
>
> - [ ] A) They're the same
> - [ ] B) Params are in URL path (/users/:id), query is after ? (q=search)
> - [ ] C) Query params are required
>
> <details>
> <summary>Answer</summary>
> **B) Route params are part of the URL path, query params come after ?** - Example: `/api/users/123?page=1` - `123` is a param, `page` is a query.
> </details>

---

## Middleware

### What is Middleware?

```javascript
// Middleware is a function with access to req, res, and next
function logger(req, res, next) {
  console.log(`${req.method} ${req.url}`);
  next(); // Important! Call next middleware
}

// Apply middleware globally
app.use(logger);
```

### Built-in Middleware

```javascript
app.use(express.json()); // Parse JSON body
app.use(express.urlencoded()); // Parse form data
app.use(express.text()); // Parse text
app.use(express.raw()); // Parse binary
app.use(express.static("public")); // Serve static files
```

> [!WARNING]
> ⚠️ Parse middleware must come BEFORE your routes! Otherwise body will be undefined.

### Custom Middleware

```javascript
// Authentication middleware
function authenticate(req, res, next) {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }

  // Verify token...
  req.user = { id: 1, name: "John" };
  next();
}

// Validation middleware
function validateUser(req, res, next) {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: "Missing fields" });
  }

  next();
}

// Apply to specific routes
app.post("/api/users", authenticate, validateUser, (req, res) => {
  // Create user
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What does `next()` do in Express middleware?
>
> - [ ] A) Ends the request
> - [ ] B) Passes control to the next middleware/route
> - [ ] C) Retries the request
>
> <details>
> <summary>Answer</summary>
> **B) Passes control to the next middleware/route** - Without calling next(), the request hangs!
> </details>

## Request/Response

### Request Object

```javascript
app.get("/api/test", (req, res) => {
  // Properties
  req.params; // URL parameters (/:id)
  req.query; // Query string (?page=1)
  req.body; // Request body (needs express.json())
  req.headers; // Headers
  req.method; // HTTP method
  req.url; // URL path
  req.path; // Path without query
  req.ip; // Client IP
  req.protocol; // http or https
  req.hostname; // Host name
});
```

### Response Object

```javascript
app.get("/api/test", (req, res) => {
  // Send response
  res.send("Hello"); // Send text/HTML
  res.json({ message: "Hello" }); // Send JSON
  res.status(404).json({ error: "Not found" }); // Set status + JSON

  // Set headers
  res.set("Content-Type", "application/json");
  res.setHeader("X-Custom", "value");

  // Redirect
  res.redirect("/other-page");
  res.redirect(301, "/new-location");

  // Download
  res.download("/path/to/file.pdf");

  // Send file
  res.sendFile("/path/to/file.html");
});
```

---

## Error Handling

### Sync Error Handler

```javascript
app.get("/api/error", (req, res) => {
  throw new Error("Something went wrong!");
});
```

### Async Error Handler

```javascript
app.get("/api/users/:id", async (req, res, next) => {
  try {
    const user = await getUser(req.params.id);
    res.json(user);
  } catch (error) {
    next(error); // Pass to error handler
  }
});
```

### Error Middleware

```javascript
// 404 handler (at the end)
app.use((req, res) => {
  res.status(404).json({ error: "Not found" });
});

// Error handler (must be last!)
app.use((err, req, res, next) => {
  console.error(err.stack);

  res.status(err.status || 500).json({
    error: err.message || "Internal server error",
  });
});
```

### Custom Error Class

```javascript
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith("4") ? "fail" : "error";
    Error.captureStackTrace(this, this.constructor);
  }
}

app.get("/api/test", (req, res, next) => {
  throw new AppError("Not found", 404);
});
```

---

## Database Integration

### MongoDB with Mongoose

```javascript
const mongoose = require("mongoose");

mongoose
  .connect("mongodb://localhost:27017/mydb")
  .then(() => console.log("Connected to MongoDB"))
  .catch((err) => console.error("MongoDB error:", err));

// Define schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true },
  age: Number,
});

// Create model
const User = mongoose.model("User", userSchema);

// Routes
app.get("/api/users", async (req, res) => {
  const users = await User.find();
  res.json(users);
});

app.post("/api/users", async (req, res) => {
  const user = new User(req.body);
  await user.save();
  res.status(201).json(user);
});

app.get("/api/users/:id", async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    return res.status(404).json({ error: "User not found" });
  }
  res.json(user);
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What's the benefit of using Mongoose with MongoDB?
>
> - [ ] A) It's faster
> - [ ] B) Schema validation and easier queries
> - [ ] C) It works with SQL
>
> <details>
> <summary>Answer</summary>
> **B) Schema validation and easier queries** - Mongoose provides schema definitions, validation, and a cleaner API!
> </details>

## Authentication

### JWT Authentication

```javascript
const jwt = require("jsonwebtoken");

// Generate token
function generateToken(user) {
  return jwt.sign({ id: user.id, email: user.email }, process.env.JWT_SECRET, {
    expiresIn: "1d",
  });
}

// Verify token middleware
function verifyToken(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: "Invalid token" });
  }
}

// Protected route
app.get("/api/profile", verifyToken, (req, res) => {
  res.json({ user: req.user });
});
```

### Login Route

```javascript
const bcrypt = require("bcrypt");

app.post("/api/login", async (req, res) => {
  const { email, password } = req.body;

  // Find user
  const user = await User.findOne({ where: { email } });

  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).json({ error: "Invalid credentials" });
  }

  // Generate token
  const token = generateToken(user);

  res.json({ token, user: { id: user.id, name: user.name } });
});
```

---

## Projects

### ⭐ Project 1: RESTful Todo API

Build a complete API:

- [ ] CRUD operations for todos
- [ ] Validation middleware
- [ ] Error handling
- [ ] In-memory storage (use array or JSON file)

---

### ⭐ Project 2: User Management API

Create an API with:

- [ ] User registration with password hashing
- [ ] Login with JWT
- [ ] Protected routes
- [ ] Profile management

---

### ⭐ Project 3: Blog API

Build a blog backend:

- [ ] Posts CRUD
- [ ] Categories
- [ ] Comments
- [ ] Basic authentication

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] How do you handle POST request body in Express?
2. [ ] What's the difference between app.use() and app.get()?
3. [ ] How do you create a 404 handler?
4. [ ] What does the error middleware need 4 parameters?
5. [ ] How do you protect a route with authentication?
6. [ ] What's the difference between PUT and PATCH?

<details>
<summary>Click to reveal all answers</summary>

1. Use express.json() middleware and access req.body
2. app.use = applies to all methods, app.get = only GET requests
3. Add a middleware at the end that returns 404
4. Express identifies it as error middleware by the 4 parameters (err, req, res, next)
5. Create a middleware that verifies the token and adds user to req
6. PUT replaces the entire resource, PATCH updates partial fields

</details>

---

## Resources

- [Express.js Docs](https://expressjs.com)
- [Express Guide](https://expressjs.com/en/guide/)
- [Express Middleware](https://expressjs.com/en/resources/middleware.html)

---

## Progress Checklist

- [ ] Express installed
- [ ] Read through all sections
- [ ] Completed Project 1 (Todo API)
- [ ] Completed Project 2 (User API)
- [ ] Answered all quiz questions
- [ ] Ready for Module 12: REST APIs

---

## Next Module

[Module 12: REST APIs →](./12-apis.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Understanding For Every Developer_
