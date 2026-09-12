# JavaScript Fundamentals

## 1. Different Data Types in JavaScript
JavaScript has 8 main data types.

Primitive types:
- String – text values
- Number – numeric values
- Boolean – true/false values
- Undefined – missing value
- Null – empty value
- BigInt – large integers
- Symbol – unique identifiers
Non-primitive:
- Object – key-value collection

Arrays and functions are also objects in JavaScript.

````javascript
let name = "Rahul";
let age = 25;
let active = true;
let city;
let value = null;
let big = 123n;
let user = { name: "Rahul" };
````
## 2. Scope in JavaScript
Scope defines where a variable can be accessed.
Main types:
* Global scope
* Function scope
* Block scope
```javascript
let x = 10;

function test() {
    let y = 20;

    console.log(x);
    console.log(y);
}

test();
````

Block scope applies to let and const.

```javascript
let x = 10;

{
  let x = 20;
  console.log(x); // 20
}

console.log(x); // 10
```

## 3. let, var and const

```javascript
var a = 10;
let b = 20;
const c = 30;
```

var:
- Function scoped
- Can be redeclared
- Can be reassigned
- Hoisted with undefined

let:

- Block scoped
- Cannot be redeclared in the same scope
- Can be reassigned
- Has Temporal Dead Zone

const:

- Block scoped
- Cannot be redeclared
- Cannot be reassigned
- Has Temporal Dead Zone

## 4. Why We Should Not Use var
var is function scoped and can create unexpected behavior.

```javascript
var x = 10;

if (true) {
  var x = 20;
}

console.log(x); // 20
```

With let:

```javascript
let x = 10;

if (true) {
  let x = 20;
}
console.log(x); // 10
```

Modern JavaScript generally prefers let and const.

## 5. Why Global Variables Are Bad
Global variables can be accessed and modified from many places.

Problems:
- Difficult to track changes
- Can accidentally be overwritten
- Creates dependencies
- Makes debugging harder
Prefer local variables and pass values through functions.

## 6. Truthy and Falsy Values

Falsy values include:

```text
false
0
-0
""
null
undefined
NaN
```

Everything else is generally truthy.

```javascript
if ("hello") {
  console.log("Truthy");
}

if (0) {
  console.log("This will not execute");
}
```

## 7. Function Hoisting

Function declarations are hoisted.

```javascript
sayHello();

function sayHello() {
  console.log("Hello");
}
```

Function expressions do not behave the same way.

```javascript
const sayHello = function () {
  console.log("Hello");
};
```

## 8. Function Without a Return Statement

A function without a return statement returns undefined.

```javascript
function greet() {
  console.log("Hello");
}

let result = greet();

console.log(result); // undefined
```

## 9. === vs ==

=== checks value and type.

== can convert values to the same type before comparing them


```javascript
5 == "5"; // true
5 === "5"; // false
```

Prefer === in modern JavaScript.

## 10. undefined vs !value

Use === undefined when specifically checking for undefined.

```javascript
if (value === undefined) {
  console.log("Value is undefined");
}
```

!value checks all falsy values.

```javascript
if (!value) {
  console.log("Value is falsy");
}
```

## 11. null vs undefined
undefined generally means no value has been assigned.
null generally represents an intentionally empty value.

```javascript
let a;
let b = null;

console.log(a); // undefined
console.log(b); // null
```

## 12. Spread Operator

Spread expands elements of an iterable.

```javascript
const a = [1, 2];
const b = [...a, 3, 4];

console.log(b); // [1, 2, 3, 4]
```

For objects:

```javascript
const user = {
  name: "Rahul",
};

const updatedUser = {
  ...user,
  age: 25,
};
```

## 13. Template Literals

Template literals make string creation easier.

```javascript
const name = "Rahul";
const age = 25;

console.log(`My name is ${name} and I am ${age} years old.`);
```

## 14. Default Parameters

Default parameters provide a value when an argument is undefined.

```javascript
function greet(name = "Guest") {
  console.log(`Hello ${name}`);
}

greet();
greet("Rahul");
```

Destructuring is a way to extract values from arrays or properties from objects into variables.


## 15. Destructuring

Destructuring is a way to extract values from arrays or properties from objects into variables.

Array destructuring:

```javascript
const numbers = [10, 20];

const [first, second] = numbers;
````

Object destructuring:

```javascript
const user = {
    name: "Rahul",
    age: 25
};

const { name, age } = user;
```

