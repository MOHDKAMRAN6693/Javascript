# 05 - Functions

## What are Functions?

Functions are reusable blocks of code that perform a specific task. They help you organize your code, avoid repetition, and make your programs more modular and maintainable.

## Function Declaration

### Basic Function
```javascript
function greet() {
    console.log("Hello, World!");
}

// Call the function
greet(); // "Hello, World!"
```

### Function with Parameters
```javascript
function greet(name) {
    console.log("Hello, " + name + "!");
}

greet("John");    // "Hello, John!"
greet("Alice");   // "Hello, Alice!"
```

### Function with Multiple Parameters
```javascript
function add(a, b) {
    return a + b;
}

let result = add(5, 3);
console.log(result); // 8
```

### Function with Return Statement
```javascript
function multiply(x, y) {
    return x * y;
}

let product = multiply(4, 6);
console.log(product); // 24
```

## Function Expression

### Anonymous Function Expression
```javascript
let greet = function(name) {
    return "Hello, " + name + "!";
};

console.log(greet("John")); // "Hello, John!"
```

### Named Function Expression
```javascript
let greet = function sayHello(name) {
    return "Hello, " + name + "!";
};

console.log(greet("John")); // "Hello, John!"
```

## Arrow Functions (ES6+)

### Basic Arrow Function
```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function
let add = (a, b) => {
    return a + b;
};

// Shorter arrow function (single expression)
let add = (a, b) => a + b;
```

### Arrow Function with One Parameter
```javascript
// Traditional
function square(x) {
    return x * x;
}

// Arrow function
let square = x => x * x;
```

### Arrow Function with No Parameters
```javascript
// Traditional
function sayHello() {
    return "Hello!";
}

// Arrow function
let sayHello = () => "Hello!";
```

## Function Parameters

### Default Parameters
```javascript
function greet(name = "Guest") {
    return "Hello, " + name + "!";
}

console.log(greet());        // "Hello, Guest!"
console.log(greet("John")); // "Hello, John!"
```

### Rest Parameters (...)
```javascript
function sum(...numbers) {
    let total = 0;
    for (let number of numbers) {
        total += number;
    }
    return total;
}

console.log(sum(1, 2, 3));           // 6
console.log(sum(1, 2, 3, 4, 5));    // 15
```

### Destructuring Parameters
```javascript
function displayUser({name, age, city}) {
    console.log(`Name: ${name}, Age: ${age}, City: ${city}`);
}

let user = {
    name: "John",
    age: 25,
    city: "New York"
};

displayUser(user);
```

## Function Scope

### Local Scope
```javascript
function myFunction() {
    let localVar = "I'm local";
    console.log(localVar); // Works inside function
}

// console.log(localVar); // Error! Cannot access outside function
```

### Global Scope
```javascript
let globalVar = "I'm global";

function myFunction() {
    console.log(globalVar); // Can access global variables
    globalVar = "Modified"; // Can modify global variables
}
```

### Block Scope
```javascript
function myFunction() {
    if (true) {
        let blockVar = "I'm in a block";
        const blockConst = "I'm also in a block";
    }
    // console.log(blockVar); // Error! Cannot access outside block
}
```

## Higher-Order Functions

### Functions as Parameters
```javascript
function greet(name, greetingFunction) {
    return greetingFunction(name);
}

function sayHello(name) {
    return "Hello, " + name + "!";
}

function sayGoodbye(name) {
    return "Goodbye, " + name + "!";
}

console.log(greet("John", sayHello));   // "Hello, John!"
console.log(greet("John", sayGoodbye)); // "Goodbye, John!"
```

### Functions as Return Values
```javascript
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

let double = createMultiplier(2);
let triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

## Array Methods with Functions

### forEach
```javascript
let fruits = ["apple", "banana", "orange"];

fruits.forEach(function(fruit) {
    console.log(fruit);
});

// With arrow function
fruits.forEach(fruit => console.log(fruit));
```

### map
```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(function(number) {
    return number * 2;
});

// With arrow function
let doubled = numbers.map(number => number * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

### filter
```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let evenNumbers = numbers.filter(function(number) {
    return number % 2 === 0;
});

// With arrow function
let evenNumbers = numbers.filter(number => number % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]
```

### reduce
```javascript
let numbers = [1, 2, 3, 4, 5];

let sum = numbers.reduce(function(accumulator, current) {
    return accumulator + current;
}, 0);

// With arrow function
let sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15
```

## Practical Examples

### Example 1: Calculator Functions
```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

function multiply(a, b) {
    return a * b;
}

function divide(a, b) {
    if (b === 0) {
        return "Cannot divide by zero";
    }
    return a / b;
}

console.log(add(5, 3));      // 8
console.log(subtract(5, 3)); // 2
console.log(multiply(5, 3)); // 15
console.log(divide(5, 3));   // 1.666...
console.log(divide(5, 0));   // "Cannot divide by zero"
```

### Example 2: String Manipulation
```javascript
function capitalizeFirst(str) {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

function reverseString(str) {
    return str.split('').reverse().join('');
}

function isPalindrome(str) {
    let cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
    return cleaned === cleaned.split('').reverse().join('');
}

console.log(capitalizeFirst("hello"));     // "Hello"
console.log(reverseString("hello"));       // "olleh"
console.log(isPalindrome("racecar"));     // true
console.log(isPalindrome("hello"));       // false
```

### Example 3: Array Processing
```javascript
function findMax(numbers) {
    let max = numbers[0];
    for (let number of numbers) {
        if (number > max) {
            max = number;
        }
    }
    return max;
}

function findMin(numbers) {
    return Math.min(...numbers);
}

function calculateAverage(numbers) {
    let sum = numbers.reduce((acc, curr) => acc + curr, 0);
    return sum / numbers.length;
}

let numbers = [1, 5, 3, 9, 2];
console.log(findMax(numbers));        // 9
console.log(findMin(numbers));       // 1
console.log(calculateAverage(numbers)); // 4
```

## Practice Exercises

### Exercise 1: Temperature Converter
Create functions to convert between Celsius and Fahrenheit:
```javascript
// Celsius to Fahrenheit: F = C * 9/5 + 32
// Fahrenheit to Celsius: C = (F - 32) * 5/9
```

### Exercise 2: Factorial Calculator
Create a function that calculates the factorial of a number:
```javascript
// factorial(5) = 5 * 4 * 3 * 2 * 1 = 120
```

### Exercise 3: Prime Number Checker
Create a function that checks if a number is prime:
```javascript
// isPrime(17) should return true
// isPrime(15) should return false
```

### Exercise 4: Array Statistics
Create functions to calculate:
- Sum of array
- Average of array
- Count of even numbers
- Find all prime numbers in array

### Exercise 5: String Functions
Create functions for:
- Count vowels in a string
- Remove spaces from a string
- Check if string is a valid email
- Convert string to title case

## Common Mistakes

### 1. Missing return statement
```javascript
// Wrong
function add(a, b) {
    a + b; // Missing return
}

// Correct
function add(a, b) {
    return a + b;
}
```

### 2. Not calling the function
```javascript
// Wrong
function greet() {
    console.log("Hello!");
}
greet; // Missing parentheses

// Correct
function greet() {
    console.log("Hello!");
}
greet(); // Call the function
```

### 3. Parameter vs Argument confusion
```javascript
// Parameters are in the function definition
function greet(name) { // 'name' is a parameter
    console.log("Hello, " + name);
}

// Arguments are what you pass when calling
greet("John"); // "John" is an argument
```

### 4. Hoisting issues with function expressions
```javascript
// This works (function declaration is hoisted)
console.log(greet("John")); // "Hello, John!"

function greet(name) {
    return "Hello, " + name;
}

// This doesn't work (function expression is not hoisted)
console.log(sayHello("John")); // Error!

let sayHello = function(name) {
    return "Hello, " + name;
};
```

## Key Takeaways

- Functions help organize and reuse code
- Use meaningful function and parameter names
- Always return a value when needed
- Understand the difference between parameters and arguments
- Practice with different function types and patterns
- Use arrow functions for shorter, cleaner code
- Functions are first-class citizens in JavaScript

## Next Steps

In the next lesson, we'll learn about:
- Objects and their properties
- Arrays and array methods
- Object methods and constructors
- JSON data format
- Working with complex data structures

---

**Ready to work with data structures? Let's explore Objects and Arrays! 🚀**
