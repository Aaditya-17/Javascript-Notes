An **Immediately Invoked Function Expression (IIFE)** is a JavaScript function that is executed immediately after it is defined. It allows you to execute code in its own scope without polluting the global scope.

---

### **Syntax of IIFE**

IIFEs are created using the following syntax:

1. **Standard Syntax**:

    ```javascript
    (function () {
        // Code to execute
        console.log("IIFE executed!");
    })();
    ```

2. **Alternative Syntax**:
    ```javascript
    (function () {
        console.log("IIFE executed again!");
    })();
    ```

In both cases:

-   The function is wrapped in parentheses `( ... )` to make it an **expression** rather than a declaration.
-   It is immediately invoked with `()` after the function definition.

---

### **Purpose of IIFE**

1. **Avoid Polluting the Global Scope**:

    - Variables inside an IIFE are not accessible outside of it, preventing conflicts with other code.
    - Example:

        ```javascript
        (function () {
            var localVar = "I'm local!";
            console.log(localVar); // Outputs: I'm local!
        })();

        console.log(localVar); // Error: localVar is not defined
        ```

2. **Encapsulation**:

    - It provides a private scope to variables, which is useful for modularizing code.

3. **Initialize Code**:

    - Useful for code that should run immediately after being loaded.
    - Example:
        ```javascript
        (function () {
            console.log("Initializing application...");
        })();
        ```

4. **Avoid Name Collisions**:
    - Helps prevent variable name clashes in projects with multiple scripts.

---

### **Parameters in IIFE**

IIFEs can accept arguments:

```javascript
(function (name) {
    console.log("Hello, " + name + "!");
})("Nate");
```

-   Output: `Hello, Nate!`

---

### **Example Use Cases**

1. **Module Pattern**:

    - Encapsulate logic and expose only the necessary parts:

        ```javascript
        var myModule = (function () {
            var privateVar = "I'm private!";
            function privateFunction() {
                return "Accessed private function!";
            }

            return {
                publicMethod: function () {
                    return privateFunction() + " and exposed!";
                },
            };
        })();

        console.log(myModule.publicMethod());
        // Output: "Accessed private function! and exposed!"
        ```

2. **Creating a Singleton**:

    - To create a single instance of an object:

        ```javascript
        var singleton = (function () {
            var instance;

            function createInstance() {
                return { name: "I am the instance" };
            }

            return {
                getInstance: function () {
                    if (!instance) {
                        instance = createInstance();
                    }
                    return instance;
                },
            };
        })();

        console.log(singleton.getInstance());
        ```

3. **Initialization Code**:
    - For setting up event listeners or configuration:
        ```javascript
        (function () {
            console.log("Page loaded!");
            document.querySelector("body").style.background = "lightblue";
        })();
        ```

---

### **Key Features of IIFE**

-   **Immediate Execution**: Runs as soon as it is defined.
-   **Private Scope**: Variables inside are inaccessible from the outside.
-   **Encapsulation**: Great for creating isolated environments in JavaScript.

---

### **Why Use IIFE?**

IIFEs were heavily used before the introduction of ES6 modules to manage scoping issues in JavaScript. They remain useful in legacy codebases and for creating isolated execution contexts in certain scenarios.
