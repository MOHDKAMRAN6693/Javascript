# 06 - Objects and Arrays

## What are Objects?

Objects are collections of key-value pairs that represent real-world entities. They allow you to group related data and functionality together.

## Creating Objects

### Object Literal Syntax
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York",
    isStudent: true
};

console.log(person); // { name: "John", age: 25, city: "New York", isStudent: true }
```

### Object Constructor
```javascript
let person = new Object();
person.name = "John";
person.age = 25;
person.city = "New York";
```

### Object.create()
```javascript
let person = Object.create(null);
person.name = "John";
person.age = 25;
```

## Accessing Object Properties

### Dot Notation
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

console.log(person.name);  // "John"
console.log(person.age);   // 25
console.log(person.city);  // "New York"
```

### Bracket Notation
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

console.log(person["name"]); // "John"
console.log(person["age"]);  // 25

// Useful for dynamic property access
let property = "name";
console.log(person[property]); // "John"
```

## Modifying Objects

### Adding Properties
```javascript
let person = {
    name: "John",
    age: 25
};

person.city = "New York";
person["isStudent"] = true;

console.log(person); // { name: "John", age: 25, city: "New York", isStudent: true }
```

### Updating Properties
```javascript
let person = {
    name: "John",
    age: 25
};

person.age = 26;
person["name"] = "Jane";

console.log(person); // { name: "Jane", age: 26 }
```

### Deleting Properties
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

delete person.city;
console.log(person); // { name: "John", age: 25 }
```

## Object Methods

### Adding Methods to Objects
```javascript
let person = {
    name: "John",
    age: 25,
    greet: function() {
        return "Hello, I'm " + this.name;
    },
    celebrateBirthday: function() {
        this.age++;
        return "Happy birthday! I'm now " + this.age;
    }
};

console.log(person.greet());           // "Hello, I'm John"
console.log(person.celebrateBirthday()); // "Happy birthday! I'm now 26"
```

### Method Shorthand (ES6+)
```javascript
let person = {
    name: "John",
    age: 25,
    greet() {
        return "Hello, I'm " + this.name;
    },
    celebrateBirthday() {
        this.age++;
        return "Happy birthday! I'm now " + this.age;
    }
};
```

## What are Arrays?

Arrays are ordered collections of values that can be of any type. They are indexed starting from 0.

## Creating Arrays

### Array Literal Syntax
```javascript
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = ["hello", 42, true, null];
```

### Array Constructor
```javascript
let fruits = new Array("apple", "banana", "orange");
let empty = new Array(5); // Creates array with 5 empty slots
```

## Accessing Array Elements

```javascript
let fruits = ["apple", "banana", "orange"];

console.log(fruits[0]);  // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits[2]); // "orange"
console.log(fruits.length); // 3
```

## Array Methods

### Adding Elements
```javascript
let fruits = ["apple", "banana"];

// Add to end
fruits.push("orange");
console.log(fruits); // ["apple", "banana", "orange"]

// Add to beginning
fruits.unshift("grape");
console.log(fruits); // ["grape", "apple", "banana", "orange"]
```

### Removing Elements
```javascript
let fruits = ["apple", "banana", "orange"];

// Remove from end
let lastFruit = fruits.pop();
console.log(lastFruit); // "orange"
console.log(fruits);    // ["apple", "banana"]

// Remove from beginning
let firstFruit = fruits.shift();
console.log(firstFruit); // "apple"
console.log(fruits);     // ["banana"]
```

### Finding Elements
```javascript
let fruits = ["apple", "banana", "orange"];

// indexOf
console.log(fruits.indexOf("banana")); // 1
console.log(fruits.indexOf("grape"));  // -1 (not found)

// includes
console.log(fruits.includes("apple")); // true
console.log(fruits.includes("grape")); // false

// find
let numbers = [1, 5, 3, 9, 2];
let found = numbers.find(num => num > 3);
console.log(found); // 5
```

### Array Iteration Methods

#### forEach
```javascript
let fruits = ["apple", "banana", "orange"];

fruits.forEach(function(fruit) {
    console.log(fruit);
});

// With arrow function
fruits.forEach(fruit => console.log(fruit));
```

#### map
```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(function(number) {
    return number * 2;
});

// With arrow function
let doubled = numbers.map(number => number * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

#### filter
```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let evenNumbers = numbers.filter(function(number) {
    return number % 2 === 0;
});

// With arrow function
let evenNumbers = numbers.filter(number => number % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]
```

#### reduce
```javascript
let numbers = [1, 2, 3, 4, 5];

let sum = numbers.reduce(function(accumulator, current) {
    return accumulator + current;
}, 0);

// With arrow function
let sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15
```

## Multidimensional Arrays

```javascript
let matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

console.log(matrix[0][0]); // 1
console.log(matrix[1][2]); // 6

// Iterating through 2D array
for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        console.log(matrix[i][j]);
    }
}
```

## Object and Array Destructuring

### Object Destructuring
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

// Extract specific properties
let { name, age } = person;
console.log(name); // "John"
console.log(age);  // 25

// With default values
let { name, age, country = "USA" } = person;
console.log(country); // "USA"
```

### Array Destructuring
```javascript
let fruits = ["apple", "banana", "orange"];

// Extract elements
let [first, second, third] = fruits;
console.log(first);  // "apple"
console.log(second); // "banana"
console.log(third);  // "orange"

// Skip elements
let [first, , third] = fruits;
console.log(first); // "apple"
console.log(third); // "orange"
```

## JSON (JavaScript Object Notation)

### Converting to JSON
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

let jsonString = JSON.stringify(person);
console.log(jsonString); // '{"name":"John","age":25,"city":"New York"}'
```

### Converting from JSON
```javascript
let jsonString = '{"name":"John","age":25,"city":"New York"}';
let person = JSON.parse(jsonString);
console.log(person.name); // "John"
```

## Practical Examples

### Example 1: Student Management System
```javascript
let students = [
    { name: "John", age: 20, grade: "A" },
    { name: "Jane", age: 19, grade: "B" },
    { name: "Bob", age: 21, grade: "A" }
];

// Find all A students
let aStudents = students.filter(student => student.grade === "A");
console.log(aStudents);

// Calculate average age
let totalAge = students.reduce((sum, student) => sum + student.age, 0);
let averageAge = totalAge / students.length;
console.log(averageAge);
```

### Example 2: Shopping Cart
```javascript
let cart = {
    items: [],
    addItem: function(product, price, quantity = 1) {
        this.items.push({ product, price, quantity });
    },
    removeItem: function(product) {
        this.items = this.items.filter(item => item.product !== product);
    },
    getTotal: function() {
        return this.items.reduce((total, item) => total + (item.price * item.quantity), 0);
    },
    getItemCount: function() {
        return this.items.reduce((count, item) => count + item.quantity, 0);
    }
};

cart.addItem("Laptop", 999, 1);
cart.addItem("Mouse", 25, 2);
console.log(cart.getTotal());    // 1049
console.log(cart.getItemCount()); // 3
```

### Example 3: Data Processing
```javascript
let sales = [
    { product: "Laptop", amount: 1200, date: "2023-01-15" },
    { product: "Mouse", amount: 25, date: "2023-01-16" },
    { product: "Keyboard", amount: 75, date: "2023-01-17" },
    { product: "Laptop", amount: 1200, date: "2023-01-18" }
];

// Total sales
let totalSales = sales.reduce((sum, sale) => sum + sale.amount, 0);
console.log(totalSales); // 2500

// Sales by product
let salesByProduct = sales.reduce((acc, sale) => {
    if (acc[sale.product]) {
        acc[sale.product] += sale.amount;
    } else {
        acc[sale.product] = sale.amount;
    }
    return acc;
}, {});
console.log(salesByProduct); // { Laptop: 2400, Mouse: 25, Keyboard: 75 }
```

## Practice Exercises

### Exercise 1: Library System
Create a library system with:
- Books array with title, author, year, isAvailable
- Functions to add, remove, and find books
- Function to check out a book (mark as unavailable)
- Function to return a book (mark as available)

### Exercise 2: Grade Calculator
Create a student grade system:
- Student object with name and grades array
- Function to add grades
- Function to calculate average grade
- Function to get letter grade (A, B, C, D, F)

### Exercise 3: Todo List
Create a todo list with:
- Tasks array with id, text, completed, priority
- Functions to add, remove, and toggle tasks
- Function to filter tasks by completion status
- Function to sort tasks by priority

### Exercise 4: Weather Data
Process weather data:
- Array of daily temperatures
- Find highest and lowest temperatures
- Calculate average temperature
- Count days above/below certain temperature

### Exercise 5: Employee Management
Create an employee management system:
- Employee objects with name, department, salary
- Functions to add, remove, and find employees
- Calculate total payroll
- Group employees by department

## Common Mistakes

### 1. Confusing arrays and objects
```javascript
// Wrong - treating array like object
let arr = [1, 2, 3];
console.log(arr.name); // undefined

// Correct - use array methods
console.log(arr[0]); // 1
```

### 2. Mutating arrays incorrectly
```javascript
// Wrong - this doesn't work as expected
let arr = [1, 2, 3];
arr[3] = 4; // This works, but...
arr[10] = 11; // Creates sparse array

// Better - use array methods
arr.push(4);
```

### 3. Not understanding reference vs value
```javascript
// Objects and arrays are passed by reference
let arr1 = [1, 2, 3];
let arr2 = arr1; // arr2 points to same array
arr2.push(4);
console.log(arr1); // [1, 2, 3, 4] - arr1 is also modified!

// To copy array
let arr3 = [...arr1]; // or arr1.slice()
```

### 4. Forgetting to return in array methods
```javascript
// Wrong
let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map(function(num) {
    num * 2; // Missing return
});

// Correct
let doubled = numbers.map(function(num) {
    return num * 2;
});
```

## Key Takeaways

- Objects group related data and functionality
- Arrays store ordered collections of values
- Use appropriate methods for adding/removing elements
- Array methods like map, filter, reduce are powerful tools
- Objects and arrays are passed by reference
- Practice with real-world data structures
- Understand the difference between objects and arrays

## Next Steps

In the next lesson, we'll learn about:
- Variable scope and how it works
- Hoisting and how JavaScript processes code
- Global scope vs local scope
- Block scope with let and const
- Scope chain and lexical scoping

---

**Ready to understand how JavaScript manages scope? Let's dive in! 🚀**
