# Module 9: Tailwind CSS

**Duration:** Week 16 | **Difficulty:** Intermediate

---

> [!TIP]
> **Learning Tip:** Tailwind is different from other CSS frameworks—you compose styles directly in your HTML using utility classes!

---

## Overview

Tailwind CSS is a utility-first CSS framework that provides low-level utility classes to build custom designs without leaving your HTML.

## Learning Objectives

By the end of this module, you will:

- [ ] Master Tailwind utility classes
- [ ] Build responsive layouts
- [ ] Customize Tailwind configuration
- [ ] Use plugins and extensions
- [ ] Optimize for production

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Utility Classes](#utility-classes)
3. [Layout](#layout)
4. [Responsive Design](#responsive-design)
5. [Customization](#customization)
6. [Components](#components)
7. [Projects](#projects)

---

## Getting Started

### Installation

```bash
# With Vite
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# With Next.js (already included!)
npm install
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What's the main philosophy of Tailwind CSS?
>
> - [ ] A) Component-based styling
> - [ ] B) Utility-first, compose in HTML
> - [ ] C) CSS-in-JS
>
> <details>

> <summary>Answer</summary>
> **B) Utility-first, compose in HTML** - You build designs by combining utility classes directly in your JSX/HTML!
> </details>

### Configuration

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### CSS Setup

```css
/* index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## Utility Classes

### Spacing

```html
<!-- Margin -->
<div class="m-4">Margin 1rem</div>
<div class="mx-4">Margin x-axis</div>
<div class="my-4">Margin y-axis</div>
<div class="mt-4">Margin top</div>
<div class="mb-4">Margin bottom</div>
<div class="ml-4">Margin left</div>
<div class="mr-4">Margin right</div>

<!-- Padding -->
<div class="p-4">Padding 1rem</div>
<div class="px-4">Padding x-axis</div>
<div class="py-4">Padding y-axis</div>
```

> [!NOTE]
> Spacing scale: 0, 1(0.25rem), 2(0.5rem), 3(0.75rem), 4(1rem), 5(1.25rem), 6(1.5rem), etc.

### Sizing

```html
<!-- Width -->
<div class="w-4">Width 1rem</div>
<div class="w-full">Width 100%</div>
<div class="w-auto">Width auto</div>
<div class="w-1/2">Width 50%</div>
<div class="w-screen">Width 100vw</div>

<!-- Height -->
<div class="h-4">Height 1rem</div>
<div class="h-full">Height 100%</div>
<div class="h-screen">Height 100vh</div>

<!-- Min/Max -->
<div class="min-h-screen">min-height: 100vh</div>
<div class="max-w-lg">max-width: 32rem</div>
```

### Colors

```html
<!-- Text colors -->
<p class="text-red-500">Red text</p>
<p class="text-blue-600">Blue text</p>
<p class="text-gray-700">Gray text</p>

<!-- Background colors -->
<div class="bg-white">White background</div>
<div class="bg-slate-900">Dark background</div>
<div class="bg-opacity-50">50% opacity</div>
```

> [!TIP]
> Tailwind colors have shades: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900

### Typography

```html
<!-- Font size -->
<p class="text-xs">Extra small</p>
<p class="text-sm">Small</p>
<p class="text-base">Base</p>
<p class="text-lg">Large</p>
<p class="text-xl">Extra large</p>
<p class="text-2xl">2XL</p>

<!-- Font weight -->
<p class="font-light">Light</p>
<p class="font-normal">Normal</p>
<p class="font-medium">Medium</p>
<p class="font-semibold">Semibold</p>
<p class="font-bold">Bold</p>

<!-- Text alignment -->
<p class="text-left">Left</p>
<p class="text-center">Center</p>
<p class="text-right">Right</p>

<!-- Text transform -->
<p class="uppercase">UPPERCASE</p>
<p class="lowercase">lowercase</p>
<p class="capitalize">Capitalize</p>
```

### Borders

```html
<!-- Border width -->
<div class="border">Default border</div>
<div class="border-2">2px border</div>
<div class="border-4">4px border</div>
<div class="border-t">Top border</div>
<div class="border-b">Bottom border</div>

<!-- Border color -->
<div class="border-gray-300">Gray border</div>
<div class="border-red-500">Red border</div>

<!-- Border radius -->
<div class="rounded">Default</div>
<div class="rounded-lg">Large</div>
<div class="rounded-full">Full (pill)</div>
<div class="rounded-t-lg">Top rounded</div>
```

---

## Layout

### Flexbox

```html
<!-- Container -->
<div class="flex">Display flex</div>
<div class="inline-flex">Inline flex</div>

<!-- Direction -->
<div class="flex-row">Row (default)</div>
<div class="flex-row-reverse">Row reverse</div>
<div class="flex-col">Column</div>
<div class="flex-col-reverse">Column reverse</div>

<!-- Justify content -->
<div class="justify-start">Start</div>
<div class="justify-center">Center</div>
<div class="justify-end">End</div>
<div class="justify-between">Between</div>
<div class="justify-around">Around</div>
<div class="justify-evenly">Evenly</div>

<!-- Align items -->
<div class="items-start">Start</div>
<div class="items-center">Center</div>
<div class="items-end">End</div>
<div class="items-stretch">Stretch</div>

<!-- Gap -->
<div class="gap-4">Gap 1rem</div>
<div class="gap-x-4">Column gap</div>
<div class="gap-y-4">Row gap</div>
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** Which class centers items both horizontally and vertically in a flex container?
>
> - [ ] A) flex-center
> - [ ] B) justify-center items-center
> - [ ] C) center
>
> <details>
> <summary>Answer</summary>
> **B) justify-center items-center** - justify-center for horizontal, items-center for vertical!
> </details>

```html
<!-- Display grid -->
<div class="grid">Display grid</div>

<!-- Columns -->
<div class="grid-cols-1">1 column</div>
<div class="grid-cols-2">2 columns</div>
<div class="grid-cols-3">3 columns</div>
<div class="grid-cols-4">4 columns</div>
<div class="grid-cols-12">12 columns</div>

<!-- Column span -->
<div class="col-span-2">Span 2 columns</div>
<div class="col-start-2">Start at column 2</div>
```

### Position

```html
<!-- Position -->
<div class="static">Static</div>
<div class="relative">Relative</div>
<div class="absolute">Absolute</div>
<div class="fixed">Fixed</div>
<div class="sticky">Sticky</div>

<!-- Placement -->
<div class="top-0">Top 0</div>
<div class="bottom-0">Bottom 0</div>
<div class="left-0">Left 0</div>
<div class="right-0">Right 0</div>
<div class="inset-0">All sides 0</div>
```

---

## Responsive Design

### Breakpoints

```html
<!-- Mobile first - add breakpoint prefix for larger screens -->
<!-- sm: 640px -->
<!-- md: 768px -->
<!-- lg: 1024px -->
<!-- xl: 1280px -->
<!-- 2xl: 1536px -->

<!-- Apply at breakpoint -->
<div class="text-base md:text-lg lg:text-xl">Responsive text</div>

<!-- Hide on mobile -->
<div class="hidden md:block">Visible on md and up</div>
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What does `md:` do in Tailwind?
>
> - [ ] A) Apply styles only on mobile
> - [ ] B) Apply styles on medium screens (768px+)
> - [ ] C) Apply styles on all screens
>
> <details>
> <summary>Answer</summary>
> **B) Apply styles on medium screens (768px+)** - The prefix applies at that breakpoint and above!
> </details>

```html
<!-- Stack on mobile, row on desktop -->
<div class="flex flex-col md:flex-row">
  <div>Item 1</div>
  <div>Item 2</div>
</div>

<!-- Full width mobile, half desktop -->
<div class="w-full md:w-1/2">Responsive width</div>
```

---

## Customization

### Theme Extension

```javascript
// tailwind.config.js
export default {
  theme: {
    extend: {
      colors: {
        primary: "#0070f3",
        secondary: "#7928ca",
      },
      fontFamily: {
        sans: ["Inter", "system-ui", "sans-serif"],
      },
      spacing: {
        128: "32rem",
      },
      borderRadius: {
        xl: "1rem",
      },
    },
  },
};
```

### Custom Classes with @apply

```css
/* Add to your CSS file */
@layer components {
  .btn-primary {
    @apply bg-blue-500 text-white px-4 py-2 rounded;
  }

  .card {
    @apply bg-white rounded-lg shadow-md p-6;
  }
}
```

```html
<!-- Use in HTML -->
<button class="btn-primary">Primary</button>
<div class="card">Card content</div>
```

### Plugins

```bash
npm install -D @tailwindcss/forms @tailwindcss/typography
```

```javascript
// tailwind.config.js
export default {
  plugins: [require("@tailwindcss/forms"), require("@tailwindcss/typography")],
};
```

> [!TIP]
> Popular plugins: Forms, Typography, Line Clamp, Aspect Ratio

---

## Components

### Button Component

```tsx
interface ButtonProps {
  variant?: "primary" | "secondary" | "outline";
  size?: "sm" | "md" | "lg";
  children: React.ReactNode;
  onClick?: () => void;
}

export function Button({
  variant = "primary",
  size = "md",
  children,
  onClick,
}: ButtonProps) {
  const base =
    "inline-flex items-center justify-center font-medium rounded-md transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2";

  const variants = {
    primary: "bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500",
    secondary:
      "bg-purple-600 text-white hover:bg-purple-700 focus:ring-purple-500",
    outline:
      "border border-gray-300 text-gray-700 hover:bg-gray-50 focus:ring-gray-500",
  };

  const sizes = {
    sm: "px-3 py-1.5 text-sm",
    md: "px-4 py-2 text-base",
    lg: "px-6 py-3 text-lg",
  };

  return (
    <button
      className={`${base} ${variants[variant]} ${sizes[size]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

### Card Component

```tsx
interface CardProps {
  children: React.ReactNode;
  className?: string;
}

export function Card({ children, className = "" }) {
  return (
    <div className={`bg-white rounded-lg shadow-md p-6 ${className}`}>
      {children}
    </div>
  );
}

export function CardHeader({ children }) {
  return <div className="mb-4 border-b pb-4">{children}</div>;
}

export function CardContent({ children }) {
  return <div>{children}</div>;
}

export function CardFooter({ children }) {
  return <div className="mt-4 pt-4 border-t">{children}</div>;
}
```

### Input Component

```tsx
interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
}

export function Input({ label, error, className, ...props }: InputProps) {
  return (
    <div className="mb-4">
      {label && (
        <label className="block text-sm font-medium text-gray-700 mb-1">
          {label}
        </label>
      )}
      <input
        className={`
          w-full px-3 py-2 border rounded-md
          focus:outline-none focus:ring-2 focus:ring-blue-500
          ${error ? "border-red-500 focus:ring-red-500" : "border-gray-300"}
          ${className}
        `}
        {...props}
      />
      {error && <p className="mt-1 text-sm text-red-500">{error}</p>}
    </div>
  );
}
```

---

## Projects

### ⭐ Project 1: Landing Page

Build a landing page with:

- [ ] Hero section with centered content and CTA
- [ ] Features grid (responsive 3 columns)
- [ ] Testimonials section
- [ ] Call-to-action section
- [ ] Footer with links

> [!TIP]
> **Try it live:** [Tailwind Play](https://play.tailwindcss.com/)

---

### ⭐ Project 2: Dashboard UI

Create a dashboard with:

- [ ] Sidebar navigation (collapsible on mobile)
- [ ] Stats cards with icons
- [ ] Data table with pagination
- [ ] Charts layout
- [ ] Responsive design

---

### ⭐ Project 3: Blog Components

Build reusable components:

- [ ] Article card with image, title, excerpt
- [ ] Author bio component
- [ ] Code block with syntax highlighting style
- [ ] Image gallery grid

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] How do you add custom colors to Tailwind?
2. [ ] What's the difference between flex and grid in Tailwind?
3. [ ] How do responsive prefixes work?
4. [ ] What's @apply used for?
5. [ ] How do you create responsive layouts?
6. [ ] What's the purpose of the content array in config?

<details>
<summary>Click to reveal all answers</summary>

1. Add to theme.extend.colors in tailwind.config.js
2. Flex = 1D layout (row OR column), Grid = 2D (rows AND columns)
3. md: applies at 768px+, lg: at 1024px+, etc. - mobile-first
4. Extract reusable class combinations into custom CSS classes
5. Use responsive prefixes like md:flex-row, md:grid-cols-2
6. Tells Tailwind which files to scan for classes - must include all template files!

</details>

---

## Resources

- [Tailwind Docs](https://tailwindcss.com/docs)
- [Tailwind Play](https://play.tailwindcss.com) - Interactive playground!
- [Tailwind UI](https://tailwindui.com) - Component library
- [Heroicons](https://heroicons.com) - Icons for Tailwind

---

## Progress Checklist

- [ ] Tailwind configured in project
- [ ] Read through all sections
- [ ] Completed Project 1 (Landing Page)
- [ ] Completed Project 2 (Dashboard)
- [ ] Answered all quiz questions
- [ ] Ready for Module 10: Node.js

---

## Next Module

[Module 10: Node.js →](./10-nodejs.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - UUnderstanding For Every Developer_
