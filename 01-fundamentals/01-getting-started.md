# 01 - Getting Started with JavaScript

## What is JavaScript?

JavaScript is a high-level, interpreted programming language that is one of the core technologies of the World Wide Web, alongside HTML and CSS. It enables interactive web pages and is an essential part of web applications.

## Why Learn JavaScript?

- **Universal Language**: Runs everywhere - browsers, servers, mobile apps, desktop apps
- **High Demand**: One of the most in-demand programming languages
- **Versatile**: Frontend, backend, mobile development, and more
- **Large Community**: Extensive libraries and frameworks
- **Easy to Start**: Simple syntax, no complex setup required

## Setting Up Your Development Environment

### 1. Code Editor
We recommend **Visual Studio Code** with these extensions:
- JavaScript (ES6) code snippets
- Prettier - Code formatter
- Live Server (for running HTML files)

### 2. Browser Developer Tools
- **Chrome DevTools**: Press F12 or Ctrl+Shift+I
- **Firefox Developer Tools**: Press F12
- **Safari Web Inspector**: Enable in Preferences

### 3. Node.js (Optional)
For server-side JavaScript development:
```bash
# Download from https://nodejs.org/
# Verify installation
node --version
npm --version
```

## Your First JavaScript Program

### Method 1: In HTML File
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First JavaScript</title>
</head>
<body>
    <h1>Hello World!</h1>
    <script>
        console.log("Hello, JavaScript!");
        alert("Welcome to JavaScript!");
    </script>
</body>
</html>
```

### Method 2: External JavaScript File
**index.html**
```html
<!DOCTYPE html>
<html>
<head>
    <title>External JavaScript</title>
</head>
<body>
    <h1>Learning JavaScript</h1>
    <script src="script.js"></script>
</body>
</html>
```

**script.js**
```javascript
console.log("Hello from external file!");
document.write("<p>This text was added by JavaScript!</p>");
```

### Method 3: Using Node.js
**hello.js**
```javascript
console.log("Hello, World!");
console.log("Welcome to JavaScript programming!");
```

Run with: `node hello.js`

## Understanding the Console

The console is your best friend for debugging and learning:

```javascript
// Different types of console output
console.log("Regular message");
console.warn("Warning message");
console.error("Error message");
console.info("Information message");

// Console with variables
let name = "John";
let age = 25;
console.log("Name:", name, "Age:", age);
```

## JavaScript Syntax Basics

### Comments
```javascript
// Single line comment

/*
Multi-line comment
This is useful for longer explanations
*/

/**
 * JSDoc comment
 * Used for documenting functions
 */
```

### Semicolons
```javascript
// Semicolons are optional but recommended
let x = 5;
let y = 10;

// This also works (but not recommended)
let a = 5
let b = 10
```

### Case Sensitivity
```javascript
// JavaScript is case-sensitive
let myVariable = "Hello";
let MyVariable = "World";  // Different variable
let MYVARIABLE = "JS";     // Another different variable
```

## Running JavaScript Code

### 1. Browser Console
- Open browser (Chrome, Firefox, Safari)
- Press F12 to open Developer Tools
- Go to Console tab
- Type JavaScript code and press Enter

### 2. HTML File
- Create an HTML file
- Add `<script>` tags
- Open in browser

### 3. Node.js
- Create a `.js` file
- Run with `node filename.js`

## Practice Exercises

### Exercise 1: Hello World
Create a program that displays "Hello, World!" in three different ways:
1. Using `console.log()`
2. Using `alert()`
3. Using `document.write()`

### Exercise 2: Personal Introduction
Write a program that introduces yourself with:
- Your name
- Your age
- Your favorite programming language
- One interesting fact about yourself

### Exercise 3: Simple Calculator
Create a basic calculator that adds two numbers and displays the result.

## Common Mistakes to Avoid

1. **Forgetting quotes around strings**
   ```javascript
   // Wrong
   let message = Hello World;
   
   // Correct
   let message = "Hello World";
   ```

2. **Case sensitivity errors**
   ```javascript
   // Wrong
   Console.log("Hello");
   
   // Correct
   console.log("Hello");
   ```

3. **Missing semicolons (in some cases)**
   ```javascript
   // Can cause issues
   let x = 5
   let y = x + 2
   
   // Better
   let x = 5;
   let y = x + 2;
   ```

## Next Steps

In the next lesson, we'll learn about:
- Variables and how to store data
- Different data types in JavaScript
- How to work with numbers, strings, and booleans

## Key Takeaways

- JavaScript is a versatile programming language
- You can run JavaScript in browsers, Node.js, or other environments
- The console is essential for debugging and learning
- Always use proper syntax and follow best practices
- Practice regularly to improve your skills

---

**Ready for the next lesson? Let's learn about Variables and Data Types! 🚀**
