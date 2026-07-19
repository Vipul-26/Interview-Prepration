# Interview Questions

A comprehensive collection of JavaScript, React, HTML, CSS, and related web development interview topics with code examples.

---

## Table of Contents

- [JavaScript Fundamentals](#javascript-fundamentals)
  - [Array Methods](#array-methods)
  - [Scope and Closures](#scope-and-closures)
  - [IIFE](#iife)
  - [Promises](#promises)
  - [Promise.all vs Promise.allSettled vs Promise.race vs Promise.any](#promiseall-vs-promiseallsettled-vs-promiserace-vs-promiseany)
  - [Callback vs Promises](#callback-vs-promises)
  - [call, apply, bind](#call-apply-bind)
  - [Currying](#currying)
  - [Object and Array Destructuring](#object-and-array-destructuring)
  - [Arrow Functions](#arrow-functions)
  - [Function Statement vs Expression](#function-statement-vs-expression)
  - [Callback Hell](#callback-hell)
  - [Map and Set](#map-and-set)
  - [WeakMap and WeakSet](#weakmap-and-weakset)
  - [Prototype](#prototype)
  - [Miscellaneous JS Gotchas](#miscellaneous-js-gotchas)
- [React](#react)
  - [Mapping Data](#mapping-data)
  - [Outside Click Handler](#outside-click-handler)
  - [Controlled vs Uncontrolled Components](#controlled-vs-uncontrolled-components)
  - [Props Drilling and Context API](#props-drilling-and-context-api)
  - [Lazy Loading](#lazy-loading)
  - [Memoization and useCallback](#memoization-and-usecallback)
  - [Passing Data from Child to Parent](#passing-data-from-child-to-parent)
  - [API Calls: fetch vs axios](#api-calls-fetch-vs-axios)
  - [Refs and Forwarding Refs](#refs-and-forwarding-refs)
  - [Component vs PureComponent](#component-vs-purecomponent)
  - [setInterval and setTimeout](#setinterval-and-settimeout)
  - [React Strict Mode](#react-strict-mode)
  - [Hook Order Rule](#hook-order-rule)
  - [Common React Facts](#common-react-facts)
- [DOM Manipulation](#dom-manipulation)
- [HTML](#html)
- [CSS and Sass](#css-and-sass)

---

## JavaScript Fundamentals

#### Closure

```javascript
function x() {
  var a = 7;
  function y() {
    console.log(a);
  }
  return y;
}

var z = x();
console.log(z()); // 7
```

A closure is the combination of a function bundled together with references to its surrounding state (lexical environment). Even after `x()` has returned, `y` still has access to `a`.

---

### IIFE

Immediately Invoked Function Expression (uses the grouping operator):

```javascript
(function (data) {
  console.log("Vipul", data);
})(data);
```

---

### call, apply, bind

```javascript
var pokemon = {
  firstname: "Pika",
  lastname: "Chu ",
  getPokeName: function () {
    var fullname = this.firstname + " " + this.lastname;
    return fullname;
  },
};

var pokemonName = function (snack, hobby) {
  console.log(this.getPokeName() + " loves " + snack + " and " + hobby);
};

// call: arguments passed individually
pokemonName.call(pokemon, "sushi", "algorithms");

// apply: arguments passed as an array
pokemonName.apply(pokemon, ["sushi", "algorithms"]);

// bind: returns a new function with bound context
var logPokemon = pokemonName.bind(pokemon);
logPokemon("sushi", "algorithms");
```

---

### Currying

#### Using nested arrow functions

```javascript
const addCurry = (a) => {
  return (b) => {
    return (c) => {
      return a + b + c;
    };
  };
};

addCurry(2)(3)(4); // 9
```

#### Using bind

```javascript
const addCurry = (a, b) => {
  return a + b;
};

const addCurryTwo = addCurry.bind(this, 2);
addCurryTwo(3); // 5

const addCurryThree = addCurry.bind(this, 10);
addCurryThree(20); // 30
```

#### Practical use case: avoiding repeated arguments

```javascript
function volume(h) {
  return (w) => {
    return (l) => {
      return l * w * h;
    };
  };
}

const hCylinderHeight = volume(100);
hCylinderHeight(200)(30); // 600,000
hCylinderHeight(2322)(232); // 53,870,400
```

---

### Object and Array Destructuring

#### Basic destructuring

```javascript
const user = {
  name: "Alex",
  address: "15th Park Avenue",
  age: 43,
};

const { name } = user;
```

#### Default values

```javascript
const { name, age, salary = 123455 } = user;
```

#### Aliasing

```javascript
const { address: permanentAddress } = user;
// `address` gives error; `permanentAddress` works
```

#### Nested destructuring

```javascript
const user = {
  department: {
    address: {
      city: "Bangalore",
    },
  },
};

const {
  department: {
    address: { city },
  },
} = user;
```

#### Array destructuring with rest

```javascript
const [firstTool, ...rest] = ["hammer", "screwdriver", "wrench"];
console.log(firstTool); // "hammer"
console.log(rest); // ["screwdriver", "wrench"]
```

#### Spread with nested update

```javascript
const updated = {
  ...user,
  department: {
    ...user.department,
    number: 7,
  },
};
```

#### Omitting a property

```javascript
const { age, ...rest } = user;
console.log(age, rest);
```

---

### Arrow Functions

```javascript
// Traditional
let add = function (x, y) {
  return x + y;
};

// Arrow
let add = (x, y) => x + y;

console.log(add(10, 20)); // 30
```

---

### Function Statement vs Expression

#### Function Statement (Declaration) -- can be hoisted

```javascript
a(); // No Error -- hoisted
function a() {
  console.log("a called");
}
```

#### Function Expression -- cannot be hoisted

```javascript
b(); // Gives Error
var b = function () {
  console.log("b called");
};
```

> The main difference is **hoisting**.

#### Named Function Expression

```javascript
var b = function c() {
  console.log("b called");
};

b(); // No Error
c(); // ReferenceError
```

#### First-Class Functions

The ability of a function to be used as a value, passed as an argument, and returned from another function.

---

### Miscellaneous JS Gotchas

#### Block-scoped variables cannot be redeclared

```javascript
let myVar = 10;
// let myVar = () => console.log(myVar); // SyntaxError
```

#### Array length with sparse elements

```javascript
const arr = [4, 5, 6, 7];
arr[100] = 121;
console.log(arr.length); // 101
arr.length = 0;
console.log(arr); // []
console.log(arr[20]); // undefined
```

#### Semicolon after for loop

```javascript
// With semicolon after for -- loop body is empty
const length = 4;
const numbers = [];
for (var i = 0; i < length; i++); {
  numbers.push(i + 1);
}
console.log(numbers); // [5]

// Without semicolon -- normal loop
const length2 = 4;
const numbers2 = [];
for (var j = 0; j < length2; j++) {
  numbers2.push(j + 1);
}
console.log(numbers2); // [1, 2, 3, 4]
```

#### delete operator on objects

```javascript
(function test(x) {
  delete x.prop1;
  return x;
})({ prop1: 10, prop2: 20 });
// Result: { prop2: 20 }
```

---

### API Calls: fetch vs axios

> `fetch` is part of the JS Window Object while `axios` is a JS library for making HTTP requests.
>
> - `fetch()` uses the `body` property for POST; `axios` uses the `data` property.
> - Request data in `fetch()` must be stringified with `JSON.stringify()`; `axios` does it automatically.
> - In `fetch()`, you must call `response.json()` to parse; `axios` gives you the parsed data directly.

#### Axios GET

```javascript
useEffect(() => {
  axios
    .get("http://dummy.restapiexample.com/api/v1/employees")
    .then((response) => {
      setData(response.data);
    })
    .catch((err) => {
      console.log(err);
    });
}, []);
```

#### Axios POST

```javascript
useEffect(() => {
  axios({
    method: "post",
    url: "http://dummy.restapiexample.com/api/v1/employees",
    timeout: 4000,
    data: {
      firstName: "abc",
      lastName: "def",
    },
  })
    .then((response) => {
      setData(response.data);
    })
    .catch((err) => {
      console.log(err);
    });
}, []);
```

#### Fetch GET

```javascript
useEffect(() => {
  fetch("http://dummy.restapiexample.com/api/v1/employees")
    .then((response) => response.json())
    .then((data) => {
      setData(data);
    })
    .catch((err) => {
      console.log(err);
    });
}, []);
```

#### Fetch POST

```javascript
useEffect(() => {
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      firstName: "abc",
      lastName: "def",
    }),
  };

  fetch("http://dummy.restapiexample.com/api/v1/employees", options)
    .then((response) => response.json())
    .then((data) => {
      setData(data);
    })
    .catch((err) => {
      console.log(err);
    });
}, []);
```

---

### setInterval and setTimeout

```jsx
const list = ["a", "b", "c", "d", "e"];

const [finalData, setFinal] = useState([]);
const [count, setCount] = useState(0);
let timer = useRef();

useEffect(() => {
  timer.current = setInterval(
    () => setCount((prev) => prev + 1),
    2000
  );
}, []);

useEffect(() => {
  list.forEach((item, index) => {
    index === count - 1 &&
      setFinal([...finalData, <li key={index}>{item}</li>]);
  });
  if (count > list.length) {
    clearInterval(timer.current);
  }
}, [count]);
```

#### setInterval vs setTimeout

```html
<div class="container">
  Time now is <span id="time"></span>
</div>

<script>
  function displayTime() {
    time = new Date();
    document.getElementById("time").innerHTML = time;
  }
  const intervalId = setInterval(displayTime, 1000);

  function greet() {
    console.log("Hello Good Morning");
  }
  const timeoutId = setTimeout(greet, 5000);

  clearInterval(intervalId);
  clearTimeout(timeoutId);
</script>
```

---

### React Strict Mode

In development mode, if your app is wrapped in `<React.StrictMode>`, React will:

1. Mount component
2. Run `useEffect`
3. Immediately unmount component
4. Mount again
5. Run `useEffect` again

This is to detect bugs like side effects not being cleaned up, memory leaks, and unsafe operations. **In production, it runs only once.**

---

### Hook Order Rule

> **"Rendered more hooks than during the previous render"**

React relies on hook **order**, not names. If hooks are called conditionally, the order can change between renders.

**Problem:**

```jsx
// Early returns before hooks cause order changes
if (loading) return <p>Loading...</p>;
if (error) return <p>Error: {error}</p>;

const completedTodos = useMemo(() => {
  return todos.filter((data) => data.completed);
}, [todos]);
```

**Fix: Always call all hooks before any return statement.**

```jsx
function App() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [todos, setTodos] = useState([]);

  // All hooks called first
  const completedTodos = useMemo(() => {
    return todos.filter((data) => data.completed);
  }, [todos]);

  useEffect(() => {
    const fetchTodos = async () => {
      setLoading(true);
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/todos"
        );
        const todosJson = await response.json();
        setTodos(todosJson);
      } catch (error) {
        setError(error.message);
      } finally {
        setLoading(false);
      }
    };
    fetchTodos();
  }, []);

  // Conditional returns after hooks
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  if (!todos.length) return <p>No data found</p>;

  return (
    <>
      <h2>Completed: {completedTodos.length}</h2>
      <h2>In Progress: {todos.length - completedTodos.length}</h2>
    </>
  );
}
```

#### Common Bug: `event.target.value` always returns a string

```jsx
// BUG: "false" is truthy
<select value={completed}>
  <option value="true">Done</option>
  <option value="false">Not Done</option>
</select>

// FIX: Convert to boolean
onChange={(e) =>
  setNewTodoData((prev) => ({
    ...prev,
    completed: e.target.value === "true",
  }))
}
```

---

### Common React Facts

- **Babel** is a Compiler + Transpiler.
- React components return a **single element**.
- **Props** are passed into other components.
- `webpack-dev-server` runs on port **8080** by default.
- Lifecycle hook to stop updating: `shouldComponentUpdate()`.
- Calling `setState()` inside `render()` creates an infinite loop.
- All React components must act like **pure functions** with respect to their props.
- `ref` is used to directly access the DOM node.
- `Array.map()` receives a callback function called once for each element.
- Component names must be **capitalized**.
- Keys in a list must be **unique among siblings** only.
- The smallest building block of ReactJS is **components**.
- `setState` is **asynchronous** in nature.
- Arbitrary inputs of components are **props**.
- `ReactDOM.render()` renders React content into an HTML page.

---

## DOM Manipulation

### Selecting Elements

```javascript
// Root nodes
console.log(document.documentElement);
console.log(document.body);
console.log(document.head);

// Children
console.log(document.body.childNodes);
console.log(document.body.children);
console.log(document.body.firstElementChild);
console.log(document.body.lastElementChild);
const childrensOfBody = Array.from(document.body.children);

// Siblings
const secondLi = ulTag.children[1];
console.log(secondLi.previousElementSibling.textContent);

// Search by ID, Class, Tag
const element = document.getElementById("element");
const listItems = document.getElementsByClassName("list-item");
console.log(document.getElementsByTagName("table"));
const items = document.querySelectorAll("ul > li:nth-child(2)");
```

### Table DOM Manipulation

```javascript
const tableTag = document.body.children[1];
tableTag.tBodies[0].rows[0].cells[1].style =
  "background-color:blue;";
```

### Table HTML

```html
<table>
  <thead>
    <tr>
      <th>Month</th>
      <th>Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>January</td>
      <td>$10,000</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$12,000</td>
    </tr>
  </tbody>
</table>
```

### Attributes

```javascript
console.log(element.getAttribute("data"));
element.setAttribute("order-placed", "pending");
element.removeAttribute("order-placed");
element.hasAttribute("order-placed");
console.log(element.attributes);
```

### Creating and Removing Elements

```javascript
const newDiv = document.createElement("div");
newDiv.innerHTML = `<ul id="element">
  <li class="list-item">First element</li>
  <li class="list-item">Second element</li>
</ul>`;
const newText = document.createTextNode("Namaste World");
newDiv.appendChild(newText);
body.append(newDiv);
body.prepend(newDiv);
body.before(newDiv);
body.after(newDiv);
firstDiv.replaceWith(newDiv);
firstDiv.remove();

document.getElementById("abc").innerHTML += "<div>Hii</div>";
```

### Manipulating Classes

```javascript
body.className = "second page";
body.classList.add("new");
body.classList.remove("new");
body.classList.toggle("new");
console.log(body.classList);
```

### Manipulating Styles

```javascript
body.style.color = "red";
body.style["background-color"] = "orange";
body.style.margin = "200px";
```

### Events

```javascript
function callMe(event) {
  console.log(event.type);
  console.log(event.currentTarget);
  console.log(event.clientY);
  console.log(event.clientX);
}

clickBtn.onclick = callMe;

clickBtn.addEventListener("click", callMe, (useCapture = false));
clickBtn.removeEventListener("click", callMe);
```

---

## HTML

### Document Structure

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
    <link rel="shortcut icon" href="/favicon.png" />
    <meta
      name="viewport"
      content="width=device-width,initial-scale=1.0"
    />
    <meta name="description" content="" />
    <meta name="author" content="Author Name" />
    <meta name="robots" content="index, follow" />
    <!-- Open Graph -->
    <meta property="og:title" content="Title" />
    <meta property="og:description" content="Description" />
    <meta property="og:type" content="website" />
    <meta property="og:url" content="https://example.com/" />
    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:title" content="Title" />
  </head>
  <body>
    <p>Content</p>
  </body>
</html>
```

### Comments

```html
<!-- Write your comments here -->
```

### Media Elements

#### Video

```html
<video width="320" height="240" autoplay controls>
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogg" type="video/ogg" />
  Your browser does not support the video tag.
</video>
```

#### Audio

```html
<audio controls autoplay muted>
  <source src="horse.ogg" type="audio/ogg" />
  <source src="horse.mp3" type="audio/mpeg" />
  Your browser does not support the audio element.
</audio>
```

#### YouTube Embed

```html
<iframe
  width="420"
  height="315"
  src="https://www.youtube.com/embed/tgbNymZ7vqY?controls=0&loop=0&autoplay=0&mute=0"
>
</iframe>
```

### Tables

```html
<table style="width:100%">
  <tr>
    <th>Company</th>
    <th>Contact</th>
    <th>Country</th>
  </tr>
  <tr>
    <td>Alfreds Futterkiste</td>
    <td>Maria Anders</td>
    <td>Germany</td>
  </tr>
</table>

<!-- colspan and rowspan -->
<th colspan="2">Name</th>
<th rowspan="2">Phone</th>
```

### Lists

```html
<!-- Ordered -->
<ol type="1|a|A|i|I" reversed></ol>

<!-- Unordered -->
<ul type="disc|circle|square|none"></ul>

<!-- Definition -->
<dl>
  <dt>Coffee</dt>
  <dd>- black hot drink</dd>
  <dt>Milk</dt>
  <dd>- white cold drink</dd>
</dl>
```

### Semantic Elements

`<article>`, `<aside>`, `<details>`, `<figcaption>`, `<figure>`, `<footer>`, `<header>`, `<main>`, `<mark>`, `<nav>`, `<section>`, `<summary>`, `<time>`

### Block-Level Elements

`<address>`, `<article>`, `<aside>`, `<blockquote>`, `<canvas>`, `<dd>`, `<div>`, `<dl>`, `<dt>`, `<fieldset>`, `<figcaption>`, `<figure>`, `<footer>`, `<form>`, `<h1>`-`<h6>`, `<header>`, `<hr>`, `<li>`, `<main>`, `<nav>`, `<noscript>`, `<ol>`, `<p>`, `<pre>`, `<section>`, `<table>`, `<tfoot>`, `<ul>`, `<video>`

### Inline Elements

`<a>`, `<abbr>`, `<b>`, `<br>`, `<button>`, `<cite>`, `<code>`, `<em>`, `<i>`, `<img>`, `<input>`, `<kbd>`, `<label>`, `<map>`, `<object>`, `<output>`, `<q>`, `<script>`, `<select>`, `<small>`, `<span>`, `<strong>`, `<sub>`, `<sup>`, `<textarea>`, `<time>`, `<var>`

### Forms

```html
<form
  action="/action_page.php"
  novalidate
  autocomplete="on"
  target="_blank"
  method="get"
>
  <label for="fname">First name:</label><br />
  <input
    type="text"
    id="fname"
    name=""
    value=""
    required
    autofocus
    readonly
    disabled
    min=""
    max=""
    placeholder=""
  /><br />

  <input type="radio" id="html" name="fav_language" value="HTML" />
  <label for="html">HTML</label><br />

  <label for="cars">Choose a car:</label>
  <select id="cars" name="cars">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
  </select>

  <textarea name="message" style="width:200px; height:600px;">
    The cat was playing in the garden.
  </textarea>

  <button type="button" onclick="alert('Hello World!')">
    Click Me!
  </button>

  <input type="submit" value="Submit" />
</form>
```

#### Input Types

`button`, `checkbox`, `color`, `date`, `email`, `file`, `hidden`, `image`, `month`, `number`, `password`, `radio`, `range`, `reset`, `search`, `submit`, `tel`, `text`, `time`, `url`

### Non-breaking Space

```html
&nbsp;
```

---

## CSS and Sass

### Center a Div

```css
body {
  display: flex;
  align-items: center;
  justify-content: center;
}

div {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid red;
  width: 100px;
  height: 100px;
  text-align: center;
}
```

### Flexbox

```css
.flex-container {
  display: flex;
  flex-direction: row;
}

@media (max-width: 800px) {
  .flex-container {
    flex-direction: column;
  }
}
```

Reference: [Flex Cheatsheet](https://yoksel.github.io/flex-cheatsheet/#section-flex-wrap)

### CSS Specificity

From highest to lowest:

1. **Inline styles** -- `<h1 style="color: pink;">`
2. **Internal styles** -- `<style></style>`
3. **External styles**
   1. **IDs** -- `#navbar`
   2. **Classes, pseudo-classes, attribute selectors** -- `.test`, `:hover`, `[href]`
   3. **Elements and pseudo-elements** -- `h1`, `::before`

### CSS Animation

```css
@keyframes example {
  0% {
    background-color: red;
    left: 0px;
    top: 0px;
  }
  25% {
    background-color: yellow;
    left: 200px;
    top: 0px;
  }
  50% {
    background-color: blue;
    left: 200px;
    top: 200px;
  }
  75% {
    background-color: green;
    left: 0px;
    top: 200px;
  }
  100% {
    background-color: red;
    left: 0px;
    top: 0px;
  }
}

div {
  width: 100px;
  height: 100px;
  position: relative;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
}
```

### Sass Features

#### 1. Variables

```scss
$myFont: Helvetica, sans-serif;
$myColor: red;
$myFontSize: 18px;
$myWidth: 680px;

body {
  font-family: $myFont;
  font-size: $myFontSize;
  color: $myColor;
}

#container {
  width: $myWidth;
  margin-#{$position}: 10px;
}
```

#### 2. Nesting

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

#### 3. @import and @use

```scss
@import "variables";
@import "colors";
@use "../../public/colors" as *; // File Name with _colors.scss
```

#### 4. @mixin

```scss
@mixin bordered($color: blue, $width: 1px) {
  border: $width solid $color;
}

.myArticle {
  @include bordered(blue, 1px);
}
```

#### 5. @extend

```scss
.button-basic {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button-report {
  @extend .button-basic;
  background-color: red;
}
```

#### 6. Selectors

```scss
a {
  font-size: 20px;
  &:hover {
    background-color: yellow;
  }
}

p:after {
  content: "I have #{8 + 2} books on SASS!";
}

input {
  padding: 0;
  &::placeholder {
    color: red;
  }
}
```

#### 7. Conditions

```scss
$type: audi;
p {
  @if $type == benz {
    color: red;
  } @else if $type == mahindra {
    color: blue;
  } @else if $type == audi {
    color: green;
  } @else {
    color: black;
  }
}
```

### Bootstrap to Tailwind

```html
<!-- Bootstrap -->
<div class="d-flex flex-row">
  <div class="p-2">Flex item 1</div>
</div>

<!-- Tailwind -->
<div class="flex flex-row">
  <div class="p-2">Flex item 1</div>
</div>
```
