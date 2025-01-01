### Notes on `ToPrimitive` Abstract Operation in JavaScript

#### **What is `ToPrimitive`?**

-   Converts an object to a primitive value.
-   Triggered when an object is used in a context that expects a primitive, like:
    -   Arithmetic operations (`+`, `-`, etc.)
    -   String concatenation (`+`)
    -   Template literals
    -   Comparisons (`<`, `>`, `==`)
-   Also triggered explicitly by `String()`, `Number()`, etc.

#### **When is `ToPrimitive` Invoked?**

-   Automatically in type coercion contexts.
-   Explicitly when primitive conversion is required.

#### **Steps in the Algorithm**

1. **Check if the input is already a primitive:**

    - Primitives: `string`, `number`, `bigint`, `boolean`, `symbol`, `null`, `undefined`.
    - If the value is primitive, return it.

2. **Use the "hint" to decide how to convert:**

    - Hints:
        - `"default"`: General context (e.g., `==`, `+`).
        - `"string"`: Preference for string (e.g., template literals).
        - `"number"`: Preference for number (e.g., arithmetic operations).

3. **Call object methods based on the hint:**

    - For `"string"`:
        - Call `toString()`. If it returns a primitive, use it.
        - Otherwise, call `valueOf()`.
    - For `"number"`:
        - Call `valueOf()`. If it returns a primitive, use it.
        - Otherwise, call `toString()`.

4. **If neither returns a primitive:**
    - Throw a `TypeError`.

#### **Examples**

##### **Default Hint**

```javascript
const obj = {
    toString() {
        return "hello";
    },
    valueOf() {
        return 42;
    },
};

console.log(obj + " world"); // 'hello world' (uses toString)
```

##### **Hint: `"number"`**

```javascript
const obj = {
    toString() {
        return "hello";
    },
    valueOf() {
        return 42;
    },
};

console.log(obj - 10); // 32 (uses valueOf)
```

##### **Hint: `"string"`**

```javascript
const obj = {
    toString() {
        return "hello";
    },
    valueOf() {
        return 42;
    },
};

console.log(String(obj)); // 'hello' (uses toString)
```

#### **Special Cases**

1. **Objects Without `toString` or `valueOf`:**

    ```javascript
    const obj = Object.create(null);
    console.log(obj + 1); // Throws TypeError
    ```

2. **Symbol Type:**
    ```javascript
    const sym = Symbol("test");
    console.log(sym + ""); // Throws TypeError
    ```

#### **Why is `ToPrimitive` Important?**

-   Allows custom control over object behavior in type coercion.
-   Essential for debugging and understanding JavaScript quirks.
-   Forms the foundation for type conversions in JavaScript.

**Notes on ToPrimitive and Related Concepts**

### What Does ToPrimitive Do?

**ToPrimitive** is an operation in JavaScript that converts an object into a primitive value (e.g., a string, number, or boolean). Primitive values are essential for operations like comparisons, arithmetic, or concatenations, where objects need to be converted to simpler forms.

1. **Purpose**:

    - Converts an object into a primitive value when needed.
    - Used for internal type coercion in JavaScript.

2. **Input**:

    - Can be any ECMAScript value (e.g., number, string, object).
    - If already a primitive value, it is returned as-is.

3. **Output**:

    - A primitive value such as a string, number, or boolean.
    - For objects, it tries to retrieve a primitive value using the object's methods.

4. **Algorithm**:
    - Check if the input is primitive. If so, return it.
    - Determine the "hint" (type suggestion):
        - If no hint is provided, use "default."
        - Hint "string" suggests a string representation.
        - Hint "number" suggests a numeric representation.
    - Check for a custom `@@toPrimitive` method and call it if present.
    - Fallback to **OrdinaryToPrimitive**, which uses `toString` or `valueOf` methods.

---

### What Does "Hint" Mean?

The **hint** guides how an object should be converted to a primitive value. It indicates the expected primitive type (e.g., `string` or `number`) depending on the context.

1. **Types of Hints**:

    - **"number"**: Suggests a numeric representation. Calls `valueOf()` first, then `toString()` if needed.
    - **"string"**: Suggests a string representation. Calls `toString()` first, then `valueOf()` if needed.
    - **"default"**: Behaves like "number" except for **Date objects**, where it behaves like "string."

2. **How Hints Are Determined**:

    - Explicit conversion functions (e.g., `String(obj)`, `Number(obj)`) directly set the hint.
    - Implicit contexts determine the hint based on the operation:
        - String concatenation: Hint = "string."
        - Arithmetic: Hint = "number."
        - Non-explicit contexts: Hint = "default."

3. **Customizing Behavior**:
   Objects can define custom conversion behavior using `Symbol.toPrimitive`. Example:
    ```javascript
    const obj = {
        [Symbol.toPrimitive](hint) {
            return hint === "string" ? "Custom string" : 42;
        },
    };
    console.log(String(obj)); // "Custom string"
    console.log(Number(obj)); // 42
    ```

---

### What Happens If the Preferred Type Is "Number" but Input Is a String?

1. **No Action for Primitive Strings**:

    - If the input is already a primitive string, **ToPrimitive** does not modify it.

2. **Numeric Conversion Happens Later**:

    - If a number is expected, JavaScript tries to coerce the string into a number outside of **ToPrimitive**.
    - Example:
        ```javascript
        console.log("42" * 1); // 42 (string coerced to number)
        console.log("hello" * 1); // NaN (invalid numeric conversion)
        ```

3. **Objects with Strings**:
    - If the input is an object containing a string, **ToPrimitive** is invoked to extract a primitive value before numeric coercion.

---

### What Happens If an Object Cannot Be Converted to a Primitive?

If an object **cannot be converted to a primitive value**, a **TypeError** is thrown. This occurs when:

1. **No `@@toPrimitive` Method**:

    - The object does not define a custom `@@toPrimitive` method or the method exists but does not return a primitive.

2. **Fallback Methods Fail**:

    - `toString()` and `valueOf()` methods either:
        - Do not exist.
        - Return non-primitive values (e.g., another object).

3. **Example of TypeError**:

    ```javascript
    const obj = {
        toString() {
            return this; // Returns itself, not a primitive
        },
        valueOf() {
            return this; // Returns itself, not a primitive
        },
    };
    console.log(Number(obj)); // TypeError: Cannot convert object to primitive value
    ```

4. **How to Avoid Errors**:
    - Define a `Symbol.toPrimitive` method for custom behavior.
        ```javascript
        const obj = {
            [Symbol.toPrimitive](hint) {
                return hint === "number" ? 42 : "Custom string";
            },
        };
        console.log(Number(obj)); // 42
        console.log(String(obj)); // "Custom string"
        ```
    - Ensure `toString()` or `valueOf()` methods return valid primitive values.

---
