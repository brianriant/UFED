# Module 2: CSS Fundamentals

**Duration:** Week 2 | **Difficulty:** Beginner

---

> [!TIP]
> **Learning Tip:** CSS is all about practice! Each concept builds on the previous one, so work through the examples in order.

---

## Overview

CSS (Cascading Style Sheets) controls the visual presentation of web pages. It handles layout, colors, typography, and responsiveness.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand CSS syntax and selectors
- [ ] Master the box model
- [ ] Use Flexbox and Grid for layout
- [ ] Create responsive designs
- [ ] Work with animations and transitions

---

## Table of Contents

1. [Introduction to CSS](#introduction)
2. [Selectors and Specificity](#selectors)
3. [The Box Model](#box-model)
4. [Typography](#typography)
5. [Colors and Backgrounds](#colors)
6. [Flexbox Layout](#flexbox)
7. [CSS Grid](#grid)
8. [Responsive Design](#responsive)
9. [Transforms and Animations](#animations)
10. [Projects](#projects)

---

## 1. Introduction to CSS

### Ways to Add CSS

```html
<!-- External CSS (Recommended) -->
<link rel="stylesheet" href="styles.css" />

<!-- Internal CSS -->
<style>
  h1 {
    color: blue;
  }
</style>

<!-- Inline CSS (Avoid!) -->
<h1 style="color: blue;">Title</h1>
```

> [!WARNING]
> ⚠️ **Avoid inline styles!** They make code hard to maintain and override.

### Basic Syntax

```css
/* Selector */
h1 {
  /* Property: Value */
  color: blue;
  font-size: 24px;
}
```

> [!NOTE]
> CSS comments use `/* */` - unlike HTML `<!-- -->`

---

## 2. Selectors and Specificity

### Basic Selectors

```css
/* Element selector */
p {
  color: black;
}

/* Class selector */
.highlight {
  background: yellow;
}

/* ID selector */
#header {
  height: 100px;
}

/* Universal selector */
* {
  margin: 0;
}

/* Attribute selector */
[type="text"] {
  border: 1px solid gray;
}
```

### Combinators

```css
/* Descendant - any p inside div */
div p {
}

/* Child - direct p children of div */
div > p {
}

/* Adjacent sibling - p immediately after h2 */
h2 + p {
}

/* General sibling - any p after h2 */
h2 ~ p {
}
```

### Pseudo-classes

```css
/* User interaction */
:hover    /* Mouse over */
:focus    /* Input focus */
:active   /* While clicking */
:visited  /* Visited links */

/* Position */
:first-child
:last-child
:nth-child(2)
:nth-of-type(odd)
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** Which selector targets only direct children of `nav`?
>
> - [ ] A) `nav a`
> - [ ] B) `nav > a`
> - [ ] C) `nav + a`
>
> <details>
> <summary>Answer</summary>
> **B) `nav > a`** - The `>` combinator targets only direct children.
> </details>

### Pseudo-elements

```css
::before  /* Insert before content */
::after   /* Insert after content */
::first-line
::first-letter
::selection
```

### Specificity Order

```
1. Inline styles      (1000)
2. ID selectors      (100)
3. Classes/attrs     (10)
4. Elements          (1)
```

```css
/* Specificity: 0-1-1-1 */
div.highlight {
}

/* Specificity: 0-0-2-0 */
.highlight.urgent {
}

/* Specificity: 0-1-0-0 */
#header {
}
```

> [!WARNING]
> ⚠️ High specificity makes CSS hard to maintain! Prefer classes over IDs.

---

## 3. The Box Model

### Box Model Components

```
┌─────────────────────┐
│      Margin         │ ← Outside border
├─────────────────────┤
│      Border         │
├─────────────────────┤
│      Padding        │
├─────────────────────┤
│    Content Area    │ ← Width/Height
└─────────────────────┘
```

### Box Model Properties

```css
.box {
  /* Content size */
  width: 200px;
  height: 100px;

  /* Space inside border */
  padding: 20px;

  /* Border around padding */
  border: 3px solid #333;

  /* Space outside border */
  margin: 20px;
}
```

### Box Sizing

```css
/* Default - width only includes content */
box-sizing: content-box;

/* Width includes content + padding + border */
box-sizing: border-box;

/* Global reset */
* {
  box-sizing: border-box;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** With `box-sizing: border-box`, what is the actual width of an element with `width: 200px`, `padding: 20px`, and `border: 5px`?
>
> - [ ] A) 250px
> - [ ] B) 200px
> - [ ] C) 150px
>
> <details>
> <summary>Answer</summary>
> **B) 200px** - With border-box, padding and border are included in the 200px width.
> </details>

---

## 4. Typography

### Font Properties

```css
.text {
  /* Font family */
  font-family: "Inter", system-ui, sans-serif;
  font-family: "Georgia", serif;

  /* Font size */
  font-size: 16px;
  font-size: 1.5rem;
  font-size: 100%;

  /* Font weight */
  font-weight: 400;
  font-weight: bold;

  /* Line height */
  line-height: 1.5;

  /* Letter spacing */
  letter-spacing: 0.5px;

  /* Text transform */
  text-transform: uppercase;
  text-transform: capitalize;

  /* Text alignment */
  text-align: left;
  text-align: center;

  /* Text decoration */
  text-decoration: underline;
  text-decoration: none;
}
```

### Web Fonts

```css
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap");

body {
  font-family: "Inter", sans-serif;
}
```

> [!TIP]
> Use `rem` instead of `px` for responsive typography! 1rem = 16px by default.

### Text Effects

```css
.effect {
  /* Text shadow */
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);

  /* Text overflow */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

---

## 5. Colors and Backgrounds

### Color Values

```css
.colors {
  /* Named color */
  color: red;

  /* Hex */
  color: #ff0000;
  color: #f00;

  /* RGB */
  color: rgb(255, 0, 0);

  /* RGBA (with opacity) */
  color: rgba(255, 0, 0, 0.5);

  /* HSL */
  color: hsl(0, 100%, 50%);

  /* Current text color */
  color: currentColor;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What color does `rgba(255, 0, 0, 0)` produce?
>
> - [ ] A) Black
> - [ ] B) Transparent
> - [ ] C) Red
>
> <details>
> <summary>Answer</summary>
> **B) Transparent** - The last value (alpha) is 0, making it fully transparent.
> </details>

### Background Properties

```css
.bg {
  /* Background color */
  background-color: #f0f0f0;

  /* Background image */
  background-image: url("image.jpg");

  /* Background repeat */
  background-repeat: no-repeat;
  background-repeat: repeat-x;

  /* Background position */
  background-position: center;
  background-position: 20px 10px;

  /* Background size */
  background-size: cover;
  background-size: contain;
  background-size: 100px 50px;

  /* Shorthand */
  background: url("img.jpg") no-repeat center/cover;
}
```

### Gradients

```css
.gradient {
  /* Linear gradient */
  background: linear-gradient(to right, red, blue);
  background: linear-gradient(45deg, red, orange, yellow);

  /* Radial gradient */
  background: radial-gradient(circle, red, blue);

  /* Multiple backgrounds */
  background:
    linear-gradient(to right, transparent, rgba(0, 0, 0, 0.5)),
    url("image.jpg") center/cover;
}
```

---

## 6. Flexbox Layout

### Container Properties

```css
.flex-container {
  /* Enable flexbox */
  display: flex;

  /* Main axis direction */
  flex-direction: row; /* default */
  flex-direction: row-reverse;
  flex-direction: column;
  flex-direction: column-reverse;

  /* Wrap items */
  flex-wrap: nowrap; /* default */
  flex-wrap: wrap;
  flex-wrap: wrap-reverse;

  /* Shorthand for direction + wrap */
  flex-flow: row wrap;

  /* Main axis alignment */
  justify-content: flex-start; /* default */
  justify-content: flex-end;
  justify-content: center;
  justify-content: space-between;
  justify-content: space-around;
  justify-content: space-evenly;

  /* Cross axis alignment */
  align-items: stretch; /* default */
  align-items: flex-start;
  align-items: flex-end;
  align-items: center;
  align-items: baseline;

  /* Multi-line alignment */
  align-content: flex-start;
  align-content: center;
  align-content: space-between;
}
```

### Item Properties

```css
.flex-item {
  /* Order (default 0) */
  order: 1;
  order: -1;

  /* Grow factor */
  flex-grow: 1;
  flex-grow: 2; /* takes 2x space */

  /* Shrink factor */
  flex-shrink: 1;

  /* Base size */
  flex-basis: 200px;
  flex-basis: 50%;

  /* Shorthand: grow shrink basis */
  flex: 1 0 200px;

  /* Individual alignment */
  align-self: auto;
  align-self: flex-start;
  align-self: center;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** How do you center an item both horizontally AND vertically in a flex container?
>
> - [ ] A) `justify-content: center; align-items: center;`
> - [ ] B) `text-align: center;`
> - [ ] C) `margin: auto;`
>
> <details>
> <summary>Answer</summary>
> **A) `justify-content: center; align-items: center;`** - justify-content handles horizontal (main axis), align-items handles vertical (cross axis).
> </details>

### Common Flexbox Patterns

```css
/* Center item */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Space between */
.space-between {
  display: flex;
  justify-content: space-between;
}

/* Responsive stack */
.stack {
  display: flex;
  flex-direction: column;
}
@media (min-width: 768px) {
  .stack {
    flex-direction: row;
  }
}
```

---

## 7. CSS Grid

### Container Properties

```css
.grid-container {
  /* Enable grid */
  display: grid;

  /* Define columns */
  grid-template-columns: 100px 100px 100px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));

  /* Define rows */
  grid-template-rows: 100px auto;
  grid-template-rows: repeat(3, 1fr);

  /* Gap */
  gap: 20px;
  row-gap: 10px;
  column-gap: 20px;

  /* Implicit rows */
  grid-auto-rows: 100px;

  /* Grid areas */
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}
```

### Item Properties

```css
.grid-item {
  /* Place in grid */
  grid-column: 1 / 3; /* start / end */
  grid-column: 1 / span 2; /* start / span */

  grid-row: 2 / 4;

  /* Named area */
  grid-area: header;

  /* Self alignment */
  justify-self: start;
  justify-self: center;
  align-self: start;
}
```

### Grid Template Areas

```css
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "nav    main   sidebar"
    "footer footer footer";
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
}

.header {
  grid-area: header;
}
.nav {
  grid-area: nav;
}
.main {
  grid-area: main;
}
.sidebar {
  grid-area: sidebar;
}
.footer {
  grid-area: footer;
}
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 5:** What does `grid-template-columns: repeat(auto-fit, minmax(200px, 1fr))` do?
>
> - [ ] A) Creates exactly 3 columns
> - [ ] B) Creates responsive columns that fit available space
> - [ ] C) Creates 200px columns
>
> <details>
> <summary>Answer</summary>
> **B) Creates responsive columns** - auto-fit fills the available space, minmax ensures columns are at least 200px but can grow to 1fr.
> </details>

---

## 8. Responsive Design

### Media Queries

```css
/* Mobile first - base styles */

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    width: 960px;
  }
}

/* Large desktop */
@media (min-width: 1440px) {
  .container {
    width: 1200px;
  }
}

/* Specific breakpoints */
@media (max-width: 767px) {
  .hide-mobile {
    display: none;
  }
}

/* Orientation */
@media (orientation: landscape) {
  .landscape-only {
    display: block;
  }
}
```

### Common Breakpoints

```
Mobile:    0 - 767px
Tablet:    768px - 1023px
Desktop:   1024px - 1439px
Large:     1440px+
```

### Responsive Patterns

```css
/* Container */
.container {
  width: 100%;
  padding: 0 20px;
}

@media (min-width: 768px) {
  .container {
    max-width: 750px;
    margin: 0 auto;
  }
}

@media (min-width: 1024px) {
  .container {
    max-width: 960px;
  }
}

/* Responsive image */
img {
  max-width: 100%;
  height: auto;
}
```

---

## 9. Transforms and Animations

### Transforms

```css
.transform {
  /* 2D transforms */
  transform: translate(20px, 10px);
  transform: rotate(45deg);
  transform: scale(1.5);
  transform: skewX(20deg);

  /* Combined */
  transform: translate(20px, 10px) rotate(45deg);

  /* 3D transforms */
  transform: perspective(1000px) rotateY(45deg);
  transform: translateZ(100px);

  /* Transform origin */
  transform-origin: center;
  transform-origin: top left;
}
```

### Transitions

```css
.transition {
  /* Property, duration, timing, delay */
  transition: background-color 0.3s ease;
  transition: all 0.3s ease-in-out;

  /* Multiple properties */
  transition:
    background-color 0.3s,
    transform 0.2s;
}
```

> [!TIP]
> Common timing functions: `ease`, `linear`, `ease-in`, `ease-out`, `ease-in-out`

### Keyframe Animations

```css
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideIn {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

.animated {
  animation: fadeIn 1s ease forwards;
  animation: slideIn 0.5s ease-out 0.5s both;

  /* Animation properties */
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-play-state: paused;
}
```

---

## 10. Projects

### 🚀 Project 1: Landing Page

Create a landing page with:

- [ ] Hero section with centered content
- [ ] Features grid (3 columns)
- [ ] Pricing table
- [ ] Call-to-action button
- [ ] Footer

> [!TIP]
> **Try it live:** [Open in CodePen](https://codepen.io/pen/?template=landing-page-css)

---

### 🚀 Project 2: Dashboard Layout

Build a dashboard with:

- [ ] Sidebar navigation
- [ ] Header with search
- [ ] Main content area with cards
- [ ] Responsive (collapses sidebar on mobile)

---

### 🚀 Project 3: Animated Card Component

Create a card with:

- [ ] Hover effects (scale, shadow)
- [ ] Hover-triggered animation
- [ ] Smooth transitions

> [!WARNING]
> ⚠️ Test on mobile! Some hover effects don't work well on touch devices.

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between `display: flex` and `display: grid`?
2. [ ] How do you center an element both horizontally and vertically with Flexbox?
3. [ ] What does `box-sizing: border-box` do?
4. [ ] How do you create a responsive grid that auto-adjusts columns?
5. [ ] What's the CSS specificity order?
6. [ ] How do you animate an element on hover?
7. [ ] What does `justify-content` vs `align-items` control?
8. [ ] How do media queries work?

<details>
<summary>Click to reveal all answers</summary>

1. Flexbox is 1-dimensional (row OR column), Grid is 2-dimensional (rows AND columns)
2. Use `justify-content: center` and `align-items: center`
3. Includes padding and border in the element's width/height
4. Use `repeat(auto-fit, minmax(min, 1fr))`
5. Inline(1000) > ID(100) > Class(10) > Element(1)
6. Use `transition` property or `@keyframes` with `:hover`
7. justify-content = main axis (usually horizontal), align-items = cross axis (usually vertical)
8. Media queries apply styles at specific screen widths

</details>

---

## Resources

- [MDN: CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [CSS-Tricks: Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS-Tricks: Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [Flexbox Froggy](https://flexboxfroggy.com/) - Interactive game!
- [Grid Garden](https://cssgridgarden.com/) - Interactive game!

---

## Progress Checklist

- [ ] Read through the entire module
- [ ] Completed Project 1 (Landing Page)
- [ ] Completed Project 2 (Dashboard)
- [ ] Completed Project 3 (Animated Card)
- [ ] Answered all quiz questions
- [ ] Ready for Module 3: Git Basics

---

## Next Module

[Module 3: Git Basics →](../modules/03-git.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
