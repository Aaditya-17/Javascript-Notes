# 1. 📦 **CommonJS** (Default Node.js system)

## a) Default Export in CommonJS

**math.js:**
```javascript
function add(a, b) {
  return a + b;
}

module.exports = add; // Default export
```

**app.js:**
```javascript
const add = require('./math');
console.log(add(2, 3)); // 5
```

✅ Only one thing is exported, and you can give it any name when importing.

---

## b) Named Exports in CommonJS

**math.js:**
```javascript
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = {
  add,        // named export
  subtract    // named export
};
```

**app.js:**
```javascript
const math = require('./math');
console.log(math.add(5, 2));      // 7
console.log(math.subtract(5, 2)); // 3
```

✅ You import the object and access its named functions.

> **CommonJS Tip:** There’s technically **no "named export"** keyword — you're just exporting an object.

---

# 2. ✨ **ES Modules** (after `"type": "module"` in package.json)

## a) Default Export in ES Modules

**math.js:**
```javascript
export default function add(a, b) {
  return a + b;
}
```

**app.js:**
```javascript
import add from './math.js';
console.log(add(4, 6)); // 10
```

✅ Only one thing is exported.  
✅ No `{}` needed when importing.  
✅ You can rename it while importing.

---

## b) Named Exports in ES Modules

**math.js:**
```javascript
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

**app.js:**
```javascript
import { add, subtract } from './math.js';

console.log(add(10, 5));      // 15
console.log(subtract(10, 5)); // 5
```

✅ Use `{}` when importing.  
✅ Must use the same function names.

---

# 3. 🎯 Using Both (Named + Default) Together in ES Modules

**math.js:**
```javascript
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export default function multiply(a, b) {
  return a * b;
}
```

**app.js:**
```javascript
import multiply, { add, subtract } from './math.js';

console.log(multiply(2, 3));  // 6
console.log(add(2, 3));       // 5
console.log(subtract(5, 3));  // 2
```

✅ One **default export** + many **named exports** — this is very common in real projects!

---

# 🧠 Quick Summary Table:

| Feature                    | CommonJS                         | ES Modules                    |
|-----------------------------|-----------------------------------|-------------------------------|
| Default Export              | `module.exports = item`           | `export default item`          |
| Named Exports               | `module.exports = { item1, item2 }` | `export { item1, item2 }` or `export function item1() {...}` |
| Import Default              | `const item = require('./file')`  | `import item from './file.js'` |
| Import Named                | `const obj = require('./file')` then use `obj.item1` | `import { item1 } from './file.js'` |

---

# 💬 Quick Tip:
- CommonJS → Best for simple Node.js apps.
- ES Modules → Best for modern apps and frontend projects.

---
