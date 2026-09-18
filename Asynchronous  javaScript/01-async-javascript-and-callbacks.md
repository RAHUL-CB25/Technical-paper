# Asynchronous JavaScript and Callbacks

## 1. How does JavaScript run code?

JavaScript is single-threaded. This means it can do only one thing at a time. To keep track of what to run, it uses the Call Stack.

How it works:

1. The global code is put on the Call Stack first.
2. When a function is called, it is pushed on top of the stack.
3. The function runs, and when it finishes, it is removed from the stack.

```javascript
function greet() {
  console.log("Hello");
}

console.log("Start");
greet();
console.log("End");
```

Output:

```text
Start
Hello
End
```

---

## 2. Synchronous vs Asynchronous

Synchronous means each line waits for the previous line to finish.

```javascript
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

Asynchronous means a slow task is started, and JavaScript moves on to the next lines without waiting for it.

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 2000);

console.log("C");
```

Output:

```text
A
C
B
```

So synchronous code blocks the next line, but asynchronous code lets other work continue.

Common ways to write async code:

- Callbacks
- Promises
- async/await
- Browser Web APIs (timers, network requests, DOM events)

---

## 3. Web Browser APIs

JavaScript itself can't run a timer or make a network request on its own. The browser provides these features, and JavaScript just uses them.

Examples:

- setTimeout()
- fetch()
- DOM events (like a button click)
- Geolocation API
- Web Storage API

In the setTimeout example above, the browser keeps the 2 second timer running. That is why JavaScript is free to run console.log("C") in the meantime.

---

## 4. The Event Loop

The Event Loop connects the Call Stack with the queues where asynchronous tasks wait to be executed.
```
Code
↓
Call Stack
↓
Browser APIs
↓
Microtask Queue or Callback Queue (Microtask has more priority)
↓
Event Loop
↓
Call Stack
```
The Event Loop keeps checking whether the Call Stack is empty.

If the Call Stack is empty, the Event Loop first processes the Microtask Queue. Promise callbacks have higher priority than callbacks in the Callback Queue.
1. Synchronous code runs in the Call Stack, while asynchronous operations are handled by Browser APIs.
2. Promise callbacks go to the Microtask Queue, while timers and events go to the Callback Queue.
3. When the Call Stack is empty, the Event Loop processes the Microtask Queue first, then the Callback Queue.



---

## 5. What is a callback?

A callback is a function that we pass into another function, so it can be called later.

```javascript
setTimeout(() => {
  console.log("Executed later");
}, 1000);
```

Here the arrow function is the callback.

---

## 6. Callback Hell

When many async tasks depend on each other, we end up putting callbacks inside callbacks inside callbacks.

```javascript
loginUser(() => {
  getUserData(() => {
    getOrders(() => {
      getPayment(() => {
        console.log("Done");
      });
    });
  });
});
```

The code keeps moving to the right like a pyramid. This is called callback hell.

Problems:

- Hard to read
- Hard to maintain
- Hard to debug
- Error handling gets messy

Promises were introduced to fix this.

---

## 7. Inversion of Control

When we pass a callback, we hand over control to another function. We are trusting it to call our callback properly.

```javascript
getData(function (data) {
  console.log(data);
});
```

We can't be sure what getData() will do with our callback. This is called inversion of control.

Things that can go wrong:

- It never calls our callback
- It calls it more than once
- It calls it with the wrong data
- It calls it at the wrong time

Promises solve this because we get back an object and decide ourselves how to handle the result.
