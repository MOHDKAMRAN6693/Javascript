# 16 - Arrow Functions

## What are Arrow Functions?

Arrow functions are a concise way to write functions in JavaScript, introduced in ES6. They provide a shorter syntax and have different behavior with the `this` keyword compared to regular functions.

## Basic Syntax

### Traditional Function vs Arrow Function
```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function
let add = (a, b) => {
    return a + b;
};

// Even shorter - implicit return
let add = (a, b) => a + b;
```

### Different Arrow Function Forms
```javascript
// No parameters
let sayHello = () => "Hello!";

// Single parameter (parentheses optional)
let square = x => x * x;
let square2 = (x) => x * x;

// Multiple parameters
let multiply = (a, b) => a * b;

// Multiple statements
let complexFunction = (x, y) => {
    let sum = x + y;
    let product = x * y;
    return sum + product;
};
```

## Arrow Functions vs Regular Functions

### `this` Binding
```javascript
let obj = {
    name: "John",
    
    // Regular function - has its own this
    regularMethod: function() {
        console.log(this.name); // "John"
        
        setTimeout(function() {
            console.log(this.name); // undefined (this is Window)
        }, 100);
    },
    
    // Arrow function - inherits this from enclosing scope
    arrowMethod: function() {
        console.log(this.name); // "John"
        
        setTimeout(() => {
            console.log(this.name); // "John" (inherits this)
        }, 100);
    }
};

obj.regularMethod();
obj.arrowMethod();
```

### No `arguments` Object
```javascript
// Regular function has arguments object
function regularFunction() {
    console.log(arguments); // Arguments object
}

// Arrow function doesn't have arguments object
let arrowFunction = () => {
    console.log(arguments); // ReferenceError
};

// Use rest parameters instead
let arrowFunctionWithRest = (...args) => {
    console.log(args); // Array of arguments
};
```

### Cannot be Used as Constructors
```javascript
// Regular function can be used as constructor
function Person(name) {
    this.name = name;
}

let person = new Person("John"); // Works

// Arrow function cannot be used as constructor
let PersonArrow = (name) => {
    this.name = name;
};

let personArrow = new PersonArrow("John"); // TypeError
```

## Practical Examples

### Array Methods
```javascript
let numbers = [1, 2, 3, 4, 5];

// Using arrow functions with array methods
let doubled = numbers.map(x => x * 2);
let evens = numbers.filter(x => x % 2 === 0);
let sum = numbers.reduce((acc, curr) => acc + curr, 0);

console.log(doubled); // [2, 4, 6, 8, 10]
console.log(evens); // [2, 4]
console.log(sum); // 15
```

### Event Handlers
```javascript
let button = document.getElementById("myButton");

// Arrow function in event handler
button.addEventListener("click", () => {
    console.log("Button clicked!");
});

// With parameters
let buttons = document.querySelectorAll(".btn");
buttons.forEach((button, index) => {
    button.addEventListener("click", () => {
        console.log(`Button ${index} clicked!`);
    });
});
```

### Object Methods
```javascript
let calculator = {
    result: 0,
    
    add: (x) => {
        this.result += x; // Wrong! this is not calculator
        return this.result;
    },
    
    // Better to use regular function for object methods
    addCorrect: function(x) {
        this.result += x;
        return this.result;
    }
};
```

## When to Use Arrow Functions

### Good Use Cases
```javascript
// 1. Array methods
let numbers = [1, 2, 3, 4, 5];
let squares = numbers.map(x => x * x);

// 2. Event handlers that need to preserve this
class MyClass {
    constructor() {
        this.name = "MyClass";
    }
    
    setupEvent() {
        document.addEventListener("click", () => {
            console.log(this.name); // "MyClass" - this is preserved
        });
    }
}

// 3. Callback functions
setTimeout(() => {
    console.log("Delayed execution");
}, 1000);

// 4. Functional programming
let data = [1, 2, 3, 4, 5];
let processed = data
    .filter(x => x > 2)
    .map(x => x * 2)
    .reduce((acc, curr) => acc + curr, 0);
```

### When NOT to Use Arrow Functions
```javascript
// 1. Object methods
let obj = {
    name: "John",
    // Wrong - this will be Window
    greet: () => {
        console.log(`Hello, ${this.name}`); // undefined
    },
    
    // Correct - use regular function
    greetCorrect: function() {
        console.log(`Hello, ${this.name}`); // "Hello, John"
    }
};

// 2. Constructor functions
function Person(name) {
    this.name = name;
}

let person = new Person("John"); // Works

// Arrow function cannot be constructor
let PersonArrow = (name) => {
    this.name = name;
};

// let personArrow = new PersonArrow("John"); // TypeError

// 3. Methods that need to be bound to different contexts
let obj1 = { name: "Object 1" };
let obj2 = { name: "Object 2" };

function regularMethod() {
    console.log(this.name);
}

// Can be bound to different objects
let bound1 = regularMethod.bind(obj1);
let bound2 = regularMethod.bind(obj2);

// Arrow function cannot be bound
let arrowMethod = () => {
    console.log(this.name);
};
```

## Advanced Arrow Function Concepts

### Returning Objects
```javascript
// Wrong - looks like block statement
let createPerson = (name, age) => {
    name: name,
    age: age
};

// Correct - wrap in parentheses
let createPerson = (name, age) => ({
    name: name,
    age: age
});

// Even better with shorthand
let createPerson = (name, age) => ({
    name,
    age
});
```

### Arrow Functions in Classes
```javascript
class MyClass {
    constructor(name) {
        this.name = name;
    }
    
    // Regular method
    regularMethod() {
        console.log(this.name);
    }
    
    // Arrow function method (bound to instance)
    arrowMethod = () => {
        console.log(this.name);
    }
}

let instance = new MyClass("Test");

// Both work the same way
instance.regularMethod(); // "Test"
instance.arrowMethod(); // "Test"

// But when passed around, arrow method preserves this
let regularRef = instance.regularMethod;
let arrowRef = instance.arrowMethod;

regularRef(); // undefined (this is undefined)
arrowRef(); // "Test" (this is preserved)
```

### Nested Arrow Functions
```javascript
let outerFunction = (x) => {
    return (y) => {
        return (z) => {
            return x + y + z;
        };
    };
};

let result = outerFunction(1)(2)(3);
console.log(result); // 6

// Equivalent to:
let outerFunction = function(x) {
    return function(y) {
        return function(z) {
            return x + y + z;
        };
    };
};
```

## Practice Exercises

### Exercise 1: Array Processing
Use arrow functions to process an array of numbers:
```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Find all even numbers
// Double each number
// Sum all numbers
// Find the maximum number
```

### Exercise 2: Object Processing
Use arrow functions to process an array of objects:
```javascript
let people = [
    { name: "John", age: 25, city: "New York" },
    { name: "Jane", age: 30, city: "Los Angeles" },
    { name: "Bob", age: 35, city: "Chicago" }
];

// Get all names
// Find people over 30
// Calculate average age
// Group by city
```

### Exercise 3: Event Handling
Create event handlers using arrow functions:
```javascript
// Create buttons dynamically
// Add click handlers
// Handle form submission
// Process keyboard events
```

### Exercise 4: Functional Programming
Implement functional programming patterns:
```javascript
// Create a pipe function
// Create a compose function
// Implement map, filter, reduce
// Create utility functions
```

### Exercise 5: Class Methods
Create a class with both regular and arrow methods:
```javascript
class Calculator {
    constructor() {
        this.result = 0;
    }
    
    // Regular method
    add(x) {
        // Implementation
    }
    
    // Arrow method
    subtract = (x) => {
        // Implementation
    }
}
```

## Common Mistakes

### 1. Using Arrow Functions for Object Methods
```javascript
// Wrong
let obj = {
    name: "John",
    greet: () => {
        console.log(this.name); // undefined
    }
};

// Correct
let obj = {
    name: "John",
    greet: function() {
        console.log(this.name); // "John"
    }
};
```

### 2. Confusing Arrow Function Syntax
```javascript
// Wrong - missing parentheses for object return
let createObj = () => { name: "John" };

// Correct - wrap in parentheses
let createObj = () => ({ name: "John" });
```

### 3. Overusing Arrow Functions
```javascript
// Don't use arrow functions everywhere
let obj = {
    // Use regular function for methods
    method: function() {
        // Use arrow function for callbacks
        setTimeout(() => {
            console.log("Callback");
        }, 100);
    }
};
```

## Key Takeaways

- Arrow functions provide a concise syntax for writing functions
- They don't have their own `this` - they inherit from the enclosing scope
- They can't be used as constructors
- They don't have an `arguments` object
- Use them for callbacks, array methods, and when you need to preserve `this`
- Don't use them for object methods or constructors
- Practice with different use cases to understand when to use them

## Next Steps

In the next lesson, we'll learn about:
- Destructuring assignment syntax
- Array destructuring
- Object destructuring
- Function parameter destructuring
- Nested destructuring and rest/spread with destructuring

---

**Ready to unpack data structures? Let's learn about Destructuring! 🚀**
