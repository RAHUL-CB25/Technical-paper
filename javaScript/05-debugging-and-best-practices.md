# JavaScript Debugging and Best Practices

## 1. Reading Error Messages and Stack Traces

When an error happens, check:

1. Error type
2. Error message
3. File name
4. Line number
5. Function call sequence

Example:

```text
TypeError: Cannot read properties of undefined
    at getUser (app.js:10)
    at main (app.js:20)
```

Start from the reported line and trace backward through the function calls.

## 2. Debugging Strategies

When code does not work:

1. Read the error message.
2. Check the file and line number.
3. Identify the error type.
4. Check variables and arguments.
5. Check return values.
6. Use console.log when needed.
7. Break the problem into smaller parts.
8. Check MDN if needed.

Example:

```javascript
function calculateTotal(price, quantity) {
    console.log(price, quantity);

    return price * quantity;
}

## 3. Searching MDN

MDN stands for Mozilla Developer Network.

It is an important reference for JavaScript and web APIs.

Useful searches:

```text
MDN Array.map
MDN Array.reduce
MDN Array.splice
MDN JavaScript closures
MDN try catch
```

When checking a method, look at:

* Syntax
* Parameters
* Return value
* Examples
* Whether it modifies the original value

## 4. Daily Debugging Practice

Practice debugging different examples daily for two weeks.

Focus on:

- SyntaxError
- TypeError
- ReferenceError
- Undefined values
- Wrong arguments
- Wrong return values
- Array and scope issues
## 5. JavaScript Best Practices

Use meaningful variable names.

Good:

```javascript
const totalPrice = 5000;
```

Avoid:

```javascript
const x = 5000;
```

Use camelCase for variables and functions.

```javascript
const firstName = "Rahul";

function calculateTotal() {
}
```

Use PascalCase for classes.

```javascript
class Car {
}
```

Prefer const when reassignment is not required.

```javascript
const name = "Rahul";
let age = 25;
```

Keep indentation consistent.

```javascript
if (age >= 18) {
    console.log("Adult");
}
```

Use meaningful loop variable names.

Good:

```javascript
for (const user of users) {
    console.log(user.name);
}
```

Avoid unclear names:

```javascript
for (const x of users) {
    console.log(x.name);
}
```

Other important practices:

* Avoid unnecessary global variables
* Keep functions small
* Avoid unnecessary mutation
* Prefer === over ==
* Use descriptive error messages
* Keep code readable
* Follow consistent naming and indentation




