# 09 - This Keyword

## What is `this`?

The `this` keyword in JavaScript refers to the object that is currently executing the function. The value of `this` depends on how a function is called, not where it is defined.

## Understanding `this` Context

### Global Context
```javascript
console.log(this); // In browser: Window object, In Node.js: global object

function globalFunction() {
    console.log(this); // In browser: Window object, In Node.js: global object
}

globalFunction();
```

### Object Method Context
```javascript
let person = {
    name: "John",
    age: 25,
    greet: function() {
        console.log(this); // Refers to the person object
        console.log(`Hello, I'm ${this.name}`);
    }
};

person.greet(); // "Hello, I'm John"
```

## Different Ways `this` is Bound

### 1. Default Binding
When a function is called in the global scope, `this` refers to the global object.

```javascript
function sayHello() {
    console.log(this); // Window object (in browser)
}

sayHello(); // Default binding
```

### 2. Implicit Binding
When a function is called as a method of an object, `this` refers to that object.

```javascript
let car = {
    brand: "Toyota",
    model: "Camry",
    start: function() {
        console.log(`Starting ${this.brand} ${this.model}`);
    }
};

car.start(); // "Starting Toyota Camry"
```

### 3. Explicit Binding
Using `call()`, `apply()`, or `bind()` to explicitly set the value of `this`.

```javascript
function greet() {
    console.log(`Hello, ${this.name}`);
}

let person1 = { name: "John" };
let person2 = { name: "Jane" };

// Using call()
greet.call(person1); // "Hello, John"
greet.call(person2); // "Hello, Jane"

// Using apply()
greet.apply(person1); // "Hello, John"

// Using bind()
let boundGreet = greet.bind(person1);
boundGreet(); // "Hello, John"
```

### 4. New Binding
When a function is called with the `new` keyword, `this` refers to the newly created object.

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        console.log(`Hello, I'm ${this.name}`);
    };
}

let john = new Person("John", 25);
john.greet(); // "Hello, I'm John"
```

## Arrow Functions and `this`

Arrow functions don't have their own `this`. They inherit `this` from the enclosing scope.

```javascript
let person = {
    name: "John",
    regularFunction: function() {
        console.log(this); // person object
        
        // Arrow function inherits this from regularFunction
        let arrowFunction = () => {
            console.log(this); // person object
            console.log(`Hello from arrow: ${this.name}`);
        };
        
        arrowFunction();
    }
};

person.regularFunction();
```

### Arrow Function vs Regular Function
```javascript
let obj = {
    name: "Test",
    
    regularMethod: function() {
        console.log("Regular function this:", this); // obj
        
        setTimeout(function() {
            console.log("Inside setTimeout:", this); // Window object
        }, 100);
    },
    
    arrowMethod: function() {
        console.log("Arrow method this:", this); // obj
        
        setTimeout(() => {
            console.log("Inside arrow setTimeout:", this); // obj
        }, 100);
    }
};

obj.regularMethod();
obj.arrowMethod();
```

## Common `this` Pitfalls

### 1. Method Losing Context
```javascript
let person = {
    name: "John",
    greet: function() {
        console.log(`Hello, ${this.name}`);
    }
};

let greetFunction = person.greet;
greetFunction(); // "Hello, undefined" - this is now Window object
```

### 2. Callback Functions
```javascript
let button = document.getElementById("myButton");

let obj = {
    name: "John",
    handleClick: function() {
        console.log(this); // button element, not obj
        console.log(this.name); // undefined
    }
};

button.addEventListener("click", obj.handleClick);
```

### 3. Solution with `bind()`
```javascript
let button = document.getElementById("myButton");

let obj = {
    name: "John",
    handleClick: function() {
        console.log(this); // obj
        console.log(this.name); // "John"
    }
};

button.addEventListener("click", obj.handleClick.bind(obj));
```

## Practical Examples

### Example 1: Calculator Object
```javascript
let calculator = {
    result: 0,
    
    add: function(num) {
        this.result += num;
        return this;
    },
    
    subtract: function(num) {
        this.result -= num;
        return this;
    },
    
    multiply: function(num) {
        this.result *= num;
        return this;
    },
    
    getResult: function() {
        return this.result;
    }
};

// Method chaining
let result = calculator.add(5).multiply(3).subtract(2).getResult();
console.log(result); // 13
```

### Example 2: Event Handler with Context
```javascript
class Counter {
    constructor() {
        this.count = 0;
        this.button = document.getElementById("counterBtn");
        this.display = document.getElementById("countDisplay");
        
        // Bind the method to preserve this context
        this.button.addEventListener("click", this.increment.bind(this));
    }
    
    increment() {
        this.count++;
        this.display.textContent = this.count;
    }
}

new Counter();
```

### Example 3: Object with Methods
```javascript
let bankAccount = {
    balance: 1000,
    owner: "John Doe",
    
    deposit: function(amount) {
        this.balance += amount;
        console.log(`Deposited ${amount}. New balance: ${this.balance}`);
        return this;
    },
    
    withdraw: function(amount) {
        if (amount <= this.balance) {
            this.balance -= amount;
            console.log(`Withdrew ${amount}. New balance: ${this.balance}`);
        } else {
            console.log("Insufficient funds");
        }
        return this;
    },
    
    getBalance: function() {
        console.log(`Current balance: ${this.balance}`);
        return this.balance;
    }
};

bankAccount.deposit(500).withdraw(200).getBalance();
```

## Advanced `this` Concepts

### Function Borrowing
```javascript
let person1 = {
    name: "John",
    age: 25
};

let person2 = {
    name: "Jane",
    age: 30
};

function introduce() {
    console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old`);
}

// Borrow the function
introduce.call(person1); // "Hi, I'm John and I'm 25 years old"
introduce.call(person2); // "Hi, I'm Jane and I'm 30 years old"
```

### Partial Application with `bind()`
```javascript
function multiply(a, b) {
    return a * b;
}

// Create a function that always multiplies by 2
let double = multiply.bind(null, 2);
console.log(double(5)); // 10

// Create a function that always multiplies by 3
let triple = multiply.bind(null, 3);
console.log(triple(5)); // 15
```

### `this` in Classes
```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
    
    // Arrow function method (this is bound to instance)
    greetArrow = () => {
        console.log(`Hello from arrow, I'm ${this.name}`);
    }
}

let person = new Person("John", 25);
person.greet(); // "Hello, I'm John"
person.greetArrow(); // "Hello from arrow, I'm John"

// this context is preserved even when method is passed around
let greetMethod = person.greet;
greetMethod(); // "Hello, I'm John"
```

## Practice Exercises

### Exercise 1: Object Methods
Create an object with methods that use `this`:
```javascript
let car = {
    brand: "Toyota",
    model: "Camry",
    year: 2020,
    speed: 0,
    
    start: function() {
        // Implementation
    },
    
    accelerate: function(speed) {
        // Implementation
    },
    
    brake: function(speed) {
        // Implementation
    },
    
    getInfo: function() {
        // Implementation
    }
};
```

### Exercise 2: Event Handler Context
Create a counter that increments when a button is clicked:
```javascript
class Counter {
    constructor() {
        this.count = 0;
        // Add event listener that preserves this context
    }
    
    increment() {
        // Implementation
    }
}
```

### Exercise 3: Method Chaining
Create an object that supports method chaining:
```javascript
let calculator = {
    result: 0,
    
    add: function(num) {
        // Implementation
    },
    
    subtract: function(num) {
        // Implementation
    },
    
    multiply: function(num) {
        // Implementation
    },
    
    getResult: function() {
        // Implementation
    }
};
```

### Exercise 4: Function Borrowing
Create two objects and borrow methods between them:
```javascript
let obj1 = {
    name: "Object 1",
    method: function() {
        // Implementation
    }
};

let obj2 = {
    name: "Object 2"
};

// Borrow method from obj1 to obj2
```

### Exercise 5: Arrow Functions vs Regular Functions
Demonstrate the difference between arrow functions and regular functions with `this`:
```javascript
let obj = {
    name: "Test",
    
    regularMethod: function() {
        // Implementation
    },
    
    arrowMethod: function() {
        // Implementation
    }
};
```

## Common Mistakes

### 1. Losing `this` in Callbacks
```javascript
// Wrong
let obj = {
    name: "John",
    method: function() {
        setTimeout(function() {
            console.log(this.name); // undefined
        }, 100);
    }
};

// Correct
let obj = {
    name: "John",
    method: function() {
        let self = this;
        setTimeout(function() {
            console.log(self.name); // "John"
        }, 100);
    }
};

// Or use arrow function
let obj = {
    name: "John",
    method: function() {
        setTimeout(() => {
            console.log(this.name); // "John"
        }, 100);
    }
};
```

### 2. Not Understanding Arrow Function `this`
```javascript
// Wrong - arrow function doesn't have its own this
let obj = {
    name: "John",
    method: () => {
        console.log(this.name); // undefined (this is Window)
    }
};

// Correct - use regular function
let obj = {
    name: "John",
    method: function() {
        console.log(this.name); // "John"
    }
};
```

### 3. Forgetting to Bind Methods
```javascript
// Wrong
let obj = {
    name: "John",
    method: function() {
        console.log(this.name);
    }
};

let method = obj.method;
method(); // undefined

// Correct
let method = obj.method.bind(obj);
method(); // "John"
```

## Key Takeaways

- `this` refers to the object that is currently executing the function
- The value of `this` depends on how a function is called, not where it's defined
- Arrow functions don't have their own `this` - they inherit from the enclosing scope
- Use `call()`, `apply()`, and `bind()` to explicitly control `this`
- Be careful with `this` in callbacks and event handlers
- Method chaining is possible by returning `this`
- Practice with different `this` contexts to build understanding

## Next Steps

In the next lesson, we'll learn about:
- Prototypes and how they work
- Prototype chain and inheritance
- Creating objects with prototypes
- ES6 classes and inheritance
- Mixins and composition

---

**Ready to understand JavaScript's prototype system? Let's explore Prototypes and Inheritance! 🚀**
