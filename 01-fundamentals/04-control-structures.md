# 04 - Control Structures

## What are Control Structures?

Control structures allow you to control the flow of execution in your JavaScript programs. They help you make decisions, repeat code, and handle different scenarios.

## Conditional Statements

### if Statement
```javascript
let age = 18;

if (age >= 18) {
    console.log("You are an adult");
}
```

### if...else Statement
```javascript
let age = 16;

if (age >= 18) {
    console.log("You can vote");
} else {
    console.log("You cannot vote yet");
}
```

### if...else if...else Statement
```javascript
let score = 85;

if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else if (score >= 60) {
    console.log("Grade: D");
} else {
    console.log("Grade: F");
}
```

### Nested if Statements
```javascript
let age = 20;
let hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        console.log("You can drive");
    } else {
        console.log("You need a license to drive");
    }
} else {
    console.log("You are too young to drive");
}
```

## Switch Statement

```javascript
let day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of work week");
        break;
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
        console.log("Mid week");
        break;
    case "Friday":
        console.log("TGIF!");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend!");
        break;
    default:
        console.log("Invalid day");
}
```

### Switch with Numbers
```javascript
let month = 3;

switch (month) {
    case 1:
        console.log("January");
        break;
    case 2:
        console.log("February");
        break;
    case 3:
        console.log("March");
        break;
    // ... more cases
    default:
        console.log("Invalid month");
}
```

## Loops

### for Loop
```javascript
// Basic for loop
for (let i = 0; i < 5; i++) {
    console.log("Count:", i);
}

// Loop through array
let fruits = ["apple", "banana", "orange"];
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// Reverse loop
for (let i = fruits.length - 1; i >= 0; i--) {
    console.log(fruits[i]);
}
```

### for...of Loop (ES6+)
```javascript
let fruits = ["apple", "banana", "orange"];

for (let fruit of fruits) {
    console.log(fruit);
}

// With strings
let name = "JavaScript";
for (let char of name) {
    console.log(char);
}
```

### for...in Loop
```javascript
let person = {
    name: "John",
    age: 25,
    city: "New York"
};

for (let key in person) {
    console.log(key + ": " + person[key]);
}
```

### while Loop
```javascript
let count = 0;

while (count < 5) {
    console.log("Count:", count);
    count++;
}

// Infinite loop (be careful!)
// while (true) {
//     console.log("This runs forever!");
// }
```

### do...while Loop
```javascript
let count = 0;

do {
    console.log("Count:", count);
    count++;
} while (count < 5);

// This runs at least once, even if condition is false
let x = 10;
do {
    console.log("This runs once");
} while (x < 5);
```

## Loop Control Statements

### break Statement
```javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break; // Exit the loop
    }
    console.log(i); // 0, 1, 2, 3, 4
}
```

### continue Statement
```javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        continue; // Skip this iteration
    }
    console.log(i); // 0, 1, 2, 3, 4, 6, 7, 8, 9
}
```

### Labeled Statements
```javascript
outerLoop: for (let i = 0; i < 3; i++) {
    innerLoop: for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            break outerLoop; // Break out of both loops
        }
        console.log(`i: ${i}, j: ${j}`);
    }
}
```

## Practical Examples

### Example 1: Number Guessing Game
```javascript
let secretNumber = Math.floor(Math.random() * 10) + 1;
let guess = 5;

if (guess === secretNumber) {
    console.log("Congratulations! You guessed it!");
} else if (guess < secretNumber) {
    console.log("Too low! Try a higher number.");
} else {
    console.log("Too high! Try a lower number.");
}
```

### Example 2: FizzBuzz
```javascript
for (let i = 1; i <= 15; i++) {
    if (i % 3 === 0 && i % 5 === 0) {
        console.log("FizzBuzz");
    } else if (i % 3 === 0) {
        console.log("Fizz");
    } else if (i % 5 === 0) {
        console.log("Buzz");
    } else {
        console.log(i);
    }
}
```

### Example 3: Array Processing
```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let sum = 0;
let evenNumbers = [];

for (let number of numbers) {
    sum += number;
    
    if (number % 2 === 0) {
        evenNumbers.push(number);
    }
}

console.log("Sum:", sum);
console.log("Even numbers:", evenNumbers);
```

### Example 4: Password Validation
```javascript
let password = "MyPassword123";
let hasUpperCase = false;
let hasLowerCase = false;
let hasNumber = false;

for (let char of password) {
    if (char >= 'A' && char <= 'Z') {
        hasUpperCase = true;
    } else if (char >= 'a' && char <= 'z') {
        hasLowerCase = true;
    } else if (char >= '0' && char <= '9') {
        hasNumber = true;
    }
}

if (password.length >= 8 && hasUpperCase && hasLowerCase && hasNumber) {
    console.log("Password is strong!");
} else {
    console.log("Password needs improvement");
}
```

## Practice Exercises

### Exercise 1: Grade Calculator
Create a program that:
- Takes a score (0-100)
- Assigns a letter grade (A, B, C, D, F)
- Displays the grade and a message

### Exercise 2: Multiplication Table
Generate a multiplication table for a given number:
```javascript
// For number 5, output:
// 5 x 1 = 5
// 5 x 2 = 10
// 5 x 3 = 15
// ... up to 5 x 10 = 50
```

### Exercise 3: Prime Number Checker
Create a program that checks if a number is prime:
```javascript
let number = 17;
// Check if number is prime
// Display result
```

### Exercise 4: Array Statistics
Given an array of numbers, calculate:
- Sum of all numbers
- Average
- Maximum value
- Minimum value
- Count of even numbers

### Exercise 5: Text Analyzer
Analyze a string and count:
- Total characters
- Vowels (a, e, i, o, u)
- Consonants
- Words
- Sentences

## Common Mistakes

### 1. Missing break in switch
```javascript
// Wrong
switch (day) {
    case "Monday":
        console.log("Monday");
    case "Tuesday":
        console.log("Tuesday");
}

// Correct
switch (day) {
    case "Monday":
        console.log("Monday");
        break;
    case "Tuesday":
        console.log("Tuesday");
        break;
}
```

### 2. Infinite loops
```javascript
// Wrong - missing increment
let i = 0;
while (i < 10) {
    console.log(i);
    // i++; // Missing this line!
}

// Correct
let i = 0;
while (i < 10) {
    console.log(i);
    i++;
}
```

### 3. Using = instead of == or === in conditions
```javascript
// Wrong
if (x = 5) { } // Assignment, not comparison

// Correct
if (x === 5) { } // Comparison
```

### 4. Off-by-one errors
```javascript
// Wrong - might miss the last element
for (let i = 0; i < array.length - 1; i++) {
    console.log(array[i]);
}

// Correct
for (let i = 0; i < array.length; i++) {
    console.log(array[i]);
}
```

## Key Takeaways

- Use `if/else` for simple decisions, `switch` for multiple conditions
- Choose the right loop for your needs: `for` for known iterations, `while` for conditions
- Always use `break` in switch statements to prevent fall-through
- Be careful with loop conditions to avoid infinite loops
- Use `break` and `continue` to control loop flow
- Practice with different control structures to build confidence

## Next Steps

In the next lesson, we'll learn about:
- Function declarations and expressions
- Parameters and arguments
- Return values
- Function scope
- Arrow functions (ES6+)
- Higher-order functions

---

**Ready to organize your code with functions? Let's dive in! 🚀**
