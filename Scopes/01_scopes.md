In JavaScript, **scope** determines the accessibility (visibility) of variables, functions, and objects in different parts of the code. Understanding scope is crucial for managing variable lifetimes and avoiding unexpected behaviors in your programs.

### Types of Scope in JavaScript

1. **Global Scope**

    - Variables or functions declared outside any function or block have global scope.
    - They are accessible from any part of the program.
    - Example:

        ```javascript
        var globalVar = "I'm global!";

        function test() {
            console.log(globalVar); // Accessible
        }

        test();
        console.log(globalVar); // Accessible
        ```

2. **Local Scope**

    - Variables declared within a function are local to that function and cannot be accessed outside of it.
    - Example:

        ```javascript
        function test() {
            var localVar = "I'm local!";
            console.log(localVar); // Accessible here
        }

        test();
        console.log(localVar); // Error: localVar is not defined
        ```

3. **Block Scope (ES6 and later)**

    - Variables declared with `let` or `const` inside a block (`{ }`) are only accessible within that block.
    - Example:

        ```javascript
        {
            let blockVar = "I'm block-scoped!";
            console.log(blockVar); // Accessible here
        }

        console.log(blockVar); // Error: blockVar is not defined
        ```

4. **Function Scope**

    - Variables declared with `var` are function-scoped, meaning they are accessible throughout the entire function where they are declared, even before their declaration due to **hoisting**.
    - Example:

        ```javascript
        function test() {
            if (true) {
                var funcVar = "I'm function-scoped!";
            }
            console.log(funcVar); // Accessible here
        }

        test();
        ```

5. **Lexical Scope**

    - Inner functions have access to variables in their outer (parent) functions, even after the parent function has executed.
    - Example:

        ```javascript
        function outer() {
            let outerVar = "I'm from the outer scope!";

            function inner() {
                console.log(outerVar); // Accessible here
            }

            inner();
        }

        outer();
        ```

### Key Points

-   **`var`**: Function-scoped, hoisted to the top of its scope.
-   **`let` and `const`**: Block-scoped, not hoisted in the same way as `var` (temporal dead zone).
-   **Global Variables**: Automatically become properties of the `window` object (in browsers) if declared with `var`, but not with `let` or `const`.
-   **Closures**: Lexical scoping allows inner functions to remember variables from their outer functions, creating closures.

Understanding these scopes helps you write clean, bug-free code and manage variables efficiently!
