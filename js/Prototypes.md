# JavaScript Prototypes 📌

## **proto** vs. prototype: Professional Explanation

`prototype` is a property of constructor functions that serves as the blueprint for instances created with new.

`__proto__` (officially replaced by
`Object.getPrototypeOf()`) is an accessor property that exposes the internal `[[Prototype]]` of an object, forming the prototype chain.

1. `prototype`

- **Exists on:** Function objects only (created via `function`, `class`, but not `arrow functions`)

- **Purpose:** Provides shared properties/methods for instances

- **Default value:** An object with `constructor` property pointing back to the function

- **Mutation:** Can be completely replaced (common in inheritance patterns)

```js
function Engineer(name) {
  this.name = name;
}
Engineer.prototype.code = function () {
  console.log(`${this.name} is coding`);
};

const dev = new Engineer('John');
dev.code(); // Inherited from prototype
```

2. `__proto__ / [[Prototype]]`

- **Exists on:** All objects (including functions)

- **Purpose:** Implements prototype inheritance chain

- **Access:**

  - Legacy: `obj.__proto__` (deprecated)

  - Modern: `Object.getPrototypeOf(obj)`

- **Setting:**

  - Legacy: `obj.__proto__ = newProto`

  - Modern: `Object.setPrototypeOf(obj, newProto)` **(Also a bad idea)**

## Critical Relationships

1. Instance Creation:

```js
function F() {}
const obj = new F();

// The vital connection:
Object.getPrototypeOf(obj) === F.prototype; // true
```

2. Function Inheritance:

```js
function Parent() {}
function Child() {}

Child.prototype = Object.create(Parent.prototype);
// Now:
Object.getPrototypeOf(Child.prototype) === Parent.prototype; // true
```

3. Native Prototypes:

```js
// Array inheritance chain example:
const arr = [];
Object.getPrototypeOf(arr) === Array.prototype; // true
Object.getPrototypeOf(Array.prototype) === Object.prototype; // true
```

## What is the constructor Property?

## Base

1. **Automatic Reference:**

- Every function's prototype object automatically gets a constructor property pointing back to the function itself.

```js
function Person() {}
console.log(Person.prototype.constructor === Person); // true
```

2. **Inherited by Instances:**

- Objects created with new inherit this property via their **proto** (a.k.a. [[Prototype]]).

```js
const john = new Person();
console.log(john.constructor === Person); // true (looks up prototype chain)
```

### Key Behaviors

1. **Default Setup:**

```js
function Car() {}
// JavaScript automatically does:
// Car.prototype = { constructor: Car }
```

2. **Breakable:**

```js
Car.prototype = {}; // Overwrites prototype
const tesla = new Car();
console.log(tesla.constructor); // Now points to `Object` (not Car)
```

3. **Fixable:**

```js
Car.prototype = { constructor: Car }; // Manual fix
```

### Visual Prototype Chain

```
[ Instance (john) ]  
  ↓ __proto__  
[ Person.prototype ] → constructor → [ Person Function ]  
  ↓ __proto__  
[ Object.prototype ]
```

### Crucial difference JavaScript prototype vs. constructor Behavior

```js
function lol() {}
lol.prototype = { constructor: 'alalal' };
console.log(lol.constructor); // Output: function Function() { [native code] }
```

#### Expected vs. Actual

- **Expected:** `'alalal'` (from `lol.prototype.constructor`).

- **Actual:** Native Function constructor.

#### Why This Happens

1. **Two Different Chains**
- `prototype:` Used for instances (created via `new lol()`).
```js
const instance = new lol();
console.log(instance.constructor); // 'alalal' (from lol.prototype)
```

- `.__proto__` (or `[[Prototype]]`): Used for the function itself (lol).
  - `lol.constructor` looks up `lol.__proto__.constructor` (default: Function.prototype).

2. **Key Distinction**

| Property         | Affects          | Source of `constructor`         |
|------------------|------------------|---------------------------------|
| `lol.prototype`  | Instances of lol | `instance.constructor = 'alalal'` |
| `lol.__proto__`  | lol function     | `lol.constructor = Native Function` |

3. **Diagram**
```sequence
lol (function) → Function.prototype → Object.prototype
  |__ prototype → { constructor: 'alalal' } (for instances)
```

#### How to "Fix" (If Needed)
To change lol.constructor (**not recommended in practice**):

```js
Object.setPrototypeOf(lol, { constructor: 'alalal' });
console.log(lol.constructor); // 'alalal'
```

## Common inheritance pattern

```js
function Parent() {
  // …
}
Parent.prototype.parentMethod = function () {};

function Child() {
  Parent.call(this); // Make sure everything is initialized properly 
}

// Pointing the [[Prototype]] of Child.prototype to Parent.prototype
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

// Optional: Add child methods AFTER fixing constructor
Child.prototype.childMethod = function() {};
```

`Parent.call(this)` - kinda replace `super()` in classes


## Comparing with class syntax

1. **ES5 Manual Inheritance**
```js
function Parent(name) {
  this.name = name; // Instance property
}

function Child(name, age) {
  Parent.call(this, name); // ① Manual "super" call
  this.age = age;
}

Child.prototype = Object.create(Parent.prototype); // ② Set up prototype chain
Child.prototype.constructor = Child; // ③ Fix constructor
```

2. **ES6 Class Inheritance**
```js
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name); // ① Auto "super" call
    this.age = age;
  }
  // ② & ③ Handled automatically by `extends`
}
```

## Implementation of new operator

```js
function myNew(constructor, ...args) {
  const obj = Object.create(constructor.prototype);

  const result = constructor.apply(obj, args)

  return result instanceof Object ? result : obj;
}



// tests
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

// Test 1: Normal constructor
const john = myNew(Person, 'John');

console.log(john)

console.log(john.name); // 'John'
console.log(john.greet()); // 'Hello, John'

// Test 2: Constructor with object return
function Weird() {
  this.a = 1;
  return { b: 2 }; // Should override
}
console.log(myNew(Weird)); // { b: 2 } (not { a: 1 })
```

