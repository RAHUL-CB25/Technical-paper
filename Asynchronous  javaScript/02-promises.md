# Promises

## 1. What is a Promise?

 A Promise object represents "the eventual completion (or failure) of an asynchronous operation and its resulting value."
## 2. Promise states

A Promise is always in one of three states:
- Pending: the work is not finished yet.
- Fulfilled: the work finished successfully.
- Rejected: the work failed.

A Promise starts as pending and can change only once, to fulfilled or rejected. After that it never changes again.

---

## 3. Creating a Promise

We use the Promise constructor. It gives us two functions, resolve and reject.

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Operation successful");
  } else {
    reject("Operation failed");
  }
});
```

- resolve() means success
- reject() means failure

---

## 4. Consuming a Promise

We use .then() for success and .catch() for errors.

```javascript
promise
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(error);
  });
```

---

## 5. Promise Chaining

If one async task needs the result of the previous one, we can chain .then() calls.

```javascript
getUser()
  .then((user) => {
    return getOrders(user.id);
  })
  .then((orders) => {
    return getPayment(orders[0].id);
  })
  .then((payment) => {
    console.log(payment);
  })
  .catch((error) => {
    console.log(error);
  });
```

Each .then() gets the value returned by the .then() before it. This looks much cleaner than callback hell.

---

## 6. Error handling with .catch()

.catch() handles two things:

- a rejected Promise
- an error thrown inside a .then()

```javascript
Promise.resolve("Start")
  .then((data) => {
    throw new Error("Something went wrong");
  })
  .catch((error) => {
    console.log(error.message);
  });
```

Output:

```text
Something went wrong
```

If there is no .catch(), the error becomes an unhandled rejection.

Why put .catch() at the end?

One .catch() at the end can catch an error from any step in the chain, so we don't need to write error handling again and again.

```javascript
getUser()
  .then(getOrders)
  .then(getPayment)
  .then(processPayment)
  .catch((error) => {
    console.log("Error:", error);
  });
```

---

## 7. finally()

.finally() runs at the end whether the Promise succeeded or failed. It is good for cleanup work, like hiding a loading spinner.

```javascript
getData()
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("Operation completed");
  });
```
