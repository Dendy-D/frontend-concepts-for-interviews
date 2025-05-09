# SVG Sprites 🖌️

SVG sprites are a technique for combining multiple SVG icons into a single file, making it easier to manage and use them in web projects. They improve performance by reducing HTTP requests and allow for easy styling and manipulation of icons.

## SVG sprites and embedded SVGs each have their use cases. Here's a quick comparison:

### SVG Sprites:
- **Pros**:
  - 🚀 **Reduced HTTP Requests**: Combines multiple icons into one file.
  - 📦 **Maintainability**: Centralized icons in one file for easier updates.
- **Cons**:
  - Requires an additional HTTP request for the sprite file.

### Embedded SVGs:
- **Pros**:
  - ⚡ **No Extra Requests**: Inlined directly in HTML.
  - 🛠️ **Simple for Small Projects**: Easy for a few icons.
- **Cons**:
  - 📈 **Harder to Scale**: Managing multiple SVGs in HTML becomes messy.
  - 🔄 **Repetitive Updates**: Changes must be made to each SVG individually.

### When to Use:
- **SVG Sprites**: Large projects, performance-critical apps, or frequent icon updates.
- **Embedded SVGs**: Small projects or one-off use cases.

---

## How to Create an SVG Sprite

1. **Combine SVGs into a Single File**:
  - Create a file called `sprite.svg`.
  - Wrap each SVG icon in a `<symbol>` element with a unique `id`.

  ```html
  <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
    <symbol id="icon-home" viewBox="0 0 24 24">
      <!-- SVG path for home icon -->
      <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/>
    </symbol>
    <symbol id="icon-search" viewBox="0 0 24 24">
      <!-- SVG path for search icon -->
      <path d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 0 0 1.48-5.34c-.47-2.78-2.79-5-5.59-5.34a6.505 6.505 
        0 0 0-7.27 7.27c.34 2.8 2.56 5.12 5.34 5.59a6.5 6.5 0 0 0 5.34-1.48l.27.28v.79l4.25 4.25c.41.41 1.08.41
        1.49 0 .41-.41.41-1.08 0-1.49L15.5 14z"
      />
    </symbol>
  </svg>
  ```

2. **How to use SVG Sprites**:
  - Include the sprite file in your HTML using an `<svg>` tag and the `<use>` element.

  ```html
  <svg>
    <use href="sprite.svg#icon-home"></use>
  </svg>
  <svg>
    <use href="sprite.svg#icon-search"></use>
  </svg>
  ```

---

## Using `currentColor` with SVG Sprites

The `currentColor` keyword is a powerful tool for styling SVG sprites. 
It allows SVGs to inherit the `color` value from their parent element, making icons dynamically customizable.

### How It Works:
- **In CSS**: Apply `fill: currentColor` or `stroke: currentColor` to the `<svg>` element to inherit the parent's `color`.
- **In SVG Code**: Use `currentColor` directly in the `fill` or `stroke` attributes of the SVG.

### Example:
```html
<!-- SVG Sprite Definition -->
<symbol id="plusIcon" viewBox="0 0 12 12">
  <path d="M6 1V11" stroke="currentColor" stroke-width="1.5" ... />
  <path d="M1 6H11" stroke="currentColor" stroke-width="1.5" ... />
</symbol>

<!-- Usage -->
<div style="color: blue;">
  <svg>
    <use href="sprite.svg#plusIcon"></use>
  </svg>
</div>
```

In this example, the `stroke` color of the `plusIcon` will be blue because it inherits the `color: blue;` from the parent `<div>`.

#### Benefits:
  - Dynamic Styling: Easily change icon colors with CSS.
  - Reusability: Icons adapt to different contexts without modifying the SVG code.
  - Consistency: Ensures icons match the text or design system colors.

#### When to Use:
  - Use currentColor when you want icons to dynamically inherit colors from their parent elements.
  - Use fixed colors (e.g., stroke="#7C7C7C") when the icon color should never change.

