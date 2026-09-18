# Promise APIs and Error Handling

## 1. Handling multiple Promises

There are two situations:

- One task depends on the other: use chaining (one after another).
- Tasks are independent: run them together with a Promise API like Promise.all().

Chaining example (each step needs the previous result):

```javascript
getUser()
  .then((user) => getOrders(user.id))
  .then((orders) => getPayment(orders))
  .then((payment) => console.log(payment));
```

---

## 2. Promise.all()

Use it when all the promises must succeed.

```javascript
Promise.all([getUsers(), getProducts(), getOrders()])
  .then((results) => {
    console.log(results);
  })
  .catch((error) => {
    console.log(error);
  });
```

- All fulfilled: we get an array of all the results.
- Any one rejected: Promise.all() rejects.

---

## 3. Promise.allSettled()

Waits for all promises to finish, whether they succeed or fail.

```javascript
Promise.allSettled([Promise.resolve("Success"), Promise.reject("Failed")]).then(
  (results) => {
    console.log(results);
  },
);
```

We get the status of every promise. Useful when we want every result even if some fail.

---

## 4. Promise.any()

Gives us the first successful promise. Rejected ones are ignored.

```javascript
Promise.any([
  Promise.reject("Failed"),
  Promise.resolve("Success"),
  Promise.resolve("Another Success"),
]).then((result) => {
  console.log(result);
});
```

Output: Success

If all promises reject, it rejects with an AggregateError.

---

## 5. Promise.race()

Finishes as soon as the first promise settles, whether it is fulfilled or rejected.

```javascript
Promise.race([getData(), timeout()])
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
  });
```

A common use is adding a timeout: if the data takes too long, the timeout promise wins.

---

## 6. Promise.resolve() and Promise.reject()

Promise.resolve() creates a Promise that is already fulfilled.

```javascript
Promise.resolve("Hello").then((value) => {
  console.log(value);
});
```

Promise.reject() creates a Promise that is already rejected.

```javascript
Promise.reject("Something went wrong").catch((error) => {
  console.log(error);
});
```

---

## 7. Promisifying a callback function

Promisifying means changing a callback-based function into one that returns a Promise.

```javascript
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

wait(2000).then(() => {
  console.log("2 seconds completed");
});
```

Node.js also has ready-made Promise versions of many callback functions, for example fs/promises:

```javascript
import { readFile } from "fs/promises";

readFile("data.txt", "utf8")
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

---

## 8. Why is error handling important?

Async tasks can fail for many reasons:

- Network failure
- Invalid input
- File not found
- Server error
- Database failure

If we don't handle errors, the app can behave in unexpected ways and we get unhandled promise rejections.

A good pattern to follow:

```javascript
doSomething()
  .then((result) => doNextThing(result))
  .catch((error) => {
    console.log("Error:", error);
  })
  .finally(() => {
    console.log("Finished");
  });
```

- `Promise.all()` finishes when all promises succeed. If any one fails, it rejects right away.
- `Promise.allSettled()` finishes when all promises are done. It never rejects and gives the status of every promise.
- `Promise.any()` finishes when the first promise succeeds. It rejects only if all promises fail.
- `Promise.race()` finishes when the first promise settles. The result can be a success or a failure.
- Use chaining when one task needs the result of the previous task.
- Always add `.catch()` so errors don't go unhandled.
