# 02 - Variables and Data Types

## What are Variables?

Variables are containers that store data values. Think of them as labeled boxes where you can put different types of information.

## Declaring Variables

JavaScript has three ways to declare variables:

### 1. `var` (Legacy - avoid in modern code)
```javascript
var name = "John";
var age = 25;
```

### 2. `let` (Block-scoped, can be reassigned)
```javascript
let name = "John";
let age = 25;
name = "Jane"; // Can be reassigned
```

### 3. `const` (Block-scoped, cannot be reassigned)
```javascript
const name = "John";
const age = 25;
// name = "Jane"; // Error! Cannot reassign
```

## Variable Naming Rules

```javascript
// ✅ Valid names
let userName = "John";
let user_age = 25;
let $price = 100;
let _count = 5;
let user123 = "valid";

// ❌ Invalid names
let 123user = "invalid";     // Cannot start with number
let user-name = "invalid";    // Cannot use hyphens
let let = "invalid";         // Cannot use reserved words
let user name = "invalid";   // Cannot use spaces
```

### Naming Conventions
```javascript
// camelCase (recommended for JavaScript)
let firstName = "John";
let lastName = "Doe";
let isStudent = true;

// Constants often use UPPER_SNAKE_CASE
const MAX_USERS = 100;
const API_URL = "https://api.example.com";
```

## JavaScript Data Types

JavaScript has two categories of data types:

### Primitive Data Types

#### 1. Number
```javascript
let age = 25;
let price = 99.99;
let negative = -10;
let infinity = Infinity;
let notANumber = NaN;

console.log(typeof age);        // "number"
console.log(typeof price);      // "number"
console.log(typeof infinity);   // "number"
console.log(typeof notANumber); // "number"
```

#### 2. String
```javascript
let firstName = "John";
let lastName = 'Doe';
let fullName = `John Doe`;  // Template literal

// String methods
console.log(firstName.length);           // 4
console.log(firstName.toUpperCase());    // "JOHN"
console.log(firstName.toLowerCase());    // "john"
console.log(firstName.charAt(0));        // "J"

// String concatenation
let greeting = "Hello, " + firstName + "!";
let templateGreeting = `Hello, ${firstName}!`;
```

#### 3. Boolean
```javascript
let isStudent = true;
let isWorking = false;

console.log(typeof isStudent); // "boolean"
console.log(!isStudent);       // false (negation)
console.log(isStudent && isWorking); // false (AND)
console.log(isStudent || isWorking);  // true (OR)
```

#### 4. Undefined
```javascript
let x;
console.log(x);        // undefined
console.log(typeof x); // "undefined"

let y = undefined;
console.log(y);        // undefined
```

#### 5. Null
```javascript
let empty = null;
console.log(empty);        // null
console.log(typeof empty); // "object" (this is a JavaScript quirk!)
```

#### 6. Symbol (ES6+)
```javascript
let id = Symbol('id');
let anotherId = Symbol('id');

console.log(id === anotherId); // false (symbols are unique)
console.log(typeof id);        // "symbol"
```

### Reference Data Types

#### 1. Object
```javascript
let person = {
    name: "John",
    age: 25,
    isStudent: true
};

console.log(person.name);        // "John"
console.log(person['age']);      // 25
console.log(typeof person);     // "object"
```

#### 2. Array
```javascript
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = ["hello", 42, true];

console.log(fruits[0]);          // "apple"
console.log(fruits.length);     // 3
console.log(typeof fruits);     // "object"
```

## Type Checking

```javascript
let value = "Hello";

// typeof operator
console.log(typeof value);           // "string"
console.log(typeof 42);              // "number"
console.log(typeof true);            // "boolean"
console.log(typeof undefined);       // "undefined"
console.log(typeof null);            // "object" (quirk!)
console.log(typeof {});              // "object"
console.log(typeof []);              // "object"

// More specific type checking
console.log(Array.isArray([1, 2, 3]));  // true
console.log(Array.isArray("hello"));     // false
```

## Type Conversion

### Implicit Conversion (Automatic)
```javascript
let result = "5" + 3;        // "53" (string concatenation)
let result2 = "5" - 3;       // 2 (number subtraction)
let result3 = "5" * 3;       // 15 (number multiplication)
let result4 = "5" / 3;       // 1.666... (number division)

console.log(Boolean(""));    // false
console.log(Boolean("hello")); // true
console.log(Boolean(0));    // false
console.log(Boolean(1));    // true
```

### Explicit Conversion (Manual)
```javascript
// String to Number
let str = "123";
let num1 = Number(str);      // 123
let num2 = parseInt(str);    // 123
let num3 = parseFloat("123.45"); // 123.45
let num4 = +str;             // 123 (unary plus)

// Number to String
let number = 123;
let str1 = String(number);   // "123"
let str2 = number.toString(); // "123"
let str3 = number + "";      // "123"

// Boolean conversion
let bool1 = Boolean("hello"); // true
let bool2 = Boolean("");      // false
let bool3 = Boolean(0);       // false
let bool4 = Boolean(1);       // true
```

## Variable Scope

### Global Scope
```javascript
let globalVar = "I'm global";

function myFunction() {
    console.log(globalVar); // Can access global variables
}
```

### Local Scope
```javascript
function myFunction() {
    let localVar = "I'm local";
    console.log(localVar); // Works inside function
}

// console.log(localVar); // Error! Cannot access outside function
```

### Block Scope (let and const)
```javascript
if (true) {
    let blockVar = "I'm in a block";
    const blockConst = "I'm also in a block";
}

// console.log(blockVar); // Error! Cannot access outside block
```

## Practice Exercises

### Exercise 1: Variable Declaration
Create variables to store:
- Your name (string)
- Your age (number)
- Whether you're a student (boolean)
- Your favorite colors (array)

### Exercise 2: Type Conversion
```javascript
// Given these values, convert them as requested:
let str = "123";
let num = 456;
let bool = true;

// Convert str to number
// Convert num to string
// Convert bool to string
// Convert str to boolean
```

### Exercise 3: String Manipulation
```javascript
let firstName = "john";
let lastName = "doe";

// Create a full name in proper case
// Find the length of the full name
// Check if the name contains "doe"
```

### Exercise 4: Object Creation
Create an object representing yourself with:
- name
- age
- hobbies (array)
- isStudent (boolean)
- address (object with street, city, country)

## Common Mistakes

### 1. Using `var` instead of `let`/`const`
```javascript
// Avoid
var name = "John";

// Use instead
let name = "John";
const name = "John";
```

### 2. Not understanding type coercion
```javascript
// This might be confusing
console.log("5" + 3);     // "53"
console.log("5" - 3);     // 2
console.log("5" * 3);     // 15
```

### 3. Comparing with `==` instead of `===`
```javascript
// Avoid
if (age == "25") { } // Type coercion happens

// Use instead
if (age === 25) { } // Strict comparison
```

## Key Takeaways

- Use `let` for variables that change, `const` for constants
- JavaScript has dynamic typing (variables can change type)
- Understand the difference between primitive and reference types
- Be careful with type conversion and comparison
- Always use meaningful variable names
- Practice with different data types regularly

## Next Steps

In the next lesson, we'll learn about:
- Arithmetic operators (+, -, *, /, %)
- Comparison operators (==, ===, !=, !==, <, >, <=, >=)
- Logical operators (&&, ||, !)
- Assignment operators (=, +=, -=, *=, /=)

---

**Ready to learn about operators? Let's go! 🚀**
