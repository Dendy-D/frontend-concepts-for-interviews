# CSS `box-sizing` Property 🖌️

The `box-sizing` property controls how the width and height of an element are calculated, including padding and borders.

---

## Values
1. **`content-box`** (Default):
   - Width and height **exclude** padding and border.
   - Formula:  
     `Total Width = width + padding + border`  
     `Total Height = height + padding + border`

2. **`border-box`**:
   - Width and height **include** padding and border.
   - Formula:  
     `Total Width = width`  
     `Total Height = height`

---

## Example
```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  box-sizing: border-box; /* Includes padding and border in width/height */
}
```

## Why Use border-box?
  - Predictable Layouts: Ensures elements stay the specified size, even with padding or borders.
  - Easier Calculations: No need to adjust width/height manually when adding padding or borders.



## Best Practice
  Set box-sizing: border-box globally for consistent sizing:

  ```css
  * {
    box-sizing: border-box;
  }
  ```
