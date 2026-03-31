# Module 1: HTML Fundamentals

**Duration:** Week 1 | **Difficulty:** Beginner

---

> [!TIP]
> **Learning Tip:** This module is hands-on! Keep your code editor open and code along with each example.

---

## Overview

HTML (HyperText Markup Language) is the foundation of all web pages. It provides the structural content that browsers render.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand HTML document structure
- [ ] Use semantic HTML elements correctly
- [ ] Create accessible and well-formed documents
- [ ] Work with forms, tables, and media

---

## Table of Contents

1. [Introduction to HTML](#introduction)
2. [Document Structure](#structure)
3. [Text Elements](#text-elements)
4. [Links and Images](#links-and-images)
5. [Lists and Tables](#lists-and-tables)
6. [Forms](#forms)
7. [Semantic HTML](#semantic-html)
8. [Exercises](#exercises)
9. [Projects](#projects)

---

## 1. Introduction to HTML

### What is HTML?

HTML is a markup language that defines the structure of web content. It uses **tags** to tell the browser how to display content.

> [!NOTE]
> HTML is NOT a programming language—it's a markup language that describes structure.

### Your First HTML File

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My First Page</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>This is my first web page.</p>
  </body>
</html>
```

> [!QUESTION]
> 🔍 **Quick Check:** What would happen if you removed `<!DOCTYPE html>` from the top?
>
> <details>
> <summary>Click to reveal answer</summary>
> The browser would enter "quirks mode" and may render the page incorrectly.
> </details>

### Key Concepts

- **Tags:** Keywords enclosed in angle brackets (`<tag>`)
- **Elements:** Opening tag + content + closing tag
- **Attributes:** Additional information in opening tag

---

## 2. Document Structure

### The DOCTYPE Declaration

```html
<!DOCTYPE html>
```

> [!WARNING]
> ⚠️ This MUST be the very first line in your HTML document!

### The `<html>` Element

```html
<html lang="en">
  <!-- All content goes here -->
</html>
```

> [!TIP]
> Always add `lang="en"` for accessibility and SEO!

### The `<head>` Section

Contains metadata about the document:

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Page description" />
  <title>Page Title</title>
  <link rel="stylesheet" href="style.css" />
</head>
```

| Meta Tag      | Purpose                               |
| ------------- | ------------------------------------- |
| `charset`     | Character encoding (always use UTF-8) |
| `viewport`    | Responsive design for mobile          |
| `description` | SEO description                       |
| `title`       | Browser tab title                     |

### The `<body>` Section

Contains all visible content.

---

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What does the `<title>` tag do?
>
> - [ ] A) Sets the main heading on the page
> - [ ] B) Sets the text in the browser tab
> - [ ] C) Sets the page URL
>
> <details>
> <summary>Answer</summary>
> **B) Sets the text in the browser tab** - The title appears in the browser tab and is used by search engines.
> </details>

---

## 3. Text Elements

### Headings

HTML provides 6 levels of headings:

```html
<h1>Heading 1 - Main Title</h1>
<h2>Heading 2 - Section Title</h2>
<h3>Heading 3 - Subsection</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6 - Smallest</h6>
```

> [!WARNING]
> ⚠️ **Important:** Only use ONE `<h1>` per page! Use `<h2>`-`<h6>` for subheadings in a logical hierarchy.

### Paragraphs and Text Formatting

```html
<p>This is a paragraph.</p>

<!-- Text formatting -->
<strong>Bold - Important</strong>
<b>Bold - Stylistic</b>
<em>Italic - Emphasized</em>
<i>Italic - Stylistic</em>
<mark>Highlighted text</mark>
<small>Small text</small>
<sup>Superscript</sup>
<sub>Subscript</sub>
<u>Underlined</u>
<s>Strikethrough</s>
```

> [!NOTE]
>
> - `<strong>` and `<em>` have semantic meaning (important/emphasis)
> - `<b>` and `<i>` are purely visual styling

### Block vs Inline Elements

- **Block elements:** Take full width, start on new line (`<div>`, `<p>`, `<h1>`)
- **Inline elements:** Take only needed width, same line (`<span>`, `<a>`, `<strong>`)

---

## 4. Links and Images

### Links

```html
<!-- External link -->
<a href="https://google.com">Visit Google</a>

<!-- Internal link -->
<a href="about.html">About Us</a>

<!-- Same page anchor -->
<a href="#section-id">Jump to Section</a>

<!-- Open in new tab -->
<a href="https://example.com" target="_blank" rel="noopener">New Tab</a>
```

> [!WARNING]
> ⚠️ Always use `rel="noopener"` when using `target="_blank"` for security!

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What's the correct way to link to a section on the same page?
>
> - [ ] A) `<a href="section">Go to Section</a>`
> - [ ] B) `<a href="#section-id">Go to Section</a>`
> - [ ] C) `<a href="page.html#section">Go to Section</a>`
>
> <details>
> <summary>Answer</summary>
> **B) Use `#` followed by the id** - The target element needs `id="section-id"`.
> </details>

### Images

```html
<img
  src="image.jpg"
  alt="Description for accessibility"
  width="800"
  height="600"
/>
```

> [!WARNING]
> ⚠️ **Always include `alt` text!** It's required for accessibility and helps SEO.

### Figure and Figcaption

```html
<figure>
  <img src="chart.png" alt="Sales chart" />
  <figcaption>Figure 1: Quarterly sales growth</figcaption>
</figure>
```

---

## 5. Lists and Tables

### Unordered Lists

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

### Ordered Lists

```html
<ol>
  <li>First step</li>
  <li>Second step</li>
  <li>Third step</li>
</ol>
```

### Description Lists

```html
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language</dd>
  <dt>CSS</dt>
  <dd>Cascading Style Sheets</dd>
</dl>
```

### Tables

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>25</td>
      <td>New York</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>30</td>
      <td>London</td>
    </tr>
  </tbody>
</table>
```

---

## 6. Forms

### Basic Form Structure

```html
<form action="/submit" method="POST">
  <!-- Form controls go here -->
</form>
```

### Input Types

```html
<!-- Text input -->
<input type="text" name="username" placeholder="Enter username" />

<!-- Email input -->
<input type="email" name="email" required />

<!-- Password input -->
<input type="password" name="password" />

<!-- Number input -->
<input type="number" name="age" min="18" max="100" />

<!-- Checkbox -->
<input type="checkbox" id="terms" name="terms" />
<label for="terms">I agree to terms</label>

<!-- Radio buttons -->
<input type="radio" id="male" name="gender" value="male" />
<label for="male">Male</label>
<input type="radio" id="female" name="gender" value="female" />
<label for="female">Female</label>

<!-- Dropdown select -->
<select name="country">
  <option value="">Select country</option>
  <option value="us">United States</option>
  <option value="uk">United Kingdom</option>
</select>

<!-- Textarea -->
<textarea name="message" rows="4" cols="50"></textarea>

<!-- Submit button -->
<button type="submit">Submit</button>
```

### Form Attributes

| Attribute     | Description           |
| ------------- | --------------------- |
| `required`    | Makes field mandatory |
| `placeholder` | Shows hint text       |
| `min/max`     | Limits for numbers    |
| `pattern`     | Regex validation      |
| `disabled`    | Disables input        |

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What input type would you use for a phone number field?
>
> - [ ] A) `type="tel"`
> - [ ] B) `type="phone"`
> - [ ] C) `type="number"`
>
> <details>
> <summary>Answer</summary>
> **A) `type="tel"`** - This is the correct HTML5 input type for phone numbers.
> </details>

---

## 7. Semantic HTML

### Why Semantic HTML?

- **Accessibility:** Screen readers understand content
- **SEO:** Search engines index better
- **Maintainability:** Clear structure

> [!TIP]
> Think of semantic HTML as adding meaning to your markup—just like adding structure to a document with chapters and sections!

### Semantic Elements

```html
<!-- Page layout -->
<header>
  <nav>Navigation</nav>
</header>

<main>
  <article>
    <section>
      <h1>Article Title</h1>
      <p>Content...</p>
    </section>
  </article>

  <aside>
    <h2>Related Content</h2>
  </aside>
</main>

<footer>
  <p>&copy; 2024 Company</p>
</footer>
```

### List of Semantic Elements

| Element     | Purpose                |
| ----------- | ---------------------- |
| `<header>`  | Page or section header |
| `<nav>`     | Navigation links       |
| `<main>`    | Main content area      |
| `<article>` | Self-contained content |
| `<section>` | Thematic grouping      |
| `<aside>`   | Sidebar content        |
| `<footer>`  | Page or section footer |
| `<figure>`  | Media with caption     |
| `<time>`    | Date/time              |

---

## 8. Exercises

### ⭐ Exercise 1: Basic Page

Create an HTML page with:

- [ ] Proper DOCTYPE and structure
- [ ] A main heading (`<h1>`)
- [ ] 2-3 paragraphs of text
- [ ] A list of your favorite things

> [!TIP]
> **Try it live:** [Open in CodePen](https://codepen.io/pen/?template=html-basics-1)

---

### ⭐ Exercise 2: Links and Images

- [ ] Add an image with proper alt text
- [ ] Create links to 3 external websites
- [ ] Add an anchor link to jump to a section
- [ ] Use `target="_blank"` for at least one link

---

### ⭐ Exercise 3: Create a Form

Build a contact form with:

- [ ] Name field (text)
- [ ] Email field (email, required)
- [ ] Subject dropdown
- [ ] Message textarea
- [ ] Submit button

> [!WARNING]
> ⚠️ Don't forget `<label>` elements for accessibility!

---

### ⭐ Exercise 4: Table

Create a class schedule table with:

- [ ] Headers: Day, Time, Subject, Room
- [ ] 5 days of classes
- [ ] At least 4 periods per day
- [ ] Use `<thead>`, `<tbody>`, `<th>`, `<td>`

---

## 9. Projects

### 🚀 Project 1: Personal Portfolio Page (Mini)

Create a single-page portfolio with:

- [ ] Header with name and navigation
- [ ] About section with photo
- [ ] Skills list
- [ ] Projects showcase (using semantic HTML)
- [ ] Contact form
- [ ] Footer with social links

> [!TIP]
> **Stuck?** Start with the HTML skeleton, then fill in one section at a time!

---

### 🚀 Project 2: Blog Post

Create a blog post layout with:

- [ ] Article title and meta info
- [ ] Multiple paragraphs
- [ ] Images with captions
- [ ] Blockquote
- [ ] Code snippets (use `<code>` and `<pre>`)
- [ ] Related posts section

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What is the purpose of the DOCTYPE declaration?
2. [ ] What's the difference between `<div>` and `<span>`?
3. [ ] Why is semantic HTML important for accessibility?
4. [ ] How do you make a form field required?
5. [ ] What attributes are needed for an accessible image?
6. [ ] What's the difference between `<strong>` and `<b>`?
7. [ ] How do you create a dropdown with 3 options?
8. [ ] What element would you use for the main navigation?

<details>
<summary>Click to reveal all answers</summary>

1. Tells the browser this is an HTML5 document
2. `<div>` is block-level, `<span>` is inline
3. Screen readers can understand the structure; better SEO
4. Add the `required` attribute
5. `src` and `alt` (alt is required for accessibility!)
6. `<strong>` has semantic meaning (important), `<b>` is just visual
7. Use `<select>` with `<option>` elements inside
8. Use `<nav>` element

</details>

---

## Resources

- [MDN: HTML Basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)
- [HTML Living Standard](https://html.spec.whatwg.org/)
- [Web.dev: Learn HTML](https://web.dev/learn/html)
- [HTML Tutorial - W3Schools](https://www.w3schools.com/html/)

---

## Progress Checklist

- [ ] Read through the entire module
- [ ] Complete all 4 exercises
- [ ] Complete Project 1
- [ ] Answered all quiz questions
- [ ] Ready for Module 2: CSS Fundamentals

---

## Next Module

[Module 2: CSS Fundamentals →](../modules/02-css.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
