# JavaScript Arrays, Strings and Objects

## 1. Array Basic Methods

### Array.pop
Removes the last element.

Mutable.

```javascript
const numbers = [1, 2, 3];

numbers.pop();

console.log(numbers); // [1, 2]
```

### Array.push

Adds elements to the end.

Mutable.

```javascript
const numbers = [1, 2];

numbers.push(3);

console.log(numbers); // [1, 2, 3]
```

### Array.concat

Combines arrays and returns a new array.

Immutable.

```javascript
const a = [1, 2];
const b = [3, 4];

const result = a.concat(b);

console.log(result); // [1, 2, 3, 4]
```

### Array.slice

Extracts part of an array.

Immutable.

```javascript
const numbers = [10, 20, 30, 40];

const result = numbers.slice(1, 3);

console.log(result); // [20, 30]
```
Exactly bro. Show all **three uses of splice() — add, remove, and replace** with simple examples:


### Array.splice

Used to add, remove, or replace elements in an array.

Mutable.

Syntax:

```javascript
array.splice(start, deleteCount, item1, item2, ...);
````

#### 1. Remove elements

```javascript
const numbers = [10, 20, 30, 40];

numbers.splice(1, 2);

console.log(numbers); // [10, 40]
```

#### 2. Add elements

```javascript
const numbers = [10, 20, 40];

numbers.splice(2, 0, 30);

console.log(numbers); // [10, 20, 30, 40]
```

#### 3. Replace elements

```javascript
const numbers = [10, 20, 30];

numbers.splice(1, 1, 25);

console.log(numbers); // [10, 25, 30]
```




### Array.join

Converts array elements into a string.

Does not modify the original array.

```javascript
const words = ["JavaScript", "is", "fun"];

console.log(words.join(" "));
// JavaScript is fun
```

### Array.flat

Flattens nested arrays.

Immutable.

```javascript
const numbers = [1, [2, 3], [4]];

console.log(numbers.flat());
// [1, 2, 3, 4]
```

## 2. Finding Methods

### Array.find

Returns the first matching element.

```javascript
const numbers = [10, 20, 30];

const result = numbers.find(number => number > 15);

console.log(result); // 20
```

### Array.indexOf

Returns the index of a value.

```javascript
console.log([10, 20, 30].indexOf(20));
// 1
```

### Array.includes

Checks whether a value exists.

```javascript
console.log([10, 20, 30].includes(20));
// true
```

### Array.findIndex

Returns the index of the first matching element.

```javascript
const index = [10, 20, 30].findIndex(
    number => number > 15
);

console.log(index); // 1
```

## 3. Higher-Order Array Methods

### Array.forEach

Performs an operation for every element.

```javascript
numbers.forEach(number => {
    console.log(number);
});
```

### Array.filter

Creates a new array containing matching elements.

Immutable.

```javascript
const result = [1, 2, 3, 4]
    .filter(number => number % 2 === 0);

console.log(result);
// [2, 4]
```

### Array.map

Creates a new array by transforming elements.

Immutable.

```javascript
const result = [1, 2, 3]
    .map(number => number * 2);

console.log(result);
// [2, 4, 6]
```

### Array.reduce

Reduces an array to one accumulated value.

```javascript
const total = [10, 20, 30].reduce(
    (sum, number) => sum + number,
    0
);

console.log(total);
// 60
```

### Array.sort

Sorts the array.

Mutable.

```javascript
const numbers = [30, 10, 20];

numbers.sort((a, b) => a - b);

console.log(numbers);
// [10, 20, 30]
```

## 4. Array Method Chaining

Multiple array methods can be combined.

```javascript
const result = [1, 2, 3, 4, 5]
    .filter(number => number % 2 === 1)
    .map(number => number * 2);

console.log(result);
// [2, 6, 10]
```

## 5. When to Use Array Methods

forEach:

Use when you only want to perform an action.

```javascript
users.forEach(user => {
    console.log(user.name);
});
```

map:

Use when you want a transformed array.

```javascript
const names = users.map(user => user.name);
```

filter:

Use when you want selected elements.

```javascript
const adults = users.filter(user => user.age >= 18);
```

reduce:

Use when you want one accumulated result.

```javascript
const total = numbers.reduce(
    (sum, number) => sum + number,
    0
);
```

Quick rule:

```text
forEach → perform an action
map     → transform
filter  → select
reduce  → accumulate
```

## 6. Mutable and Immutable Array Methods

Mutable methods modify the original array.

```text
push
pop
splice
sort
```

Non-mutating methods do not modify the original array.

```text
concat
slice
map
filter
flat
```

## 7. String Utility Methods

Strings are immutable.

### toUpperCase

```javascript
const name = "rahul";

console.log(name.toUpperCase());
// RAHUL
```

### toLowerCase

```javascript
console.log("HELLO".toLowerCase());
// hello
```

### trim

```javascript
const text = "  hello  ";

console.log(text.trim());
// hello
```

### includes

```javascript
console.log("JavaScript".includes("Script"));
// true
```

### startsWith

```javascript
console.log("JavaScript".startsWith("Java"));
// true
```

### endsWith

```javascript
console.log("JavaScript".endsWith("Script"));
// true
```

### slice

```javascript
console.log("JavaScript".slice(0, 4));
// Java
```

### replace

```javascript
console.log(
    "Hello World".replace("World", "JavaScript")
);
// Hello JavaScript
```

### split

```javascript
const result = "a,b,c".split(",");

console.log(result);
// ["a", "b", "c"]
```

String methods do not modify the original string.

## 8. Object Utility Methods

### Object.keys

Returns an array of object keys.

```javascript
const user = {
    name: "Rahul",
    age: 25
};

console.log(Object.keys(user));
// ["name", "age"]
```

### Object.values

Returns an array of object values.

```javascript
console.log(Object.values(user));
// ["Rahul", 25]
```

### Object.entries

Returns key-value pairs.

```javascript
console.log(Object.entries(user));
// [["name", "Rahul"], ["age", 25]]
```

### Object.assign

Copies properties into a target object.

It modifies the target object.

```javascript
const user = {
    name: "Rahul"
};

Object.assign(user, {
    age: 25
});

console.log(user);
// { name: "Rahul", age: 25 }
```

### Object.freeze

Prevents changes to an object at the top level.

```javascript
const user = {
    name: "Rahul"
};

Object.freeze(user);
```
