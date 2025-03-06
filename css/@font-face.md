# CSS `@font-face` and Font Formats

The `@font-face` rule allows you to use custom fonts in your web projects, ensuring consistent typography across devices.

---

## `@font-face` Syntax
```css
@font-face {
  font-family: 'MyFont'; /* Name your font */
  src: url('myfont.woff2') format('woff2'), /* Preferred format */
       url('myfont.woff') format('woff'),   /* Fallback format */
       url('myfont.ttf') format('truetype'); /* Legacy format */
  font-weight: 400; /* Optional: Specify weight */
  font-style: normal; /* Optional: Specify style */
}
```

## Font Formats
  - **WOFF2** (Web Open Font Format 2):
    Best for web: Highly compressed, smallest file size.
    Supported by all modern browsers.
    Example: `myfont.woff2`.

  - **WOFF** (Web Open Font Format):
    Good for web: Compressed, widely supported.
    Example: `myfont.woff`.

  - **OTF** (OpenType Font):
    Advanced features: Supports advanced typography (ligatures, alternates, etc.).
    Larger file size compared to WOFF2.
    Example: `myfont.otf`.

  - **TTF** (TrueType Font):
    Legacy format: Larger file size, no compression.
    Supported by all browsers but less efficient.
    Example: `myfont.ttf`.

  - **EOT** (Embedded OpenType):
    IE-only: Used for older versions of Internet Explorer.
    Example: `myfont.eot`.

  - **SVG** (Scalable Vector Graphics):
    Rarely used: Mainly for older iOS devices.
    Example: `myfont.svg`.

  ### Font Format Browser Support

  | Format | Modern Browsers | Legacy Browsers       |
  |--------|-----------------|-----------------------|
  | WOFF2  | ✅              | ❌ (IE, older Safari) |
  | WOFF   | ✅              | ✅ (IE9+)             |
  | OTF    | ✅              | ✅                    |
  | TTF    | ✅              | ✅                    |
  | EOT    | ❌              | ✅ (IE only)          |
  | SVG    | ❌              | ✅ (iOS < 5)          |

  ## Best Practices

  - **Use WOFF2 as the primary format**:
    - It’s the most efficient and widely supported.

  - **Provide fallbacks**:
    - Include WOFF, OTF, and TTF for older browsers.

  - **Optimize font loading**:
    - Use `font-display: swap;` to avoid invisible text during loading.

  - **Use local**:
    - Use `local('Font Name')` to check if a font is installed locally on the user's device.


  ```css
  @font-face {
    font-family: 'MyFont';
    src: url('myfont.woff2') format('woff2');
    font-display: swap;
  }
  ```

  ---

  ## What is local()?
  The `local()` function checks if a font is installed locally on the user's device.
  If the font is found, it uses the local version instead of downloading it.

  ### Syntax:
  ```css
  @font-face {
    font-family: 'MyFont';
    src: local('Font Name'), /* Check for local font */
        url('myfont.woff2') format('woff2'), /* Fallback to download */
        url('myfont.woff') format('woff');
  }
  ```

  ---

  ## What is font-display: swap?

  ### The font-display property controls how a font is displayed while it’s being loaded. The swap value tells the browser to:

  - Immediately display text using a fallback system font while the custom font is loading.

  - Swap to the custom font once it’s fully loaded.

  - This prevents the FOIT (Flash of Invisible Text) and ensures users can read content immediately, even if the custom font takes time to load.

  ### Why Use font-display: swap?
  - **Improves User Experience**:
    - Users can read content immediately, even if the custom font isn’t ready.
    - Avoids the **"invisible text"** problem during font loading.

  - **Better Performance Perception**:
    - Pages feel faster because content is visible right away.

  - **Widely Supported**:
    - Supported by all modern browsers (Chrome, Firefox, Safari, Edge).