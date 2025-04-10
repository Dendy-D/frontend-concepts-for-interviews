# JavaScript Arrays 📌

## Array constructor

The Array constructor can be called with or without `new` — both behave identically.

1. Single argument (non-number) → Array with that element

```js
const arr = Array('5'); // or new Array('5')
console.log(arr); // ['5']
console.log(arr.length); // 1
```

2. Single argument (number) → Empty array with that length

```js
const arr = Array(5); // or new Array(5)
console.log(arr); // [empty × 5] (sparse array)
console.log(arr.length); // 5
```

3. Multiple arguments → Array with those elements

```js
const arr = Array(5, 5); // or new Array(5, 5)
console.log(arr); // [5, 5]
console.log(arr.length); // 2
// Creates an array with two elements: both numbers 5
```

4. No arguments → Empty array

```js
const arr = Array(); // or new Array()
console.log(arr); // []
console.log(arr.length); // 0
// Creates an empty array
```

## Sparse arrays

### What is a Sparse Array?

A sparse array is an array where some indices don't have elements (they're "empty slots"). This happens when:

- You use `Array(n)` to create an array of length `n`

- You delete an element with `delete arr[i]`

- You set an index beyond the current length (`arr[100] = 'x'` without filling 0-99)

### The Problem with Sparse Arrays

Many array methods skip empty slots entirely, which can lead to surprising behavior:

1. Example 1: map() Doesn't Process Empty Slots

```js
const sparse = Array(5); // [empty × 5]
const mapped = sparse.map(() => 1);
console.log(mapped); // [empty × 5] 😱
```

2. Example 2: forEach() Also Skips Empties

```js
const sparse = Array(5);
sparse.forEach((_, i) => console.log(i)); // No output!
```

### Solutions to Handle Sparse Arrays

1. **Spread Operator (Converts to Dense Array)**

```js
const dense = [...Array(5)]; // [undefined, undefined, undefined, undefined, undefined]
const mapped = dense.map(() => 1); // [1, 1, 1, 1, 1]
```

2. **Array.from()**

```js
const dense = Array.from(Array(5)); // Similar to spread
const mapped = dense.map(() => 1); // [1, 1, 1, 1, 1]
```

3. **Explicit Fill**

```js
const filled = Array(5).fill(); // [undefined, undefined, ...]
const mapped = filled.map(() => 1); // [1, 1, 1, 1, 1]
```

4. **Array.from() with Mapper**

```js
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

### Array Initialization Methods Comparison

| Method                 | Creates              | `map()` Works? | Memory Efficient? | Notes                          |
| ---------------------- | -------------------- | -------------- | ----------------- | ------------------------------ |
| `Array(5)`             | Sparse (empty slots) | ❌ No          | ✅ Yes            | Creates "holes" in array       |
| `[...Array(5)]`        | Dense (undefined)    | ✅ Yes         | ❌ No             | Spread operator converts       |
| `Array(5).fill()`      | Dense (undefined)    | ✅ Yes         | ❌ No             | Explicit fill with `undefined` |
| `Array.from(Array(5))` | Dense (undefined)    | ✅ Yes         | ❌ No             | Similar to spread syntax       |
| `Array(5).fill(0)`     | Dense (0)            | ✅ Yes         | ❌ No             | Pre-filled with actual values  |

### Performance Considerations

- **Memory Usage:**

  - Sparse arrays use less memory for unset indices

  - Dense arrays store undefined for each index

- **Initialization Benchmarks: (ways to get array with undefined values (dense array) from sparse array)**

  - `Array(n).fill()` - Fastest for small arrays

  - `Array.from({length:n})` - Best for large arrays

  - `[...Array(n)]` - Clean syntax but slower for huge arrays

### Tricks

1. Create Matrix:

```js
const matrix = Array(3)
  .fill()
  .map(() => Array(3).fill(0));
// [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
```

2. Identity Matrix

```js
const identity = Array(3)
  .fill()
  .map((_, i) =>
    Array(3)
      .fill()
      .map((_, j) => (i === j ? 1 : 0))
  );
// [[1, 0, 0], [0, 1, 0], [0, 0, 1]]
```

3. Dynamic Value Matrix

```js
const dynamicMatrix = Array(3)
  .fill()
  .map((_, i) =>
    Array(3)
      .fill()
      .map((_, j) => i * j)
  );
// [[0, 0, 0], [0, 1, 2], [0, 2, 4]]
```

4. Range Generation (0..n-1)

```js
const range = [...Array(5).keys()]; // [0, 1, 2, 3, 4]
```

5. Custom Range (start..end)

```js
const range = (start, end) => Array.from({ length: end - start }, (_, i) => start + i);

range(3, 7); // [3, 4, 5, 6]
```

### Pitfalls

1. Filling objects
```js
const arr = Array(5).fill({});
console.log(arr) // [{}, {}, {}, {}, {}] - the same object on each position

// The same as
const obj = {}
const arr = Array(5).fill(obj);
obj.name = 'Dan';
console.log(arr) // [{name: 'Dan'}, {name: 'Dan'}, {name: 'Dan'}, {name: 'Dan'}, {name: 'Dan'}];

// How to avoid this
const arr = Array(5).fill(0).map(() => {});
arr[0].name = 'Dan';
console.log(arr) // [{name: 'Dan'}, {}, {}, {}, {}];
```

## Complete Guide to Array.from()

### Basic Syntax

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

### Key Capabilities

1. Convert array-like objects (NodeList, arguments, strings)

```js
Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
```

2. Create arrays from iterables (Sets, Maps, Generators)

```js
Array.from(new Set([1, 2, 2, 3])); // [1, 2, 3]
```

3. Initialize with length + mapper

```js
Array.from({ length: 5 }, (_, i) => i * 2); // [0, 2, 4, 6, 8]
```

4. Clone arrays (shallow copy)

```js
const arr = [1, { a: 2 }];
const copy = Array.from(arr);
```

### `Array.from()` vs Spread Operator (`...`) Comparison

| Feature                | `Array.from()`       | Spread Operator (`...`) | Notes                                    |
| ---------------------- | -------------------- | ----------------------- | ---------------------------------------- |
| **Array-like objects** | ✅ Yes               | ❌ No                   | Spread only works on iterables           |
| **Strings**            | ✅ Converts to chars | ✅ Converts to chars    | Both split strings                       |
| **Iterables**          | ✅ Yes               | ✅ Yes                  | Works equally well                       |
| **Length + mapping**   | ✅ Built-in          | ❌ Requires map         | Spread needs extra step                  |
| **Shallow copying**    | ✅ Yes               | ✅ Yes                  | Both create new arrays                   |
| **Performance**        | ⚠️ Slightly slower   | ⚠️ Faster               | For simple array cloning                 |
| **TypeScript**         | ✅ Better inference  | ⚠️ Needs annotation     | `Array.from` has superior type inference |

### Key Differences Explained:

- **Array-like Objects**: Objects with `length` and index properties (e.g., `arguments`, `NodeList`)
- **Built-in Mapping**: `Array.from(arrayLike, mapFn)` vs `[...arrayLike].map(fn)`
- **TypeScript**: `Array.from` automatically infers types in most cases

## Array Methods: Mutation vs. Non-Mutation

### Mutation Methods (Modify Original Array)

These methods change the original array:

1. Add/Remove Elements

- `push()` - Adds to end

- `pop()` - Removes from end

- `unshift()` - Adds to start

- `shift()` - Removes from start

- `splice()` - Adds/removes anywhere

2. Order Modification

- `reverse()` - Reverses order

- `sort()` - Sorts elements

3. Content Modification

- `fill()` - Fills with static value

- `copyWithin()` - Copies within array

### Non-Mutation Methods (Return New Array/Value)

These keep the original array intact:

1. New Array Creation
  - `concat()` - Merges arrays

  - `slice()` - Returns portion

  - `flat()` - Flattens nested

  - `flatMap()` - Maps then flattens

2. Transformation
  - `map()` - Creates new transformed array

  - `filter()` - Creates filtered array

  - `reduce()` / `reduceRight()` - Returns single value

3. Search/Test
  - `find()` / `findIndex()` - Returns element/index

  - `includes()` - Checks existence

  - `indexOf()` / `lastIndexOf()` - Finds position

  - `some()` / `every()` - Tests conditions

4. Iteration
  - `forEach()` - Iterates (but doesn't return)

  - `entries()` / `keys()` / `values()` - Returns iterators


## My own flat implementation

```js
const myFlat = (arr, depth = 1) => {
  const result = [];
  for (let elem of arr) {
    if (Array.isArray(elem) && depth > 0) {
      const flattened = myFlat(elem, depth - 1);
      result.push(...flattened);
    } else {
      result.push(elem);111
  }

  return result;
};
// depth = 1 (by default)
myFlat([1, [2, [3]]]) // [1, 2, [3]] ← stops at level 2
// depth = 2
myFlat([1, [2, [3]]], Infinity) // [1, 2, 3] ← flattens everything!
```

