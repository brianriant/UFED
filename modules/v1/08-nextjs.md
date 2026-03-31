# Module 8: Next.js

**Duration:** Weeks 14-15 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** Next.js extends React with powerful features. Think of it as React + server-side superpowers!

---

## Overview

Next.js is a React framework that provides server-side rendering, static site generation, and other performance features out of the box.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand Next.js architecture
- [ ] Use file-based routing
- [ ] Implement SSR and SSG
- [ ] Work with API routes
- [ ] Optimize performance

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Routing](#routing)
3. [Data Fetching](#data-fetching)
4. [API Routes](#api-routes)
5. [Styling](#styling)
6. [Optimization](#optimization)
7. [Deployment](#deployment)
8. [Projects](#projects)

---

## 1. Getting Started

### Installation

```bash
# Create Next.js app (with App Router)
npx create-next-app@latest my-app

# Or with TypeScript
npx create-next-app@latest my-app --typescript

# Navigate
cd my-app

# Start development server
npm run dev
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What routing system does the Next.js App Router use?
>
> - [ ] A) React Router
> - [ ] B) File-based routing (folders in app/)
> - [ ] C) Dynamic routing
>
> <details>
> <summary>Answer</summary>
> **B) File-based routing** - Create folders in `app/` and they become routes automatically!
> </details>

### Project Structure

```
my-app/
├── src/
│   ├── app/              # App Router (Next.js 13+)
│   │   ├── page.tsx      # Home page
│   │   ├── layout.tsx    # Root layout
│   │   ├── globals.css   # Global styles
│   │   └── api/          # API routes
│   ├── components/       # React components
│   ├── lib/             # Utilities
│   └── styles/          # CSS modules
├── public/              # Static files
├── package.json
└── next.config.js
```

---

## 2. Routing

### App Router Basics

```tsx
// app/page.tsx - Home page
export default function Home() {
  return <h1>Welcome to Next.js</h1>;
}

// app/about/page.tsx - About page
export default function About() {
  return <h1>About Us</h1>;
}

// app/blog/[slug]/page.tsx - Dynamic route
export default function BlogPost({ params }: { params: { slug: string } }) {
  return <h1>Post: {params.slug}</h1>;
}
```

### Route Parameters

```tsx
// app/users/[id]/page.tsx
export default function UserProfile({ params }: { params: { id: string } }) {
  return <h1>User {params.id}</h1>;
}

// app/products/[category]/[id]/page.tsx
export default function Product({
  params,
}: {
  params: { category: string; id: string };
}) {
  return (
    <h1>
      {params.category} - {params.id}
    </h1>
  );
}
```

### Navigation

```tsx
import Link from "next/link";
import { useRouter } from "next/navigation";

function Navbar() {
  const router = useRouter();

  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <button onClick={() => router.push("/dashboard")}>Dashboard</button>
    </nav>
  );
}
```

### Layouts

```tsx
// app/layout.tsx - Root layout (wraps everything)
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <header>My App</header>
        {children}
        <footer>© 2024</footer>
      </body>
    </html>
  );
}

// app/dashboard/layout.tsx - Nested layout
export default function DashboardLayout({ children }) {
  return (
    <div className="dashboard">
      <aside>Sidebar</aside>
      <main>{children}</main>
    </div>
  );
}
```

> [!WARNING]
> ⚠️ Root layout is required! It must include `<html>` and `<body>` tags.

### Route Groups

```tsx
// Grouping without affecting URL
// app/(marketing)/page.tsx

// app/(marketing)/layout.tsx
// Shared layout for marketing pages
```

---

## 3. Data Fetching

### Server Components (Default!)

```tsx
// app/page.tsx - Server component by default
async function getData() {
  const res = await fetch("https://api.example.com/data");
  return res.json();
}

export default async function Page() {
  const data = await getData();
  return <div>{data.title}</div>;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What type of component are pages in the App Router by default?
>
> - [ ] A) Client component
> - [ ] B) Server component
> - [ ] C) Static component
>
> <details>
> <summary>Answer</summary>
> **B) Server component** - They run on the server by default! Add 'use client' for interactivity.
> </details>

### Data Caching

```tsx
// Static (default, cached forever)
async function getPost(slug: string) {
  const res = await fetch(`https://api.example.com/posts/${slug}`, {
    cache: "force-cache",
  });
  return res.json();
}

// Dynamic (re-render on every request)
async function getData() {
  const res = await fetch("https://api.example.com/data", {
    cache: "no-store",
  });
  return res.json();
}

// ISR - Revalidate every 60 seconds
async function getData() {
  const res = await fetch("https://api.example.com/data", {
    next: { revalidate: 60 },
  });
  return res.json();
}
```

### Client-Side Fetching

```tsx
"use client"; // Must be client component!

import { useEffect, useState } from "react";

export default function ClientComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("/api/data")
      .then((res) => res.json())
      .then(setData);
  }, []);

  return <div>{data?.title}</div>;
}
```

> [!NOTE]
> Use `'use client'` for components that need: useState, useEffect, event handlers, browser APIs

---

## 4. API Routes

### Route Handlers

```tsx
// app/api/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({ message: "Hello" });
}

export async function POST(request: Request) {
  const body = await request.json();
  return NextResponse.json({ success: true, data: body });
}
```

### Dynamic Routes

```tsx
// app/api/users/[id]/route.ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } },
) {
  const user = await fetchUser(params.id);
  return NextResponse.json(user);
}

export async function PUT(
  request: Request,
  { params }: { params: { id: string } },
) {
  const body = await request.json();
  const updated = await updateUser(params.id, body);
  return NextResponse.json(updated);
}

export async function DELETE(
  request: Request,
  { params }: { params: { id: string } },
) {
  await deleteUser(params.id);
  return NextResponse.json({ success: true });
}
```

### Request Helpers

```tsx
// Query parameters
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const page = searchParams.get("page");
  const limit = searchParams.get("limit");

  return NextResponse.json({ page, limit });
}

// Headers
export async function GET(request: Request) {
  const token = request.headers.get("authorization");
  return NextResponse.json({ token });
}
```

---

## 5. Styling

### CSS Modules

```css
/* components/Button.module.css */
.button {
  padding: 8px 16px;
  background: blue;
  color: white;
  border-radius: 4px;
}
```

```tsx
// components/Button.tsx
import styles from "./Button.module.css";

export default function Button({ children }) {
  return <button className={styles.button}>{children}</button>;
}
```

### Global CSS

```css
/* app/globals.css */
:root {
  --primary: blue;
}

* {
  box-sizing: border-box;
}
```

```tsx
// app/layout.tsx
import "./globals.css";

export default function RootLayout({ children }) {
  return <body>{children}</body>;
}
```

---

## 6. Optimization

### Images

```tsx
import Image from "next/image";

export default function Hero() {
  return (
    <Image
      src="/hero.jpg"
      alt="Hero"
      width={800}
      height={400}
      priority // Load immediately
    />
  );
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What's the benefit of using the Next.js Image component?
>
> - [ ] A) Better colors
> - [ ] B) Automatic optimization, lazy loading, and size optimization
> - [ ] C) Smaller file size
>
> <details>
> <summary>Answer</summary>
> **B) Automatic optimization, lazy loading, and size optimization** - Next.js automatically serves the right size for each device!
> </details>

### Dynamic Imports

```tsx
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("./HeavyComponent"), {
  loading: () => <p>Loading...</p>,
  ssr: false, // Disable SSR for client-only
});
```

### Metadata

```tsx
import { Metadata } from "next";

export const metadata: Metadata = {
  title: "My Page",
  description: "Page description",
  openGraph: {
    title: "My Page",
    description: "Page description",
    images: ["/og-image.jpg"],
  },
};

export default function Page() {
  return <h1>Content</h1>;
}
```

---

## 7. Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

### Environment Variables

```env
# .env.local (development)
DATABASE_URL=postgres://...

# .env.production
DATABASE_URL=postgres://prod...
```

```tsx
// Access in code
const apiKey = process.env.API_KEY;
```

---

## 8. Projects

### ⭐ Project 1: Blog Platform

Build a blog with:

- [ ] Server-side rendered posts
- [ ] Static categories page with ISR
- [ ] API for post comments
- [ ] SEO metadata for each post

---

### ⭐ Project 2: E-commerce Store

Create a store with:

- [ ] Product listing (SSG with ISR)
- [ ] Product detail pages (SSR)
- [ ] Shopping cart with localStorage
- [ ] Checkout API route

---

### ⭐ Project 3: Dashboard

Build an admin dashboard with:

- [ ] Protected routes
- [ ] Data visualization (charts)
- [ ] API integration
- [ ] Responsive layout

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between SSR and SSG?
2. [ ] How do you make a component client-side?
3. [ ] What are route groups used for?
4. [ ] How do you add dynamic metadata?
5. [ ] What's the purpose of ISR?
6. [ ] How do API routes work in Next.js?

<details>
<summary>Click to reveal all answers</summary>

1. SSR = render on each request, SSG = render at build time (cached)
2. Add 'use client' at the top of the file
3. Group routes without affecting the URL (folders in parentheses)
4. Export metadata from the page
5. Incremental Static Regeneration - update static pages without rebuilding
6. Create route.ts in app/api/ folder - they work like Express routes

</details>

---

## Resources

- [Next.js Docs](https://nextjs.org/docs)
- [App Router Tutorial](https://nextjs.org/docs/app)
- [Vercel Deployment](https://vercel.com/docs)

---

## Progress Checklist

- [ ] Next.js project created
- [ ] Read through all sections
- [ ] Completed Project 1 (Blog)
- [ ] Answered all quiz questions
- [ ] Ready for Module 9: Tailwind CSS

---

## Next Module

[Module 9: Tailwind CSS →](../modules/09-tailwind.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
