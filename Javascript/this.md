# `this` Keyword in JavaScript

## Table of Contents

- [What is `this`?](#what-is-this)
- [How `this` Works](#how-this-works)
- [Global Context](#global-context)
- [Inside an Object](#inside-an-object)
- [Regular Function](#regular-function)
- [Arrow Function](#arrow-function)
- [Constructor Function](#constructor-function)
- [Class Methods](#class-methods)
- [Event Handlers](#event-handlers)
- [`call()`](#call)
- [`apply()`](#apply)
- [`bind()`](#bind)
- [Arrow Function vs Regular Function](#arrow-function-vs-regular-function)

---

# What is `this`?

## Definition

> **`this` is a special keyword that refers to the object that is currently executing the function.**

## Simple Definition

> **The value of `this` depends on how a function is called, not where it is defined.**

---

# How `this` Works

Unlike many programming languages, JavaScript determines `this` at **runtime** based on the way a function is invoked.

The same function can have different values of `this`.

---

# Global Context

### Browser

```javascript
console.log(this);
```

Output

```javascript
window
```

---

### Node.js

```javascript
console.log(this);
```

Output

```javascript
{}
```

(At the top level of a CommonJS module.)

---

# Inside an Object

```javascript
const user = {
  name: "Vipul",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output

```
Vipul
```

Here,

```
this === user
```

---

# Regular Function

```javascript
function show() {
  console.log(this);
}

show();
```

### Non-Strict Mode

```
window (Browser)
```

### Strict Mode

```
undefined
```

---

# Arrow Function

Arrow functions **do not have their own `this`**.

They inherit `this` from their surrounding (lexical) scope.

```javascript
const user = {
  name: "Vipul",

  greet() {
    const print = () => {
      console.log(this.name);
    };

    print();
  }
};

user.greet();
```

Output

```
Vipul
```

The arrow function uses the `this` from `greet()`.

---

# Constructor Function

```javascript
function User(name) {
  this.name = name;
}

const user = new User("Vipul");

console.log(user.name);
```

Output

```
Vipul
```

When using `new`:

- A new object is created.
- `this` refers to that new object.
- The new object is returned automatically.

---

# Class Methods

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(this.name);
  }
}

const user = new User("Vipul");

user.greet();
```

Output

```
Vipul
```

Here,

```
this === user
```

---

# Event Handlers

```javascript
button.addEventListener("click", function () {
  console.log(this);
});
```

`this` refers to the element that triggered the event.

```
<button>
```

Using an arrow function:

```javascript
button.addEventListener("click", () => {
  console.log(this);
});
```

The arrow function inherits `this` from the outer scope, so it does **not** refer to the button.

---

# `call()`

Calls a function immediately with a specified `this`.

```javascript
function greet(city) {
  console.log(this.name, city);
}

const user = {
  name: "Vipul"
};

greet.call(user, "Pune");
```

Output

```
Vipul Pune
```

---

# `apply()`

Similar to `call()`, but arguments are passed as an array.

```javascript
function greet(city, country) {
  console.log(this.name, city, country);
}

const user = {
  name: "Vipul"
};

greet.apply(user, ["Pune", "India"]);
```

---

# `bind()`

Returns a new function with `this` permanently bound.

```javascript
function greet() {
  console.log(this.name);
}

const user = {
  name: "Vipul"
};

const fn = greet.bind(user);

fn();
```

Output

```
Vipul
```

---

# Arrow Function vs Regular Function

| Regular Function | Arrow Function |
|------------------|----------------|
| Has its own `this`. | Doesn't have its own `this`. |
| `this` depends on how it's called. | `this` is inherited from the surrounding scope. |
| Can be used as a constructor. | Cannot be used as a constructor. |
| Supports `call`, `apply`, and `bind`. | `call`, `apply`, and `bind` cannot change its `this`. |

---
