# JavaScript Var 📌

## “var” tolerates redeclarations

- **If we declare the same variable with let twice in the same scope, that’s an error:**

```js
let user;
let user; // SyntaxError: 'user' has already been declared
```

or

```js
var user;
let user; // SyntaxError: 'user' has already been declared
```

- **With var, we can redeclare a variable any number of times. If we use var with an already-declared variable, it’s just ignored:**

```js
var user = "Pete";

var user = "John"; // this "var" does nothing (already declared)
// ...it doesn't trigger an error

alert(user); // John
```

## “var” variables can be declared below their use

```js
function sayHi() {
  phrase = "Hello"; // (*)

  if (false) {
    var phrase;
  } // even with false in if block, we still see var outside of that block

  alert(phrase);
}
sayHi();
```

## Tricky task with var, setTimeout and closures

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i));
}
// 3 3 3
```

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i));
}
// 0 1 2
```



