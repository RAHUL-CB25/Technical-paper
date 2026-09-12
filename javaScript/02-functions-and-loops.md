# JavaScript Functions and Loops

## 1. Different Ways of Declaring a Function
Function declaration:
```javascript
function add(a, b) {
    return a + b;
}
```
Function expression:

```javascript
const add = function (a, b) {
    return a + b;
};
```
Arrow function:

```javascript
const add = (a, b) => {
    return a + b;
};
```
Short arrow function:

```javascript
const add = (a, b) => a + b;
```

## 2. Pass by Value and Reference

Pass by Value → A copy of the value is passed, so changing it does not affect the original.

Pass by Reference → A reference to the same object is passed, so changes can affect the original object.
```


```javascript
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
console.log(b); // 20
```

Objects contain references to objects:

```javascript
let user1 = {
    name: "Rahul"
};

let user2 = user1;

user2.name = "Raj";

console.log(user1.name); // Raj
```

Both variables refer to the same object.

## 3. for Loop

Used when you need control over the counter.

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

## 4. for...in

Usually used to iterate over object keys.

```javascript
const user = {
    name: "Rahul",
    age: 25
};

for (const key in user) {
    console.log(key, user[key]);
}
```

## 5. for...of

Used to iterate over iterable values.

```javascript
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number);
}
```

## 6. while Loop

Runs while a condition remains true.

```javascript
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

## 7. forEach

Runs a callback for every array element.

```javascript
const numbers = [10, 20, 30];

numbers.forEach(number => {
    console.log(number);
});
```

## 8. Passing Functions to Other Functions

Functions can be passed as arguments.

```javascript
function greet() {
    console.log("Hello");
}

function execute(callback) {
    callback();
}

execute(greet);
```

This concept is heavily used with higher-order functions.

## 9. Named and Anonymous Functions

Named function:

```javascript
function greet() {
    console.log("Hello");
}
```

Anonymous function:

```javascript
const greet = function () {
    console.log("Hello");
};
```

Named functions are easier to identify while debugging.

Anonymous functions are commonly used as callbacks.

## 10. Variable Number of Arguments

Rest parameters collect multiple arguments into an array.

```javascript
function sum(...numbers) {
    return numbers.reduce(
        (total, number) => total + number,
        0
    );
}

console.log(sum(10, 20, 30));
```

## 11. Closures

A closure happens when an inner function remembers variables from its outer scope.

```javascript
function counter() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}

const increment = counter();

console.log(increment()); // 1
console.log(increment()); // 2
```

The inner function remembers count.

## 12. Arrow Functions vs Regular Functions

Arrow functions differ from regular functions mainly in their handling of this, arguments and constructors.

Regular function:

```javascript
function greet() {
    console.log(this);
}
```

Arrow function:

```javascript
const greet = () => {
    console.log(this);
};
```

Important differences:

* Arrow functions do not have their own this
* Arrow functions do not have their own arguments object
* Arrow functions cannot be used as constructors
* Regular functions can have their own this depending on how they are called

## 13. Function Hoisting

Function declarations can be called before their declaration.

```javascript
greet();

function greet() {
    console.log("Hello");
}
```

Function expressions and arrow functions assigned to let or const cannot normally be used before initialization.

## 14. Function Return Value

If a function does not return anything, its result is undefined.

```javascript
function test() {
    console.log("Testing");
}

console.log(test());
// Testing
// undefined
```
