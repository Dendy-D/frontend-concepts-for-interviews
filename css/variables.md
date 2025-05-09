# CSS Variables 🖌️

**CSS Variables allow you to store values (like colors, spacing, sizes) in reusable, scoped variables — making your styles cleaner and easier to maintain.**

---

## Basic Syntax

```css
:root {
  --main-color: #3498db;
  --padding: 1rem;
}

.button {
  background-color: var(--main-color);
  padding: var(--padding);
}
```

- **`--main-color`** is a custom property.

- **`var(--main-color)`** is how you use it.

## Where to Define CSS Variables

| Scope     | How                        | Example                     |
| --------- | -------------------------- | --------------------------- |
| Global    | Use `:root` selector       | `:root { --color: #fff; }`  |
| Component | Inside a specific selector | `.card { --gap: 10px; }`    |
| Inline    | As a `style` attribute     | `<div style="--gap: 10px">` |

> ⚠️ Variables are **inherited** — child elements can access variables from parent scopes.

## Using Fallbacks

```css
color: var(--maybe-undefined, black);
```

If `--maybe-undefined` isn't defined, it falls back to `'black'`.

## Real-World Use Cases

### Theming

```css
:root {
  --bg: white;
  --text: black;
}

[data-theme='dark'] {
  --bg: #121212;
  --text: #f5f5f5;
}
```

```css
body {
  background: var(--bg);
  color: var(--text);
}
```

## Responsive Spacing

```css
:root {
  --spacing: 16px;
}

.section {
  padding: var(--spacing);
}
```

## Dynamic Values with JavaScript

```js
// Set a variable
document.documentElement.style.setProperty('--primary-color', '#ff0000');

// Get a variable
const primaryColor = getComputedStyle(document.documentElement).getPropertyValue('--primary-color');
```

## Limitations

- No math: **You can't do `--size \* 2` directly in CSS (use `calc()` instead).**

- **Not scoped to media queries** (use tricks or JS).

- **Can't reference inside media queries or selectors themselves.**

## calc() with variables

```css
:root {
  --header-height: 60px;
}

.main {
  height: calc(100vh - var(--header-height));
}
```

## Why it's better to declare variables in :root but not in \*

### Key Differences Between :root and \* for CSS Variables

1. **Scope & Inheritance**

   - `:root` targets the **document root** (equivalent to `html` but with higher specificity)

   - `*` applies to **all elements** (including `html`, `body`, and every nested element)

2. **Performance Impact**

   - `* { --var: value }` forces the browser to:

     - Apply the variable declaration to every single element
     - Recalculate styles for the entire DOM tree

   - `:root { --var: value }` applies just once at the document level

3. **Variable Shadowing**

   - With `*`, you might accidentally override variables in child elements

   - `:root` establishes clean global variables that can be safely overridden locally

4. **Specificity**

   - `:root` has lower specificity than `html` (useful for overrides)

   - `*` has no specificity advantage and can cause unintended side effects

### Practical Example

```css
/* ❌ Problematic approach */
* {
  --primary: blue; /* Applies to EVERY element */
}

/* ✅ Recommended approach */
:root {
  --primary: blue; /* Single declaration */
}

header {
  --primary: red; /* Safe local override */
}
```

### Why :root is Better for Variables

1. **Single Source of Truth**

   - Variables cascade down naturally from the document root

   - No duplicate declarations in memory

2. **Performance Optimization**

   - Browsers can optimize variable lookup starting from `:root`

3. **Cleaner Overrides**

   - Child elements can redefine variables without conflicts

4. **Standard Practice**

   - Follows W3C recommendations for custom properties
