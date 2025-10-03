# 07 - Scope and Hoisting

## What is Scope?

Scope determines the accessibility of variables, functions, and objects in your code. It defines where you can access these elements and where you cannot.

## Types of Scope

### Global Scope
Variables declared outside any function or block are in global scope.

```javascript
// Global scope
let globalVar = "I'm global";
var globalVar2 = "I'm also global";

function myFunction() {
    console.log(globalVar);  // Can access global variables
    console.log(globalVar2); // Can access global variables
}

myFunction();
console.log(globalVar);  // Can access from anywhere
```

### Function Scope
Variables declared inside a function are only accessible within that function.

```javascript
function myFunction() {
    let localVar = "I'm local";
    var localVar2 = "I'm also local";
    
    console.log(localVar);  // Works inside function
    console.log(localVar2); // Works inside function
}

// console.log(localVar);  // Error! Cannot access outside function
// console.log(localVar2); // Error! Cannot access outside function
```

### Block Scope (ES6+)
Variables declared with `let` and `const` are block-scoped.

```javascript
if (true) {
    let blockVar = "I'm in a block";
    const blockConst = "I'm also in a block";
    var functionVar = "I'm function scoped";
    
    console.log(blockVar);   // Works inside block
    console.log(blockConst); // Works inside block
    console.log(functionVar); // Works inside block
}

// console.log(blockVar);   // Error! Cannot access outside block
// console.log(blockConst); // Error! Cannot access outside block
console.log(functionVar); // Works! var is function scoped
```

## Scope Chain

JavaScript uses lexical scoping - inner functions have access to outer function variables.

```javascript
let globalVar = "Global";

function outerFunction() {
    let outerVar = "Outer";
    
    function innerFunction() {
        let innerVar = "Inner";
        
        console.log(globalVar); // "Global" - from global scope
        console.log(outerVar);  // "Outer" - from outer function
        console.log(innerVar); // "Inner" - from inner function
    }
    
    innerFunction();
}

outerFunction();
```

## What is Hoisting?

Hoisting is JavaScript's behavior of moving declarations to the top of their scope before code execution.

### Variable Hoisting

#### var Hoisting
```javascript
console.log(x); // undefined (not error!)
var x = 5;
console.log(x); // 5

// This is equivalent to:
var x; // Declaration is hoisted
console.log(x); // undefined
x = 5; // Assignment happens here
console.log(x); // 5
```

#### let and const Hoisting
```javascript
// console.log(y); // Error! Cannot access before initialization
let y = 10;

// console.log(z); // Error! Cannot access before initialization
const z = 15;
```

### Function Hoisting

#### Function Declarations
```javascript
console.log(sayHello()); // "Hello!" - works!

function sayHello() {
    return "Hello!";
}

// This is equivalent to:
function sayHello() { // Function declaration is hoisted
    return "Hello!";
}
console.log(sayHello()); // "Hello!"
```

#### Function Expressions
```javascript
// console.log(sayGoodbye()); // Error! Cannot access before initialization
let sayGoodbye = function() {
    return "Goodbye!";
};
```

## Temporal Dead Zone (TDZ)

The temporal dead zone is the time between entering a scope and a variable being declared.

```javascript
// Temporal Dead Zone starts here
console.log(typeof x); // ReferenceError for let/const
console.log(typeof y); // "undefined" for var

let x = 5;
var y = 10;
```

## Practical Examples

### Example 1: Scope Chain
```javascript
let globalVar = "Global";

function outerFunction() {
    let outerVar = "Outer";
    
    function innerFunction() {
        let innerVar = "Inner";
        
        // All variables are accessible
        console.log(globalVar); // "Global"
        console.log(outerVar);  // "Outer"
        console.log(innerVar); // "Inner"
    }
    
    innerFunction();
    
    // Only global and outer variables accessible
    console.log(globalVar); // "Global"
    console.log(outerVar);  // "Outer"
    // console.log(innerVar); // Error!
}

outerFunction();
```

### Example 2: Hoisting with var
```javascript
console.log(a); // undefined
var a = 5;
console.log(a); // 5

function test() {
    console.log(b); // undefined
    var b = 10;
    console.log(b); // 10
}

test();
```

### Example 3: Hoisting with let/const
```javascript
// This will cause ReferenceError
// console.log(c); // ReferenceError: Cannot access 'c' before initialization
let c = 5;

// This will cause ReferenceError
// console.log(d); // ReferenceError: Cannot access 'd' before initialization
const d = 10;
```

### Example 4: Function Hoisting
```javascript
// This works - function declarations are hoisted
console.log(hoistedFunction()); // "I'm hoisted!"

function hoistedFunction() {
    return "I'm hoisted!";
}

// This doesn't work - function expressions are not hoisted
// console.log(notHoisted()); // ReferenceError
let notHoisted = function() {
    return "I'm not hoisted!";
};
```

## Common Pitfalls

### 1. var vs let/const in loops
```javascript
// Problem with var
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 3, 3, 3 (all print 3!)
    }, 100);
}

// Solution with let
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 0, 1, 2 (correct!)
    }, 100);
}
```

### 2. Function declarations vs expressions
```javascript
// This works
console.log(declaredFunction()); // "Hello!"

function declaredFunction() {
    return "Hello!";
}

// This doesn't work
// console.log(expressionFunction()); // ReferenceError
let expressionFunction = function() {
    return "Hello!";
};
```

### 3. Block scope confusion
```javascript
if (true) {
    var functionScoped = "I'm function scoped";
    let blockScoped = "I'm block scoped";
}

console.log(functionScoped); // "I'm function scoped" - works!
// console.log(blockScoped); // ReferenceError - doesn't work!
```

## Best Practices

### 1. Use const by default
```javascript
const name = "John"; // Use const for values that don't change
let age = 25;        // Use let for values that change
// Avoid var in modern JavaScript
```

### 2. Declare variables at the top of their scope
```javascript
function goodPractice() {
    let name = "John";
    let age = 25;
    let isStudent = true;
    
    // Use variables here
    console.log(name, age, isStudent);
}
```

### 3. Use meaningful variable names
```javascript
// Good
let userFirstName = "John";
let userAge = 25;

// Bad
let a = "John";
let b = 25;
```

## Practice Exercises

### Exercise 1: Scope Chain
Create nested functions and demonstrate scope chain:
```javascript
let globalVar = "Global";

function outerFunction() {
    let outerVar = "Outer";
    
    function innerFunction() {
        let innerVar = "Inner";
        // Log all accessible variables
    }
    
    innerFunction();
}

outerFunction();
```

### Exercise 2: Hoisting Demonstration
Create examples showing:
- var hoisting behavior
- let/const hoisting behavior
- Function declaration hoisting
- Function expression hoisting

### Exercise 3: Block Scope
Demonstrate the difference between var and let in blocks:
```javascript
if (true) {
    var functionScoped = "Function scoped";
    let blockScoped = "Block scoped";
}

// Try to access both variables outside the block
```

### Exercise 4: Temporal Dead Zone
Create examples showing the temporal dead zone:
```javascript
// Show TDZ with let
// Show TDZ with const
// Compare with var behavior
```

### Exercise 5: Loop Scope
Create a loop that demonstrates the difference between var and let:
```javascript
// Create a loop that shows the var problem
// Fix it using let
// Explain why let works better
```

## Key Takeaways

- Scope determines where variables can be accessed
- Global scope is accessible everywhere
- Function scope is accessible within the function
- Block scope (let/const) is accessible within the block
- Hoisting moves declarations to the top of their scope
- var is hoisted and initialized with undefined
- let/const are hoisted but not initialized (TDZ)
- Function declarations are fully hoisted
- Function expressions are not hoisted
- Use let/const instead of var in modern JavaScript

## Next Steps

In the next lesson, we'll learn about:
- Closures and how they work
- Lexical scoping in detail
- Practical uses of closures
- Common closure patterns
- Memory management with closures

---

**Ready to master closures? Let's explore this powerful concept! 🚀**
