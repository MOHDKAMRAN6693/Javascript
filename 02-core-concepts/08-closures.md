# 08 - Closures

## What are Closures?

A closure is a function that has access to variables in its outer (enclosing) scope even after the outer function has returned. Closures are created every time a function is created.

## Understanding Closures

### Basic Closure Example
```javascript
function outerFunction(x) {
    // Outer function's variable
    let outerVariable = x;
    
    // Inner function (closure)
    function innerFunction(y) {
        return outerVariable + y;
    }
    
    return innerFunction;
}

let closure = outerFunction(10);
console.log(closure(5)); // 15
```

### Closure with Multiple Variables
```javascript
function createCounter() {
    let count = 0;
    
    return function() {
        count++;
        return count;
    };
}

let counter1 = createCounter();
let counter2 = createCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2
console.log(counter2()); // 1 (separate closure)
console.log(counter1()); // 3
```

## How Closures Work

### Lexical Scoping
```javascript
function outerFunction() {
    let outerVar = "I'm outer";
    
    function innerFunction() {
        console.log(outerVar); // Can access outer variable
    }
    
    return innerFunction;
}

let myClosure = outerFunction();
myClosure(); // "I'm outer" - still has access to outerVar
```

### Closure with Parameters
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

## Practical Closure Examples

### Example 1: Counter with Reset
```javascript
function createCounter() {
    let count = 0;
    
    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        reset: function() {
            count = 0;
            return count;
        },
        getCount: function() {
            return count;
        }
    };
}

let counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
console.log(counter.reset());     // 0
```

### Example 2: Private Variables
```javascript
function createBankAccount(initialBalance) {
    let balance = initialBalance;
    
    return {
        deposit: function(amount) {
            balance += amount;
            return balance;
        },
        withdraw: function(amount) {
            if (amount <= balance) {
                balance -= amount;
                return balance;
            } else {
                return "Insufficient funds";
            }
        },
        getBalance: function() {
            return balance;
        }
    };
}

let account = createBankAccount(100);
console.log(account.deposit(50));  // 150
console.log(account.withdraw(30)); // 120
console.log(account.getBalance()); // 120
// console.log(account.balance); // undefined - private!
```

### Example 3: Function Factory
```javascript
function createValidator(min, max) {
    return function(value) {
        return value >= min && value <= max;
    };
}

let ageValidator = createValidator(18, 65);
let scoreValidator = createValidator(0, 100);

console.log(ageValidator(25));   // true
console.log(ageValidator(70));   // false
console.log(scoreValidator(85)); // true
console.log(scoreValidator(105)); // false
```

### Example 4: Memoization
```javascript
function createMemoizedFunction(fn) {
    let cache = {};
    
    return function(...args) {
        let key = JSON.stringify(args);
        
        if (cache[key]) {
            console.log("From cache");
            return cache[key];
        }
        
        let result = fn(...args);
        cache[key] = result;
        return result;
    };
}

function expensiveCalculation(n) {
    console.log("Calculating...");
    return n * n;
}

let memoizedCalc = createMemoizedFunction(expensiveCalculation);
console.log(memoizedCalc(5)); // Calculating... 25
console.log(memoizedCalc(5)); // From cache 25
```

## Common Closure Patterns

### Module Pattern
```javascript
let myModule = (function() {
    let privateVariable = "I'm private";
    
    function privateFunction() {
        return "Private function called";
    }
    
    return {
        publicMethod: function() {
            return privateVariable + " - " + privateFunction();
        },
        setPrivate: function(value) {
            privateVariable = value;
        }
    };
})();

console.log(myModule.publicMethod()); // "I'm private - Private function called"
myModule.setPrivate("New value");
console.log(myModule.publicMethod()); // "New value - Private function called"
```

### Callback Functions
```javascript
function processData(data, callback) {
    // Simulate processing
    setTimeout(function() {
        let result = data.map(x => x * 2);
        callback(result);
    }, 1000);
}

processData([1, 2, 3], function(result) {
    console.log("Processed:", result); // [2, 4, 6]
});
```

### Event Handlers
```javascript
function createButtonHandler(buttonName) {
    return function() {
        console.log(buttonName + " clicked!");
    };
}

let button1Handler = createButtonHandler("Button 1");
let button2Handler = createButtonHandler("Button 2");

button1Handler(); // "Button 1 clicked!"
button2Handler(); // "Button 2 clicked!"
```

## Closure Gotchas

### 1. Loop Variable Problem
```javascript
// Problem: All functions reference the same variable
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 3, 3, 3 (all print 3!)
    }, 100);
}

// Solution 1: Use let
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 0, 1, 2 (correct!)
    }, 100);
}

// Solution 2: Use closure
for (var i = 0; i < 3; i++) {
    (function(j) {
        setTimeout(function() {
            console.log(j); // 0, 1, 2 (correct!)
        }, 100);
    })(i);
}
```

### 2. Memory Leaks
```javascript
// Problem: Closure keeps reference to large object
function createClosure() {
    let largeObject = new Array(1000000).fill("data");
    
    return function() {
        return "Hello";
    };
}

let closure = createClosure();
// largeObject is still in memory even though we don't use it!

// Solution: Set to null when done
function createClosure() {
    let largeObject = new Array(1000000).fill("data");
    
    return function() {
        largeObject = null; // Free memory
        return "Hello";
    };
}
```

## Advanced Closure Concepts

### Closure with this
```javascript
let obj = {
    name: "My Object",
    createClosure: function() {
        let self = this; // Capture this
        return function() {
            return self.name;
        };
    }
};

let closure = obj.createClosure();
console.log(closure()); // "My Object"
```

### Closure with Arrow Functions
```javascript
function createClosure() {
    let count = 0;
    
    return () => {
        count++;
        return count;
    };
}

let closure = createClosure();
console.log(closure()); // 1
console.log(closure()); // 2
```

## Practice Exercises

### Exercise 1: Calculator with Memory
Create a calculator that remembers the last result:
```javascript
function createCalculator() {
    let memory = 0;
    
    return {
        add: function(x) { /* implementation */ },
        subtract: function(x) { /* implementation */ },
        multiply: function(x) { /* implementation */ },
        divide: function(x) { /* implementation */ },
        getMemory: function() { /* implementation */ },
        clear: function() { /* implementation */ }
    };
}
```

### Exercise 2: Rate Limiter
Create a function that limits how often it can be called:
```javascript
function createRateLimiter(maxCalls, timeWindow) {
    // Implementation
}

let limiter = createRateLimiter(3, 1000); // 3 calls per second
```

### Exercise 3: Cache with TTL
Create a cache that expires entries after a certain time:
```javascript
function createCache(ttl) {
    // Implementation
}

let cache = createCache(5000); // 5 second TTL
```

### Exercise 4: Function Debouncing
Create a debounced function that only executes after a delay:
```javascript
function createDebounced(fn, delay) {
    // Implementation
}

let debouncedSearch = createDebounced(searchFunction, 300);
```

### Exercise 5: State Manager
Create a simple state manager with closures:
```javascript
function createStateManager(initialState) {
    // Implementation
}

let state = createStateManager({ count: 0, name: "John" });
```

## Common Mistakes

### 1. Not understanding closure scope
```javascript
// Wrong - trying to access closure variable directly
function createCounter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}

let counter = createCounter();
// console.log(counter.count); // undefined - count is not accessible
```

### 2. Creating closures in loops incorrectly
```javascript
// Wrong
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // All print 3
    }, 100);
}

// Correct
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 0, 1, 2
    }, 100);
}
```

### 3. Memory leaks with closures
```javascript
// Wrong - keeping references to large objects
function createClosure() {
    let largeData = new Array(1000000);
    return function() {
        return "Hello";
    };
}

// Better - clean up when done
function createClosure() {
    let largeData = new Array(1000000);
    return function() {
        largeData = null;
        return "Hello";
    };
}
```

## Key Takeaways

- Closures allow functions to access variables from their outer scope
- Closures are created every time a function is created
- Closures are useful for creating private variables and methods
- Closures can cause memory leaks if not handled properly
- Use closures for module patterns, callbacks, and state management
- Be careful with closures in loops - use let instead of var
- Closures are a fundamental concept in JavaScript

## Next Steps

In the next lesson, we'll learn about:
- The `this` keyword and how it works
- Different ways `this` is bound
- Arrow functions and `this`
- Call, apply, and bind methods
- Common `this` pitfalls and solutions

---

**Ready to master the `this` keyword? Let's dive in! 🚀**
