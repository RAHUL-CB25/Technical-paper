# JavaScript Errors and Modules

## 1. Error and Exception

An error is a problem in the program that causes the code to work incorrectly.

An exception is an error that happens while the program is running and can be handled using try...catch.

Common types of errors:

* SyntaxError → invalid syntax
* ReferenceError → variable does not exist
* TypeError → invalid operation or wrong type

Example:

```javascript
const user = undefined;

console.log(user.name);
// TypeError
```

## 2. Error Handling with try...catch

try contains code that may produce an error.

catch handles the error.

```javascript
try {
    const result = JSON.parse("invalid json");
} catch (error) {
    console.error(error.message);
}
```

## 3. Throwing Errors

throw is used to manually create an error condition.

```javascript
function withdraw(amount) {
    if (amount < 0) {
        throw new Error("Amount cannot be negative");
    }
}
```

## 4. throw new Error vs throw String

Preferred:

```javascript
throw new Error("Something went wrong");
```

Avoid:

```javascript
throw "Something went wrong";
```

Error objects provide useful information such as:

* message
* name
* stack trace

This makes debugging easier.

## 5. Importance of catch

catch provides a place to:

* Read the error
* Log useful information
* Recover from the error
* Show a useful message
* Prevent unexpected application failure

```javascript
try {
    riskyOperation();
} catch (error) {
    console.error("Operation failed:", error.message);
}
```

## 6. Importing and Exporting Modules

CommonJS uses module.exports and require.

Export:

```javascript
function add(a, b) {
    return a + b;
}

module.exports = add;
```

Import:

```javascript
const add = require("./add");

console.log(add(2, 3));
```

Modules help divide large applications into smaller reusable files.

## 7. Console Methods

console.log:

```javascript
console.log("Hello");
```

console.error:

```javascript
console.error("Something went wrong");
```

console.info:

```javascript
console.info("Server started");
```

Other useful methods:

```javascript
console.warn("Warning");

console.table(users);

console.time("operation");

console.timeEnd("operation");
```

