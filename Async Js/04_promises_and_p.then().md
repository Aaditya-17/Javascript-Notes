### **What Are Promises in JavaScript?**

In JavaScript, a **Promise** is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. Promises provide a cleaner and more robust way to handle asynchronous code compared to callbacks.

---

### **Why Use Promises?**

Promises address the problems of **callback hell** and **inversion of control** by providing a structured way to handle asynchronous operations. They make the code more readable, maintainable, and easier to chain together for sequential tasks.

---

### **States of a Promise**

A Promise has three possible states:

1. **Pending**: The initial state, neither fulfilled nor rejected.
2. **Fulfilled**: The operation completed successfully, and the Promise has a resulting value.
3. **Rejected**: The operation failed, and the Promise has a reason for the failure (an error).

Once a Promise transitions to either the **fulfilled** or **rejected** state, it is considered "settled" and cannot change states.

---

### **Creating a Promise**

A Promise is created using the `Promise` constructor, which takes a function (called the **executor**) with two parameters:

-   **`resolve`**: A function to call when the Promise is fulfilled.
-   **`reject`**: A function to call when the Promise is rejected.

#### **Example:**

```javascript
const myPromise = new Promise((resolve, reject) => {
    const success = true;

    if (success) {
        resolve("Operation successful!");
    } else {
        reject("Operation failed!");
    }
});
```

---

### **Using Promises**

Promises are consumed using the `.then()`, `.catch()`, and `.finally()` methods:

-   **`then(onFulfilled)`**: Called when the Promise is resolved.
-   **`catch(onRejected)`**: Called when the Promise is rejected.
-   **`finally(callback)`**: Called regardless of whether the Promise is resolved or rejected.

#### **Example:**

```javascript
myPromise
    .then((result) => {
        console.log("Success:", result);
    })
    .catch((error) => {
        console.error("Error:", error);
    })
    .finally(() => {
        console.log("Promise settled.");
    });
```

---

### **Chaining Promises**

Promises can be chained to handle sequential asynchronous operations.

#### **Example:**

```javascript
const fetchData = () => {
    return new Promise((resolve) => {
        setTimeout(() => resolve("Data fetched!"), 1000);
    });
};

const processData = (data) => {
    return new Promise((resolve) => {
        setTimeout(() => resolve(`${data} Processed!`), 1000);
    });
};

fetchData()
    .then((data) => {
        console.log(data);
        return processData(data);
    })
    .then((processedData) => {
        console.log(processedData);
    })
    .catch((error) => {
        console.error("Error:", error);
    });
```

#### **Output:**

```
Data fetched!
Data fetched! Processed!
```

---

### **Promise Methods**

JavaScript provides utility methods for working with multiple Promises:

1. **`Promise.all(promises)`**:

    - Waits for all Promises to resolve.
    - Rejects if any Promise is rejected.

    ```javascript
    Promise.all([promise1, promise2, promise3]).then((results) => {
        console.log(results); // Array of results
    });
    ```

2. **`Promise.race(promises)`**:

    - Resolves or rejects as soon as the first Promise settles.

    ```javascript
    Promise.race([promise1, promise2]).then((result) => {
        console.log(result); // First settled Promise
    });
    ```

3. **`Promise.allSettled(promises)`**:

    - Waits for all Promises to settle (either resolve or reject).
    - Provides an array of results, including status.

4. **`Promise.any(promises)`**:
    - Resolves as soon as the first Promise is fulfilled.
    - Rejects only if all Promises are rejected.

---

### **Benefits of Promises**

1. **Avoids Callback Hell**: Promises flatten nested callbacks into a readable chain.
2. **Error Handling**: Errors can be caught in a single `.catch()` block.
3. **Composability**: Promises can be easily combined using `Promise.all`, `Promise.race`, etc.

---

### **Promises vs Callbacks**

| **Aspect**         | **Callbacks**                               | **Promises**                              |
| ------------------ | ------------------------------------------- | ----------------------------------------- |
| **Readability**    | Can lead to callback hell (deep nesting).   | Clean chaining with `.then()`.            |
| **Error Handling** | Errors need to be handled in each callback. | Centralized `.catch()` block.             |
| **Control Flow**   | Harder to manage complex flows.             | Easier with chaining and utility methods. |

---

Here is the organized and formatted version of the two code samples, with proper comments to make them easier to understand:

---

### **First Code Sample: Using `createPromiseWithTimeOut`**

```javascript
// Importing required module
const { reject } = require("async");

// Function to generate a random integer between 0 and num-1
function getrandomInt(num) {
    return Math.floor(Math.random() * num);
}

// Function to create a promise with a timeout
function createPromiseWithTimeOut() {
    return new Promise(function executor(resolve, reject) {
        console.log("Entering Executor Callback");

        // Simulate asynchronous operation using setTimeout
        setTimeout(function () {
            let no = getrandomInt(10); // Generate a random number
            console.log("Inside setTimeout");

            // Resolve or reject based on the random number
            if (no % 2 == 0) {
                resolve(no);
            } else {
                reject(no);
            }
        }, 3000);

        console.log("Exiting Executor Callback");
    });
}

// Execution starts here
console.log("Starting");

// Create a promise
const p = createPromiseWithTimeOut();

console.log("Waiting for promise to complete");
console.log("Currently, my promise object is:", p);

// Handling the promise resolution or rejection
p.then(
    function fulfillHandler(value) {
        console.log("Inside fulfill handler");
        console.log("Fulfilled with value:", value);
        console.log("My promise is:", p);
    },
    function rejectHandler(value) {
        console.log("Inside reject handler");
        console.log("Rejected with value:", value);
        console.log("My promise is:", p);
    }
);

// Another setTimeout for demonstration
setTimeout(() => {
    console.log("Another setTimeout");
}, 2000);
```

---

#### First Code Sample: **`createPromiseWithTimeOut`**

This example demonstrates how a promise works with asynchronous tasks, particularly `setTimeout`. Here's a step-by-step breakdown:

1. **Promise Creation:**

    - `createPromiseWithTimeOut` creates a new promise.
    - Inside the `Promise` constructor, the `executor` function is executed immediately.
    - Logs `Entering Executor Callback` and `Exiting Executor callback` are printed as part of synchronous code execution.

2. **Pending State:**

    - The promise starts in the **pending** state. A `setTimeout` is initiated, which will execute after 3 seconds.
    - The promise's `then` method registers two handlers (`fulfillHandler` and `rejectHandler`) for when the promise resolves or rejects.

3. **Resolution or Rejection:**

    - After 3 seconds, the `setTimeout` callback runs. It generates a random number.
    - If the number is even, the promise resolves (`resolve(no)`); if odd, it rejects (`reject(no)`).
    - The handlers registered via `.then` are added to the **microtask queue** based on the result.

4. **Promise State Changes:**

    - When the promise resolves or rejects, The handlers registered via `.then` are added to the **microtask queue** based on the result.
    - At this point, the promise transitions from `pending` to `fulfilled` or `rejected`.

5. **Output Order:**
    - "Starting" → "Entering Executor Callback" → "Exiting Executor callback" → "waiting for promise to complete" → `setTimeout` executes → "inside fulfill/reject handler".

**Key Takeaways:**

-   `.then` handlers are added to the microtask queue.
-   The microtask queue has higher priority than the macrotask queue (where `setTimeout` callbacks go).
-   The promise's state (`pending`, `fulfilled`, or `rejected`) can be inspected before and after it changes.

---

### **Second Code Sample: Using `createPromise`**

```javascript
// Function to create a promise
function createPromise() {
    return new Promise(function (resolve, reject) {
        console.log("Promise resolved");
        resolve("done"); // Resolves the promise immediately
    });
}

// Set a setTimeout task (macrotask)
setTimeout(function exec1() {
    console.log("Set Timeout Executed");
}, 0);

// Create the promise and immediately resolve it
const p = createPromise();

// Handling the fulfilled promise
p.then(function fulfillHandler(val) {
    console.log(val + " - Promise resolved inside fulfillHandler");
});

// Final synchronous code
console.log("Ending of global code");
```

---

#### Second Code Sample: **`createPromise`**

This example focuses on microtasks and demonstrates the priority of the **microtask queue** over the **macrotask queue**.

1. **Promise and `setTimeout` Creation:**

    - The `Promise` constructor logs `Promise resolved` immediately since it is synchronous.
    - `resolve("done")` is called, so the promise transitions to the **fulfilled** state.
    - The `then` handler (`fulfillHandler`) is registered in the **microtask queue**.

2. **`setTimeout` Callback:**

    - The `setTimeout` callback (`exec1`) is registered in the **macrotask queue**.
    - It logs "Set TimeOut Executed" when executed.

3. **Execution Order:**
    - The microtask queue (containing `fulfillHandler`) is processed before the macrotask queue (containing `exec1`).
    - Therefore, the output order is:
        - "Promise resolved" → "ending of global code" → "donePromise resolved inside fulfillHandler" → "Set TimeOut Executed".

---

### Summary of Differences:

| Feature                    | First Sample (`setTimeout`)                             | Second Sample (`microtasks`) |
| -------------------------- | ------------------------------------------------------- | ---------------------------- |
| **Promise Initialization** | Includes asynchronous resolution (`setTimeout`).        | Resolves immediately.        |
| **Handlers Registered**    | `fulfillHandler` and `rejectHandler`.                   | Only `fulfillHandler`.       |
| **Queues Involved**        | Both microtask (handlers) and macrotask (`setTimeout`). | Microtask and macrotask.     |
| **Execution Priority**     | Microtasks (handlers) execute before macrotasks.        | Same principle applies.      |
