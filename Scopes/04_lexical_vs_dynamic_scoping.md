In JavaScript, `var` and `let` are both used to declare variables, but they have significant differences in terms of scope, hoisting, and re-declaration. These differences were introduced with the addition of `let` and `const` in ES6 (2015) to improve the language's clarity and reduce potential bugs.

---

### **Key Differences Between `var` and `let`**

| Feature                   | `var`                                            | `let`                                            |
| ------------------------- | ------------------------------------------------ | ------------------------------------------------ |
| **Scope**                 | Function-scoped                                  | Block-scoped                                     |
| **Re-declaration**        | Can be re-declared in the same scope             | Cannot be re-declared in the same scope          |
| **Hoisting**              | Hoisted to the top of its scope (with undefined) | Hoisted but not initialized (temporal dead zone) |
| **Initialization**        | Optional, undefined by default                   | Optional, undefined by default                   |
| **Global Object Binding** | Becomes a property of the global object          | Does not become a property of the global object  |
| **Use Case**              | Legacy code, rarely recommended                  | Modern code, preferred for mutable variables     |

---

### **1. Scope**

-   **`var`: Function Scope**

    -   A variable declared with `var` is accessible throughout the entire function, even if declared inside a block.
    -   Example:

        ```javascript
        function testVar() {
            if (true) {
                var x = 10;
            }
            console.log(x); // Outputs: 10
        }

        testVar();
        ```

-   **`let`: Block Scope**

    -   A variable declared with `let` is only accessible within the block `{}` where it is defined.
    -   Example:

        ```javascript
        function testLet() {
            if (true) {
                let y = 20;
            }
            console.log(y); // Error: y is not defined
        }

        testLet();
        ```

---

### **2. Re-declaration**

-   **`var`: Allows Re-declaration**

    -   The same variable can be declared multiple times in the same scope without errors.
    -   Example:
        ```javascript
        var z = 30;
        var z = 40; // No error
        console.log(z); // Outputs: 40
        ```

-   **`let`: No Re-declaration**
    -   Redeclaring a variable in the same scope with `let` causes an error.
    -   Example:
        ```javascript
        let a = 50;
        let a = 60; // Error: Identifier 'a' has already been declared
        ```

---

### **3. Hoisting**

-   **`var`: Hoisted with `undefined`**

    -   Variables declared with `var` are hoisted to the top of their scope and initialized with `undefined`.
    -   Example:
        ```javascript
        console.log(b); // Outputs: undefined
        var b = 70;
        ```

-   **`let`: Hoisted but in a Temporal Dead Zone**
    -   Variables declared with `let` are hoisted but not initialized until their declaration is reached. Accessing them before declaration results in an error.
    -   Example:
        ```javascript
        console.log(c); // Error: Cannot access 'c' before initialization
        let c = 80;
        ```

---

### **4. Global Object Binding**

-   **`var`: Adds to Global Object**

    -   Variables declared with `var` in the global scope become properties of the global object (`window` in browsers).
    -   Example:
        ```javascript
        var d = 90;
        console.log(window.d); // Outputs: 90
        ```

-   **`let`: Does Not Add to Global Object**
    -   Variables declared with `let` in the global scope do not become properties of the global object.
    -   Example:
        ```javascript
        let e = 100;
        console.log(window.e); // Outputs: undefined
        ```

---

### **When to Use `var` or `let`**

-   **Use `let`**:
    -   For most modern JavaScript code.
    -   When you need a mutable variable that is block-scoped.
-   **Use `var`**:
    -   Rarely, typically in legacy codebases for compatibility reasons.

---

### **Example Comparison**

```javascript
function compareVarAndLet() {
    console.log(x); // undefined (hoisted)
    // console.log(y); // Error: Cannot access 'y' before initialization

    var x = 1;
    let y = 2;

    if (true) {
        var x = 3; // Affects the entire function
        let y = 4; // Scoped to this block only
        console.log(x, y); // Outputs: 3, 4
    }

    console.log(x); // Outputs: 3
    console.log(y); // Outputs: 2
}

compareVarAndLet();
```

---

In modern JavaScript, **`let`** (and `const`) is preferred over `var` due to its predictable scoping rules and reduced risk of bugs. Use `let` when you need a variable that might change, and `const` for variables that should remain constant.
