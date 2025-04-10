# JavaScript Event Listeners & Event Delegation Cheat Sheet

## Different Ways to Attach Event Listeners
1. **addEventListener** (Recommended)
```js
element.addEventListener(eventType, callback, options);
```
Most flexible (supports multiple listeners, options like capture, once, passive).

2. **Inline HTML onEvent Attribute** (Avoid for complex apps)
```html
<button onclick="handleClick()">Click Me</button>
```
❌ Downsides:

- Mixes HTML and JS (bad for maintainability).

- Only one handler per event.

- Global scope pollution (must define handler globally).

3. **DOM Property Assignment** (Limited Use)
```js
element.onclick = function() { console.log('Clicked!'); };
```
❌ Downsides:

- Overrides previous handlers (only one per event).

- No support for capture/passive options.

## ✅ addEventListener Basics

### Syntax
```js
element.addEventListener(eventType, callback, options);
```

### Parameters
| Parameter | Description |
|----------|-------------|
| `eventType` | A string like `'click'`, `'input'`, `'keydown'` |
| `callback` | The function to run when event fires |
| `options` / `useCapture` | Either a boolean or an object to define behavior |

### Options Object
```js
{
  capture: false, // use capturing phase
  once: false,    // remove listener after one trigger
  passive: false  // never call preventDefault()
}
```

## 🔁 Removing Event Listeners
```js
element.removeEventListener(eventType, callback, options);
```
- The `callback` and `options.capture` must match exactly.

```js
function handleClick() {}
button.addEventListener('click', handleClick, { capture: true });
button.removeEventListener('click', handleClick, { capture: true });
```

## addEventListener with an Object Handler (Instead of Function)
Modern JS allows passing an object with a handleEvent method as the listener:

```js
const handler = {
  handleEvent(event) {
    console.log('Event handled:', event.type);
  }
};

button.addEventListener('click', handler); // Automatically calls handler.handleEvent()
```
Useful for stateful handlers (object can store data).
Works with removeEventListener.

## 🎯 Event Phases

### Event Propagation Phases:
1. **Capturing Phase** (top → target)
2. **Target Phase**
3. **Bubbling Phase** (target → top)

### Controlling Propagation:
```js
event.stopPropagation();      // Stop bubbling
event.stopImmediatePropagation(); // Stops all listeners
```

### Prevent Default:
```js
event.preventDefault();
```

## 📦 Event Delegation

### What is it?
Attaching a single event listener to a parent element to manage events on its children.

### Why?
- Efficient memory usage
- Works well with dynamic elements

### Example:
```js
document.querySelector('#list').addEventListener('click', (e) => {
  if (e.target.matches('li')) {
    console.log('Item clicked:', e.target.textContent);
  }
});
```

## ✨ Custom Events
```js
const event = new CustomEvent('my-event', {
  detail: { key: 'value' }
});
element.dispatchEvent(event);
```

```js
element.addEventListener('my-event', (e) => {
  console.log(e.detail);
});
```

## ⚠️ Common Mistakes
- Not using matching `capture` when removing event listeners
- Forgetting to debounce or throttle input/scroll events
- Forgetting to clean up listeners (e.g., in SPA or components)

## 🧠 Tips
- Use delegation for lists, tables, etc.
- Prefer `{ once: true }` for one-time listeners
- Use `passive: true` for scroll listeners to improve performance
- Use `event.currentTarget` to access the element the listener is attached to

## 🧪 Example: Passive Scroll Listener
```js
window.addEventListener('scroll', handleScroll, { passive: true });
```

## 📌 Summary Table
| Feature | Purpose |
|--------|---------|
| `capture` | Use capturing phase instead of bubbling |
| `once` | Auto-remove listener after first call |
| `passive` | Boost performance by not calling preventDefault |
| `stopPropagation()` | Prevent event from bubbling up |
| `stopImmediatePropagation()` | Stop all listeners of same event |
| `preventDefault()` | Prevent default browser behavior |
| `currentTarget` | The element the handler is attached to |
| `target` | The element that initiated the event |

---
This is your one-stop MD guide for handling events in JavaScript efficiently.



