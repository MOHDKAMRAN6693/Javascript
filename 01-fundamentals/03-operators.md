# 03 - Operators

## What are Operators?

Operators are symbols that perform operations on values (operands). They are the building blocks of JavaScript expressions and statements.

## Arithmetic Operators

### Basic Arithmetic
```javascript
let a = 10;
let b = 3;

console.log(a + b);  // 13 (addition)
console.log(a - b);  // 7 (subtraction)
console.log(a * b);  // 30 (multiplication)
console.log(a / b);  // 3.333... (division)
console.log(a % b);  // 1 (modulus/remainder)
console.log(a ** b); // 1000 (exponentiation)
```

### Increment and Decrement
```javascript
let x = 5;

// Post-increment (use then increment)
console.log(x++); // 5 (returns 5, then x becomes 6)
console.log(x);   // 6

// Pre-increment (increment then use)
console.log(++x); // 7 (x becomes 7, then returns 7)

// Post-decrement
console.log(x--); // 7 (returns 7, then x becomes 6)

// Pre-decrement
console.log(--x); // 5 (x becomes 5, then returns 5)
```

### Unary Plus and Minus
```javascript
let str = "123";
let num = +str;        // 123 (converts string to number)
let negative = -num;   // -123 (makes number negative)

console.log(+true);    // 1
console.log(+false);   // 0
console.log(+"");      // 0
console.log(+"hello"); // NaN
```

## Assignment Operators

### Basic Assignment
```javascript
let x = 10;
let y = 5;

x = y;        // x is now 5
x += y;       // x = x + y (x is now 10)
x -= y;       // x = x - y (x is now 5)
x *= y;       // x = x * y (x is now 25)
x /= y;       // x = x / y (x is now 5)
x %= y;       // x = x % y (x is now 0)
x **= y;      // x = x ** y (x is now 0)
```

### String Assignment
```javascript
let message = "Hello";
message += " World";  // "Hello World"
message += "!";       // "Hello World!"
```

## Comparison Operators

### Equality Operators
```javascript
let a = 5;
let b = "5";

// Loose equality (==) - performs type coercion
console.log(a == b);   // true (5 == "5")
console.log(a == 5);   // true
console.log(a == "5"); // true

// Strict equality (===) - no type coercion
console.log(a === b);   // false (5 !== "5")
console.log(a === 5);  // true
console.log(a === "5"); // false

// Inequality
console.log(a != b);   // false (loose)
console.log(a !== b);  // true (strict)
```

### Relational Operators
```javascript
let x = 10;
let y = 5;

console.log(x > y);   // true
console.log(x < y);   // false
console.log(x >= y);  // true
console.log(x <= y);  // false
```

### String Comparison
```javascript
console.log("apple" < "banana");  // true (lexicographic order)
console.log("A" < "a");          // true (ASCII values)
console.log("10" < "2");         // true (string comparison, not numeric)
```

## Logical Operators

### AND (&&)
```javascript
let age = 25;
let hasLicense = true;

// Both conditions must be true
if (age >= 18 && hasLicense) {
    console.log("Can drive");
}

// Short-circuit evaluation
let result = false && console.log("This won't print"); // false
```

### OR (||)
```javascript
let isStudent = true;
let hasDiscount = false;

// At least one condition must be true
if (isStudent || hasDiscount) {
    console.log("Gets discount");
}

// Default values
let name = "" || "Anonymous"; // "Anonymous"
let count = null || 0;       // 0
```

### NOT (!)
```javascript
let isLoggedIn = false;

if (!isLoggedIn) {
    console.log("Please log in");
}

// Double negation for boolean conversion
let value = "hello";
let isTruthy = !!value; // true
```

## Ternary Operator (Conditional)

```javascript
let age = 20;
let status = age >= 18 ? "adult" : "minor";
console.log(status); // "adult"

// Nested ternary (use sparingly)
let grade = 85;
let letterGrade = grade >= 90 ? "A" : 
                  grade >= 80 ? "B" : 
                  grade >= 70 ? "C" : "F";
```

## Nullish Coalescing (??)

```javascript
let name = null;
let defaultName = "Anonymous";
let result = name ?? defaultName; // "Anonymous"

// Different from || operator
let count = 0;
let defaultCount = 10;
console.log(count || defaultCount);  // 10 (0 is falsy)
console.log(count ?? defaultCount);  // 0 (0 is not nullish)
```

## Optional Chaining (?.)

```javascript
let user = {
    name: "John",
    address: {
        street: "123 Main St",
        city: "New York"
    }
};

// Safe property access
console.log(user?.name);           // "John"
console.log(user?.address?.city); // "New York"
console.log(user?.phone?.number); // undefined (no error)

// With arrays
let users = [{ name: "John" }];
console.log(users?.[0]?.name);     // "John"
console.log(users?.[1]?.name);     // undefined
```

## Operator Precedence

```javascript
// Multiplication has higher precedence than addition
let result = 2 + 3 * 4;     // 14 (not 20)
let result2 = (2 + 3) * 4;   // 20

// Common precedence (from highest to lowest)
// 1. () - grouping
// 2. ++ -- - unary operators
// 3. * / % - multiplication, division, modulus
// 4. + - - addition, subtraction
// 5. < <= > >= - relational
// 6. == != === !== - equality
// 7. && - logical AND
// 8. || - logical OR
// 9. ?: - ternary
// 10. = += -= etc. - assignment
```

## Typeof Operator

```javascript
console.log(typeof 42);           // "number"
console.log(typeof "hello");      // "string"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof null);         // "object" (quirk!)
console.log(typeof {});           // "object"
console.log(typeof []);           // "object"
console.log(typeof function(){}); // "function"
```

## Practice Exercises

### Exercise 1: Calculator
Create a simple calculator that performs basic arithmetic operations:
```javascript
let a = 10;
let b = 3;

// Calculate and display:
// - Sum
// - Difference
// - Product
// - Quotient
// - Remainder
// - Power
```

### Exercise 2: Age Checker
```javascript
let age = 20;
let hasLicense = true;
let hasInsurance = false;

// Check if person can drive (18+ and has license)
// Check if person can rent a car (21+ and has license and insurance)
// Display appropriate messages
```

### Exercise 3: Grade Calculator
```javascript
let score = 85;

// Calculate letter grade:
// A: 90-100
// B: 80-89
// C: 70-79
// D: 60-69
// F: 0-59
```

### Exercise 4: String Operations
```javascript
let firstName = "john";
let lastName = "doe";

// Create full name in proper case
// Check if name is longer than 5 characters
// Create initials (J.D.)
```

### Exercise 5: Temperature Converter
```javascript
let celsius = 25;

// Convert to Fahrenheit: F = C * 9/5 + 32
// Convert to Kelvin: K = C + 273.15
// Display all three temperatures
```

## Common Mistakes

### 1. Using = instead of == or ===
```javascript
// Wrong
if (x = 5) { } // Assignment, not comparison

// Correct
if (x === 5) { } // Comparison
```

### 2. Confusing == and ===
```javascript
let a = 5;
let b = "5";

console.log(a == b);  // true (type coercion)
console.log(a === b); // false (strict comparison)
```

### 3. Operator precedence issues
```javascript
// Confusing
let result = 2 + 3 * 4; // 14, not 20

// Clear
let result = 2 + (3 * 4); // 14
let result = (2 + 3) * 4;  // 20
```

### 4. NaN comparisons
```javascript
let value = NaN;

// Wrong
console.log(value === NaN); // false (NaN is never equal to itself)

// Correct
console.log(isNaN(value)); // true
console.log(Number.isNaN(value)); // true
```

## Key Takeaways

- Use `===` and `!==` for strict comparisons
- Understand operator precedence and use parentheses when needed
- Be careful with type coercion in comparisons
- Use logical operators for complex conditions
- Practice with different operator combinations
- Remember that `NaN` is never equal to itself

## Next Steps

In the next lesson, we'll learn about:
- if/else statements for decision making
- switch statements for multiple conditions
- for loops for repetition
- while loops for conditional repetition
- break and continue statements

---

**Ready to control the flow of your programs? Let's learn about Control Structures! 🚀**
