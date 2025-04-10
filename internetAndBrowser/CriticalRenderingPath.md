# Browser Critical Rendering Path (CRP) 📜

## The 5-Step Process

### 1. HTML → DOM (Incremental)

**What happens:**

- Bytes → Characters → Tokens → Nodes → DOM Tree
- Browser parses HTML line by line
- Creates DOM nodes for each element
- **Blocking events:**
  - `<script>` tags pause parsing (unless `async/defer`)
  - External resources trigger requests

**Visual:**

```html
<html>
  →
  <head>
    →
    <body>
      →
      <div>
        →
        <p>↓ ↓ ↓ ↓ ↓ DOM Node DOM Node DOM Node DOM Node DOM Node</p>
      </div>
    </body>
  </head>
</html>
```

### 2. CSS → CSSOM (Blocking, Non-Incremental)

**What happens:**

- All CSS must be fully parsed before use

- Browser resolves CSS specificity (cascade)

- Creates CSSOM tree with computed styles

**Why blocking?**

- Later CSS can override earlier rules

- `body { color: red }` → `body { color: blue }`

### 3. DOM + CSSOM → Render Tree (Visible Nodes Only)

**What happens:**

- Combines visible DOM nodes with computed CSS

- Excludes:

  - `display: none` elements

  - `<head>`, `<meta>` tags

  - Comments

- Creates visual hierarchy for painting

### 4. Layout (Reflow) (Calculate Sizes/Positions)

**What happens:**

- Calculates exact pixel positions

- Handles:

  - Viewport size changes

  - Device rotation

  - Style changes affecting box model

- Expensive operations:

  - `offsetTop`, `getComputedStyle()` reads

  - Changing `width` / `height` / `margins`

### 5. Paint (Pixels to Screen)

**What happens:**

- Converts layout to actual pixels

- Multiple layers for complex pages

- Two phases:

  - Paint Recording (Skia commands)

  - Rasterization (GPU/CPU drawing)

**Optimization Example:**
```
BAD: Changing `top/left` → Layout → Paint → Composite
GOOD: Changing `transform` → Composite only
```

### 6. Compositing (After Paint)

**What is Compositing?**
- **Purpose:** Combines different painted layers into the final screen image

- **Trigger:** Happens after painting, before display

```mermaid
flowchart LR
    A[Layout] --> B[Paint] --> C[Compositing] --> D[Display]
```

**What Gets Composited?**
- **Elements with:**
```css
transform: translateZ(0);
will-change: transform;
opacity: 0.99; /* >0.9 triggers GPU */
position: fixed;
```
- **Browser-decided layers (scrollable areas, etc.)**

### Incremental vs Non-Incremental

**Incremental (DOM):**

- Browser builds HTML piece by piece (like assembling Lego while unboxing)
- Shows partial content during loading

**Non-Incremental (CSSOM):**

- Browser needs ALL CSS before it can proceed
- Blocks rendering (like waiting for all Lego instructions before building)

## 2. Key Optimizations

### 🚀 DOM (HTML)

**Fix: Reduce total elements**

```html
<!-- BAD -->
<div>
  <div>
    <div><span>Hi</span></div>
  </div>
</div>

<!-- GOOD -->
<span>Hi</span>
```

### 🎨 CSSOM (Styles)

**Fix: Unblock rendering**

```html
<!-- Load non-critical CSS later -->
<link href="main.css" rel="stylesheet" media="all" />
<link href="animations.css" rel="stylesheet" media="(prefers-reduced-motion: no-preference)" />
```

### ⚡ Layout (Sizes/Positions)

**Fix: Avoid this pattern:**

```js
// BAD - Forces browser to recalculate 2x
element.style.width = '100px'; // Write
const width = element.offsetWidth; // Read (forces layout!)
element.style.height = `${width}px`; // Write again
```

**Do this instead:**

```js
// GOOD - Batch changes
requestAnimationFrame(() => {
  element.style.width = '100px';
  element.style.height = '100px';
});
```

### 🖌️ Paint (Pixels)

**Fix: Use GPU-friendly properties**

```js
/* BAD - Triggers layout */
.box { left: 100px; transition: left 0.3s; }

/* GOOD - Uses GPU */
.box { transform: translateX(100px); transition: transform 0.3s; }
```

## TL;DR - Golden Rules

- Less DOM = Faster

- CSS loads ASAP = Less blocking

- Read THEN write (never mix)

- GPU > CPU for animations
