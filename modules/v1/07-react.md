# Module 7: React

**Duration:** Weeks 11-13 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** React is all about components! Think of your UI as a tree of reusable pieces.

---

## Overview

React is a JavaScript library for building user interfaces. It uses a component-based architecture and virtual DOM for efficient updates.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand React fundamentals
- [ ] Build reusable components
- [ ] Manage state with hooks
- [ ] Handle routing with React Router
- [ ] Use context for global state

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Components](#components)
3. [JSX](#jsx)
4. [Props and State](#props-state)
5. [Hooks](#hooks)
6. [React Router](#router)
7. [Context API](#context)
8. [Best Practices](#best-practices)
9. [Projects](#projects)

---

## 1. Getting Started

### Installation

```bash
# Create React app
npx create-react-app my-app

# Or better - use Vite (faster!)
npm create vite@latest my-app -- --template react

# Navigate to directory
cd my-app

# Start development server
npm run dev
```

> [!TIP]
> Vite is much faster than Create React App! It uses native ES modules.

### Project Structure

```
my-app/
├── src/
│   ├── components/
│   │   └── Button.jsx
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
├── public/
├── package.json
└── vite.config.js
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What's the main advantage of Vite over Create React App?
>
> - [ ] A) Better React features
> - [ ] B) Faster development server and build
> - [ ] C) More templates
>
> <details>
> <summary>Answer</summary>
> **B) Faster development server and build** - Vite uses native ES modules and is significantly faster!
> </details>

---

## 2. Components

### Function Components (Recommended!)

```jsx
// Basic component
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// Component with props
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Arrow function component
const Welcome = ({ name }) => <h1>Hello, {name}!</h1>;

// Usage
<Welcome name="John" />;
```

### Class Components (Legacy - Avoid)

```jsx
// Old way - rarely used now
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

> [!WARNING]
> ⚠️ Class components still work but function components with hooks are the modern standard!

### Component Composition

```jsx
function App() {
  return (
    <div>
      <Header />
      <main>
        <Sidebar />
        <Content />
      </main>
      <Footer />
    </div>
  );
}
```

> [!NOTE]
> Think of components like LEGO blocks - small, reusable pieces that fit together!

---

## 3. JSX

### JSX Rules

```jsx
// Must return single root element
// ❌ Invalid
return (
  <h1>Title</h1>
  <p>Description</p>
);

// ✅ Valid - use fragment
return (
  <>
    <h1>Title</h1>
    <p>Description</p>
  </>
);

// Or use a wrapper
return <div>
  <h1>Title</h1>
  <p>Description</p>
</div>;
```

### Expressions in JSX

```jsx
const name = "John";
const isLoggedIn = true;

return (
  <div>
    {/* Variable */}
    <h1>Hello, {name}!</h1>

    {/* Ternary */}
    {isLoggedIn ? <Dashboard /> : <Login />}

    {/* Logical AND */}
    {isLoggedIn && <WelcomeMessage />}

    {/* Function call */}
    <p>{formatDate(new Date())}</p>

    {/* Style object */}
    <div style={{ color: "blue", fontSize: "16px" }}>Text</div>
  </div>
);
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What will `{isLoggedIn && <Welcome />}` render if isLoggedIn is false?
>
> - [ ] A) Nothing (renders nothing)
> - [ ] B) A blank component
> - [ ] C) An error
>
> <details>
> <summary>Answer</summary>
> **A) Nothing** - The && returns false, which React ignores!
> </details>

### Styling

```jsx
// Inline styles
<div
  style={{
    color: "blue",
    fontSize: "16px",
  }}
>
  Content
</div>;

// CSS modules (imported separately)
import styles from "./Button.module.css";
<button className={styles.button}>Click</button>;

// Regular CSS
import "./App.css";
<div className="container">Content</div>;
```

---

## 4. Props and State

### Props (Read-Only Data)

```jsx
// Passing props
<Button variant="primary" onClick={handleClick} disabled={false}>
  Click Me
</Button>;

// Receiving props
function Button({ children, variant, onClick, disabled }) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
}

// Default props
Button.defaultProps = {
  variant: "primary",
  disabled: false,
};
```

> [!NOTE]
> Props are passed from parent to child - think of them like function arguments!

### State (Dynamic Data)

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

> [!TIP]
> `useState` returns an array: `[currentValue, setterFunction]`

### Multiple State Values

```jsx
function Form() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [isSubmitting, setIsSubmitting] = useState(false);

  // or use object
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    isSubmitting: false,
  });

  return <form>...</form>;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** How do you update state based on previous state?
>
> - [ ] A) `setCount(count + 1)`
> - [ ] B) `setCount(prev => prev + 1)`
> - [ ] C) Both work, but B is safer
>
> <details>
> <summary>Answer</summary>
> **C) Both work, but B is safer** - Use the function form when the update depends on previous state to avoid race conditions!
> </details>

---

## 5. Hooks

### useState

```jsx
const [state, setState] = useState(initialValue);

// Functional updates (safer!)
setCount((prev) => prev + 1);

// Lazy initialization (for expensive computations)
const [data, setData] = useState(() => {
  const saved = localStorage.getItem("data");
  return saved ? JSON.parse(saved) : defaultData;
});
```

### useEffect (Side Effects)

```jsx
import { useEffect, useState } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  // Run on every render
  useEffect(() => {
    console.log("Effect ran");
  });

  // Run only on mount + when userId changes
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // dependency array

  // Run on mount, cleanup on unmount
  useEffect(() => {
    const subscription = subscribe(id);
    return () => subscription.unsubscribe(); // cleanup!
  }, []);

  return <div>{user?.name}</div>;
}
```

> [!WARNING]
> ⚠️ Missing the dependency array can cause infinite loops or stale data!

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What does an empty dependency array `[]` do?
>
> - [ ] A) Run on every render
> - [ ] B) Run only on mount and unmount
> - [ ] C) Never run
>
> <details>
> <summary>Answer</summary>
> **B) Run only on mount and unmount** - It's like saying "run once"!
> </details>

### useContext (Global State)

```jsx
// AuthContext.jsx
import { createContext, useContext, useState } from "react";

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}
```

### useRef (Mutable Ref)

```jsx
function InputFocus() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus</button>
    </>
  );
}
```

> [!NOTE]
> useRef doesn't trigger re-renders! It's for persisting values across renders.

### useReducer (Complex State)

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </>
  );
}
```

### Custom Hooks

```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const saved = localStorage.getItem(key);
    return saved ? JSON.parse(saved) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Usage
function App() {
  const [name, setName] = useLocalStorage("name", "");
  return <input value={name} onChange={(e) => setName(e.target.value)} />;
}
```

---

## 6. React Router

### Installation

```bash
npm install react-router-dom
```

### Setup

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users/123">User</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<User />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Navigation

```jsx
import { useNavigate } from "react-router-dom";

function LoginButton() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // ... login logic
    navigate("/dashboard");
  };

  return <button onClick={handleLogin}>Login</button>;
}
```

### Route Parameters

```jsx
import { useParams } from "react-router-dom";

function User() {
  const { id } = useParams();
  return <h1>User ID: {id}</h1>;
}
```

---

## 7. Context API (Review)

### Creating Context (Full Example)

```jsx
// AuthContext.jsx
import { createContext, useContext, useState } from "react";

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  return useContext(AuthContext);
}
```

### Using in Components

```jsx
function Dashboard() {
  const { user, logout } = useAuth();

  return (
    <div>
      <p>Welcome, {user.name}</p>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

---

## 8. Best Practices

### Component Guidelines

```jsx
// ✅ Good: Small, focused components
function UserAvatar({ user }) {
  return <img src={user.avatar} alt={user.name} />;
}

// ❌ Bad: Large, monolithic components
function UserProfile() {
  // 500 lines...
}

// Extract logic to custom hooks
function useSearch(items) {
  const [query, setQuery] = useState("");
  const results = items.filter((item) =>
    item.name.toLowerCase().includes(query.toLowerCase()),
  );
  return { query, setQuery, results };
}
```

### Performance

```jsx
import { useMemo, useCallback } from "react";

// Memoize expensive calculations
const expensiveValue = useMemo(() => {
  return data.map((item) => transform(item));
}, [data]);

// Memoize callback functions
const handleClick = useCallback((id) => {
  setSelected(id);
}, []);

// Memoize components
const MemoizedComponent = React.memo(function MyComponent({ data }) {
  return <div>{data.title}</div>;
});
```

### File Organization

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.css
│   │   └── index.js
│   └── Input/
├── hooks/
│   ├── useLocalStorage.js
│   └── useFetch.js
├── context/
│   └── AuthContext.jsx
├── pages/
│   ├── Home.jsx
│   └── About.jsx
└── utils/
    └── api.js
```

---

## 9. Projects

### ⭐ Project 1: Todo App

Build a todo application:

- [ ] Add new todo
- [ ] Mark complete/incomplete (toggle)
- [ ] Delete todo
- [ ] Filter by status (all/active/completed)
- [ ] Persist to localStorage

> [!TIP]
> **Try it live:** [Open in StackBlitz](https://stackblitz.com/fork/react)

---

### ⭐ Project 2: Blog Dashboard

Create a blog admin with:

- [ ] List posts with cards
- [ ] Create/Edit post form with validation
- [ ] Delete with confirmation modal
- [ ] Loading states and error handling

---

### ⭐ Project 3: E-commerce Product Page

Build a product page:

- [ ] Product details display (image, price, description)
- [ ] Add to cart functionality
- [ ] Quantity selector
- [ ] Related products section

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between props and state?
2. [ ] When does useEffect run?
3. [ ] How do you prevent unnecessary re-renders?
4. [ ] What's the purpose of useCallback and useMemo?
5. [ ] How do you handle routing in React?
6. [ ] What's the benefit of custom hooks?
7. [ ] When would you use useReducer instead of useState?

<details>
<summary>Click to reveal all answers</summary>

1. Props = passed from parent (immutable), State = managed internally (mutable)
2. Depends on dependency array - every render (no array), mount (empty array), mount + changes (with dependencies)
3. Use React.memo, useMemo, useCallback, or restructure components
4. useCallback memoizes functions, useMemo memoizes computed values
5. Use react-router-dom with BrowserRouter, Routes, Route, and Link
6. Reusable logic - extract stateful logic into custom functions
7. Complex state logic, multiple sub-values, or when you'd use a switch statement

</details>

---

## Resources

- [React Docs](https://react.dev)
- [React Hooks Reference](https://react.dev/reference/react)
- [React Router Docs](https://reactrouter.com)
- [React Tutorial - FreeCodeCamp](https://www.freecodecamp.org/learn/front-end-development-libraries/)

---

## Progress Checklist

- [ ] React project created with Vite
- [ ] Read through all sections
- [ ] Completed Project 1 (Todo App)
- [ ] Completed Project 2 (Blog Dashboard)
- [ ] Answered all quiz questions
- [ ] Ready for Module 8: Next.js

---

## Next Module

[Module 8: Next.js →](../modules/08-nextjs.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
