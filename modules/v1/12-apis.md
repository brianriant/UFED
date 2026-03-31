# Module 12: REST APIs

**Duration:** Weeks 21-22 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** REST is about conventions and best practices, not strict rules! Follow the patterns consistently.

---

## Overview

REST (Representational State Transfer) is an architectural style for designing networked applications. This module covers REST API design principles and best practices.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand REST principles
- [ ] Design RESTful endpoints
- [ ] Implement API versioning
- [ ] Handle pagination and filtering
- [ ] Document APIs

---

## Table of Contents

1. [REST Principles](#principles)
2. [API Design](#design)
3. [Best Practices](#best-practices)
4. [Documentation](#documentation)
5. [Testing](#testing)
6. [Security](#security)
7. [Projects](#projects)

---

## 1. REST Principles

### Core Principles

| Principle             | Description                 |
| --------------------- | --------------------------- |
| **Client-Server**     | Separation of concerns      |
| **Stateless**         | No client context on server |
| **Cacheable**         | Responses can be cached     |
| **Uniform Interface** | Consistent resource paths   |
| **Layered System**    | Scalable architecture       |

### Resources and Endpoints

```
Resources (nouns)       →      Endpoints
─────────────────────────────────────────────
Users                   →      GET /users
                        →      POST /users

Single User             →      GET /users/:id
                        →      PUT /users/:id
                        →      DELETE /users/:id

User's Posts            →      GET /users/:id/posts
                        →      POST /users/:id/posts

Single Post             →      GET /posts/:id
                        →      PUT /posts/:id
                        →      DELETE /posts/:id
```

> [!NOTE]
> Use nouns for resources, not verbs! Use HTTP methods for actions.

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What HTTP method should you use to create a new resource?
>
> - [ ] A) GET
> - [ ] B) POST
> - [ ] C) PUT
>
> <details>
> <summary>Answer</summary>
> **B) POST** - POST is used to create new resources!
> </details>

---

## 2. API Design

### HTTP Methods

| Method | Purpose          | Idempotent |
| ------ | ---------------- | ---------- |
| GET    | Read resource    | ✅ Yes     |
| POST   | Create resource  | ❌ No      |
| PUT    | Replace resource | ✅ Yes     |
| PATCH  | Partial update   | ❌ No      |
| DELETE | Remove resource  | ✅ Yes     |

> [!WARNING]
> Idempotent means calling the same method multiple times has the same result!

### Status Codes

```javascript
// 2xx Success
200 OK                 // GET, PUT, PATCH success
201 Created           // POST resource created
204 No Content        // DELETE success

// 4xx Client Errors
400 Bad Request        // Invalid input
401 Unauthorized       // Not authenticated
403 Forbidden         // No permission
404 Not Found         // Resource doesn't exist
409 Conflict          // Duplicate/validation error
422 Unprocessable      // Valid but can't process

// 5xx Server Errors
500 Internal Server   // Unexpected error
503 Service Unavailable
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What status code should you return for a successful DELETE?
>
> - [ ] A) 200
> - [ ] B) 204
> - [ ] C) 201
>
> <details>
> <summary>Answer</summary>
> **B) 204 No Content** - DELETE typically returns 204 as there's no content to send back!
> </details>

### Response Formats

```json
// Success response
{
  "success": true,
  "data": {
    "id": 1,
    "name": "John",
    "email": "john@example.com"
  }
}

// Error response
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      { "field": "email", "message": "Must be a valid email" }
    ]
  }
}

// List response with pagination
{
  "success": true,
  "data": [
    { "id": 1, "name": "John" }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "pages": 10
  }
}
```

---

## 3. Best Practices

### URL Design

```javascript
// ❌ Bad - using verbs
GET / getUsers;
POST / createUser;
GET / users / 1 / getPosts;

// ✅ Good - using nouns
GET / users;
POST / users;
GET / users / 1 / posts;

// Use plurals
GET / users; // Not /user
GET / posts; // Not /post

// Use kebab-case for multi-word
GET / user - profiles;
POST / order - items;
```

### Versioning

```javascript
// URL versioning (most common)
GET /api/v1/users
GET /api/v2/users

// Header versioning
GET /users
Accept: application/vnd.myapp.v2+json

// Query versioning
GET /users?version=2
```

> [!TIP]
> URL versioning (`/api/v1/`) is the most common and easiest to understand!

### Filtering and Sorting

```javascript
// Filtering
GET /users?status=active
GET /users?role=admin&status=active

// Sorting
GET /users?sort=name
GET /users?sort=-createdAt  // descending

// Combined
GET /users?status=active&sort=-createdAt&limit=10&page=1
```

### Pagination

```javascript
// Offset-based (good for small datasets)
GET /users?page=1&limit=10

// Cursor-based (better for large datasets)
GET /users?cursor=abc123&limit=10
```

---

## 4. Documentation

### OpenAPI/Swagger Example

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0

paths:
  /users:
    get:
      summary: Get all users
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        "200":
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      type: object
                      properties:
                        id:
                          type: integer
                        name:
                          type: string
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** Why is API documentation important?
>
> - [ ] A) It's required by law
> - [ ] B) Helps developers understand how to use your API
> - [ ] C) Makes the API faster
>
> <details>
> <summary>Answer</summary>
> **B) Helps developers understand how to use your API** - Good docs = easier integration!
> </details>

---

## 5. Testing

### Unit Tests with Jest

```javascript
const request = require("supertest");
const app = require("../app");

describe("User API", () => {
  describe("GET /api/users", () => {
    it("should return all users", async () => {
      const res = await request(app).get("/api/users").expect(200);

      expect(res.body.data).toBeInstanceOf(Array);
    });
  });

  describe("POST /api/users", () => {
    it("should create a new user", async () => {
      const newUser = { name: "John", email: "john@example.com" };

      const res = await request(app)
        .post("/api/users")
        .send(newUser)
        .expect(201);

      expect(res.body.data.name).toBe("John");
    });

    it("should return 400 for invalid input", async () => {
      const res = await request(app)
        .post("/api/users")
        .send({ name: "John" }) // missing email
        .expect(400);

      expect(res.body.success).toBe(false);
    });
  });
});
```

> [!TIP]
> Use `supertest` to test your Express routes without starting a real server!

---

## 6. Security

### Essential Security Practices

```javascript
// CORS - control who can access your API
const cors = require("cors");
app.use(
  cors({
    origin: ["https://example.com"],
    credentials: true,
  }),
);

// Rate limiting - prevent abuse
const rateLimit = require("express-rate-limit");
app.use(
  rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per window
  }),
);

// Helmet - security headers
const helmet = require("helmet");
app.use(helmet());
```

### Input Validation

```javascript
const Joi = require("joi");

const userSchema = Joi.object({
  name: Joi.string().min(2).max(50).required(),
  email: Joi.string().email().required(),
  age: Joi.number().integer().min(0).max(150),
  password: Joi.string().min(8).required(),
});

app.post("/api/users", (req, res, next) => {
  const { error, value } = userSchema.validate(req.body);

  if (error) {
    return res.status(400).json({
      success: false,
      error: error.details,
    });
  }

  next();
});
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What's the purpose of rate limiting?
>
> - [ ] A) Make API faster
> - [ ] B) Prevent abuse and protect against DDoS
> - [ ] C) Cache responses
>
> <details>
> <summary>Answer</summary>
> **B) Prevent abuse and protect against DDoS** - Rate limiting prevents too many requests from one source!
> </details>

---

## 7. Projects

### ⭐ Project 1: E-commerce API

Design an API for:

- [ ] Products (full CRUD)
- [ ] Categories
- [ ] Orders
- [ ] Cart management

---

### ⭐ Project 2: Social Media API

Build an API with:

- [ ] Posts (CRUD)
- [ ] Comments on posts
- [ ] Likes/unlikes
- [ ] Followers/following
- [ ] Notifications

---

### ⭐ Project 3: Real-time Features

Add to any API:

- [ ] WebSocket support for real-time updates
- [ ] Event notifications
- [ ] Live updates when data changes

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What are the 5 REST principles?
2. [ ] What's the difference between PUT and PATCH?
3. [ ] How do you implement pagination?
4. [ ] What security measures should you implement?
5. [ ] What's the purpose of OpenAPI/Swagger?
6. [ ] How do you handle errors consistently in a REST API?

<details>
<summary>Click to reveal all answers</summary>

1. Client-Server, Stateless, Cacheable, Uniform Interface, Layered System
2. PUT replaces entire resource, PATCH updates partial fields
3. Use page/limit query params or cursor-based pagination
4. CORS, rate limiting, input validation, authentication, HTTPS
5. Standard API documentation format that describes endpoints, parameters, responses
6. Use consistent error response format with success flag, error code, message, and details

</details>

---

## Resources

- [REST API Tutorial](https://restfulapi.net/)
- [JSON:API](https://jsonapi.org/) - API specification
- [OpenAPI Initiative](https://www.openapis.org/)
- [Swagger UI](https://swagger.io/tools/swagger-ui/)

---

## Progress Checklist

- [ ] Read through all sections
- [ ] Completed Project 1 (E-commerce API)
- [ ] Completed Project 2 (Social Media API)
- [ ] Answered all quiz questions
- [ ] 🎉 Congratulations! UFED curriculum complete!

---

## 🎉 Congratulations!

You've completed the UFED curriculum! Here's what's next:

### Build Your Capstone Project

Combine everything you've learned into a full-stack application!

### Keep Learning

- GraphQL
- WebSockets
- Microservices
- Docker & Kubernetes
- CI/CD
- Testing (Jest, Cypress)

### Practice Daily

- Build real projects
- Read code from open source
- Join developer communities

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
_Happy Coding! 🚀_
