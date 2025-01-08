### **Understanding `async` and `await` in JavaScript**

`async` and `await` are modern JavaScript keywords used to work with asynchronous code in a simpler and more readable way. They are built on top of **promises** and provide a more synchronous-looking syntax to handle asynchronous operations.

---

### **How `async` and `await` Work**

1. **`async` Function**:

    - A function declared with the `async` keyword always returns a **promise**.
    - Inside the `async` function, you can use the `await` keyword to pause the execution until a promise is resolved or rejected.

    ```javascript
    async function example() {
        return "Hello, World!";
    }

    example().then(console.log); // Output: "Hello, World!"
    ```

2. **`await` Keyword**:

    - Used only inside `async` functions.
    - It pauses the execution of the `async` function until the promise resolves or rejects.
    - If the promise resolves, it returns the resolved value.
    - If the promise rejects, it throws the error, which can be caught using a `try...catch` block.

    ```javascript
    async function example() {
        const data = await Promise.resolve("Data loaded");
        console.log(data); // Output: "Data loaded"
    }

    example();
    ```

---

### **Key Characteristics**

1. **Sequential Execution**:

    - `await` makes it easier to read asynchronous code that needs to be executed in sequence.

    ```javascript
    async function sequentialTasks() {
        const data = await fetch("https://api.example.com/data");
        const json = await data.json();
        console.log(json);
    }
    ```

2. **Error Handling**:

    - Errors in asynchronous operations can be handled using `try...catch`.

    ```javascript
    async function handleErrors() {
        try {
            const response = await fetch("https://api.example.com/data");
            if (!response.ok) {
                throw new Error("Network error");
            }
            const json = await response.json();
            console.log(json);
        } catch (error) {
            console.error("Error occurred:", error.message);
        }
    }

    handleErrors();
    ```

3. **Parallel Execution**:

    - Use `Promise.all` with `await` for tasks that can run in parallel.

    ```javascript
    async function parallelTasks() {
        const [task1, task2] = await Promise.all([
            fetch("https://api.example.com/data1"),
            fetch("https://api.example.com/data2"),
        ]);

        console.log("Task1:", await task1.json());
        console.log("Task2:", await task2.json());
    }

    parallelTasks();
    ```

---

### **Benefits of `async` and `await`**

1. **Simpler Syntax**:

    - `async`/`await` provides a more readable, synchronous-like structure for asynchronous operations compared to `then()` and `catch()` chains.

2. **Better Error Handling**:

    - Errors are handled using `try...catch`, which is familiar and less error-prone than `.catch()`.

3. **Chaining Made Easy**:
    - Easier to work with complex workflows involving multiple asynchronous steps.

---

### **Comparison with Promises**

| **Aspect**      | **Promises**                         | **Async/Await**                    |
| --------------- | ------------------------------------ | ---------------------------------- |
| Syntax          | `then()` and `catch()` methods       | More synchronous-like with `await` |
| Error Handling  | `.catch()`                           | `try...catch`                      |
| Readability     | Harder to read for complex workflows | Cleaner and easier to read         |
| Execution Style | Callback-based chaining              | Sequential or parallel, as needed  |

---

### **Example Comparison**

#### **Using Promises**:

```javascript
fetch("https://api.example.com/data")
    .then((response) => response.json())
    .then((data) => console.log(data))
    .catch((error) => console.error(error));
```

#### **Using Async/Await**:

```javascript
async function fetchData() {
    try {
        const response = await fetch("https://api.example.com/data");
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

fetchData();
```

---

### **Best Practices**

1. **Always Use `try...catch`**:

    - Handle errors gracefully when using `await`.

2. **Combine Parallel and Sequential Logic**:

    - Use `Promise.all` for parallel operations and `await` for sequential logic.

3. **Avoid `await` in Loops**:

    - If possible, use `Promise.all` to execute multiple asynchronous tasks in parallel instead of waiting for each in a loop.

    ```javascript
    const urls = ["url1", "url2", "url3"];
    const fetchPromises = urls.map((url) =>
        fetch(url).then((res) => res.json())
    );
    const results = await Promise.all(fetchPromises);
    console.log(results);
    ```

---

---

### Example of Async await

```javascript
function download(url) {
    return new Promise(function (resolve, reject) {
        // Simulate a delay of 5 seconds for downloading
        setTimeout(function () {
            console.log("Data downloading from", url);
            const data = "aditya"; // Simulated downloaded data
            resolve(data); // Resolve the promise with the downloaded data
        }, 3000);
    });
}

// Function to simulate saving data into a file
function saveIntoFile(data) {
    return new Promise(function (resolve) {
        // Simulate a delay of 5 seconds for saving the file
        setTimeout(function () {
            const fileName = "test.txt"; // File name to save the data
            console.log(data, "saved in", fileName);
            resolve(fileName); // Resolve the promise with the file name
        }, 3000);
    });
}

// Function to simulate uploading a file to a given URL
function upload(url, fileName) {
    return new Promise(function (resolve) {
        // Simulate a delay of 5 seconds for uploading
        setTimeout(function () {
            console.log("Uploading", fileName, "to", url);
            resolve(fileName); // Resolve the promise with the file name
        }, 3000);
    });
}

async function steps() {
    const downloadData = await download("xyz.com");
    console.log("data downloaded is", downloadData);
    const fileWrritten = await saveIntoFile(downloadData);
    console.log("file wrritten is", fileWrritten);
    const uploadedfrom = await upload("abc.com", fileWrritten);
    console.log("file uploaded on", uploadedfrom);
    return uploadedfrom;
}

steps().then((value) => console.log("We have completed steps with", value));
console.log("outside");
for (let i = 0; i < 100000000; i++) {}
setTimeout(function () {
    console.log("Timer done");
}, 4000);
console.log("For loop ended");
```

---
