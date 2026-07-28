# JavaScript (ES6+) Quick Lookup Cheat Sheet

## 1. Variables & Scope
```javascript
const name = "Alice"; // Block-scoped, re-assignable: NO, re-declarable: NO
let count = 0;        // Block-scoped, re-assignable: YES, re-declarable: NO
var legacy = true;    // Function-scoped, avoid using in modern JS
```

---

## 2. Data Types & Type Checking
```javascript
// Primitive Types
typeof "Hello"      // "string"
typeof 42           // "number"
typeof 9007199254740991n // "bigint"
typeof true         // "boolean"
typeof Symbol("id") // "symbol"
typeof undefined    // "undefined"
typeof null         // "object" (known bug in JS)

// Objects & Functions
typeof { a: 1 }     // "object"
typeof [1, 2, 3]    // "object" (Use Array.isArray(arr) -> true)
typeof function(){} // "function"
```

---

## 3. Arrow Functions & Rest / Spread
```javascript
// Arrow Functions
const add = (a, b) => a + b;
const square = x => x * x;
const getObj = () => ({ id: 1 }); // Wrap object literal in parentheses

// Spread (...) & Rest (...)
const nums = [1, 2, 3];
const newNums = [...nums, 4, 5]; // Spread into new array

const user = { name: "Alex", age: 30 };
const updatedUser = { ...user, role: "Admin" }; // Spread into new object

const sumAll = (...args) => args.reduce((acc, n) => acc + n, 0); // Rest parameter
```

---

## 4. Destructuring Assignment
```javascript
// Array Destructuring
const [first, second, ...rest] = [10, 20, 30, 40];

// Object Destructuring
const person = { firstName: "Jane", age: 25, city: "Tokyo" };
const { firstName, age, city: location = "Unknown" } = person;
```

---

## 5. Array Methods (Modern & Essential)
```javascript
const numbers = [1, 2, 3, 4, 5];

// Iteration & Transformation
numbers.forEach(num => console.log(num));            // Loop without return
const doubled = numbers.map(num => num * 2);          // [2, 4, 6, 8, 10]
const evens = numbers.filter(num => num % 2 === 0);   // [2, 4]
const sum = numbers.reduce((acc, curr) => acc + curr, 0); // 15

// Searching & Checking
const found = numbers.find(num => num > 3);           // 4 (first match)
const index = numbers.findIndex(num => num === 3);    // 2
const hasEven = numbers.some(num => num % 2 === 0);   // true
const allPositive = numbers.every(num => num > 0);    // true
const includesThree = numbers.includes(3);           // true
```

---

## 6. Template Literals & String Methods
```javascript
const user = "Sam";
const greeting = `Hello, ${user}! 2 + 2 = ${2 + 2}`;

// Useful String Methods
"hello".toUpperCase();             // "HELLO"
"  clean  ".trim();                // "clean"
"JavaScript".slice(0, 4);          // "Java"
"a,b,c".split(",");                // ["a", "b", "c"]
"foobar".includes("bar");          // true
"code".startsWith("co");           // true
```

---

## 7. Promises & Async / Await
```javascript
// Async / Await Syntax
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Fetch failed:", error);
  }
}

// Parallel Execution
const [users, posts] = await Promise.all([
  fetchData("/api/users"),
  fetchData("/api/posts")
]);
```

---

## 8. Optional Chaining & Nullish Coalescing
```javascript
const user = { profile: { name: "Sarah" } };

// Optional Chaining (?.)
const zipCode = user?.address?.zipCode; // undefined (doesn't throw Error)
const firstItem = user?.getItems?.()?.[0];

// Nullish Coalescing (??) - checks ONLY for null or undefined
const input = 0;
const value1 = input || 100; // 100 (because 0 is falsy)
const value2 = input ?? 100; // 0 (0 is not nullish)
```

---

## 9. Classes & Inheritance
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    return `${this.name} makes a noise.`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }
  speak() {
    return `${this.name} barks!`;
  }
}

const dog = new Dog("Rex", "German Shepherd");
```

---

## 10. ES Modules (Import / Export)
```javascript
// Exporting (mathUtils.js)
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;
export default class Calculator { /* ... */ }

// Importing (app.js)
import Calculator, { add, multiply as mult } from './mathUtils.js';
```
