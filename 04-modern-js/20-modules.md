# 20 - Modules

Modules allow you to split your JavaScript code into separate files, making your code more organized, reusable, and maintainable. ES6 introduced a standardized module system for JavaScript.

## 📚 Table of Contents
- [What are Modules?](#what-are-modules)
- [Export Syntax](#export-syntax)
- [Import Syntax](#import-syntax)
- [Default Exports](#default-exports)
- [Named Exports](#named-exports)
- [Mixed Exports](#mixed-exports)
- [Re-exporting](#re-exporting)
- [Dynamic Imports](#dynamic-imports)
- [Module Scope](#module-scope)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## What are Modules?

Modules are JavaScript files that can export and import functionality. Each module has its own scope, preventing global namespace pollution.

```javascript
// math.js - A module file
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

## Export Syntax

### Named Exports

Export individual functions, variables, or classes using the `export` keyword.

```javascript
// utils.js
export const PI = 3.14159;

export function multiply(a, b) {
  return a * b;
}

export function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero");
  }
  return a / b;
}

export class Calculator {
  constructor() {
    this.history = [];
  }
  
  calculate(operation, a, b) {
    let result;
    switch(operation) {
      case 'add':
        result = a + b;
        break;
      case 'subtract':
        result = a - b;
        break;
      case 'multiply':
        result = multiply(a, b);
        break;
      case 'divide':
        result = divide(a, b);
        break;
      default:
        throw new Error("Unknown operation");
    }
    
    this.history.push({ operation, a, b, result });
    return result;
  }
}
```

### Default Exports

Each module can have one default export.

```javascript
// user.js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
    this.createdAt = new Date();
  }
  
  getInfo() {
    return `${this.name} (${this.email})`;
  }
}

export default User;
```

### Mixed Exports

You can combine default and named exports in the same module.

```javascript
// api.js
const API_BASE_URL = 'https://api.example.com';

export function fetchUser(id) {
  return fetch(`${API_BASE_URL}/users/${id}`)
    .then(response => response.json());
}

export function fetchPosts(userId) {
  return fetch(`${API_BASE_URL}/users/${userId}/posts`)
    .then(response => response.json());
}

export default class ApiClient {
  constructor(apiKey) {
    this.apiKey = apiKey;
  }
  
  async makeRequest(endpoint) {
    const response = await fetch(`${API_BASE_URL}${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${this.apiKey}`
      }
    });
    return response.json();
  }
}
```

## Import Syntax

### Importing Named Exports

```javascript
// main.js
import { multiply, divide, PI } from './utils.js';
import { Calculator } from './utils.js';

console.log(multiply(4, 5)); // 20
console.log(divide(10, 2)); // 5
console.log(PI); // 3.14159

const calc = new Calculator();
console.log(calc.calculate('add', 3, 4)); // 7
```

### Importing Default Exports

```javascript
// main.js
import User from './user.js';

const user = new User('John Doe', 'john@example.com');
console.log(user.getInfo()); // "John Doe (john@example.com)"
```

### Importing Everything

```javascript
// main.js
import * as utils from './utils.js';

console.log(utils.multiply(3, 4)); // 12
console.log(utils.PI); // 3.14159

const calc = new utils.Calculator();
```

### Mixed Imports

```javascript
// main.js
import ApiClient, { fetchUser, fetchPosts } from './api.js';

const api = new ApiClient('your-api-key');
const user = await fetchUser(123);
const posts = await fetchPosts(123);
```

## Re-exporting

You can re-export items from other modules.

```javascript
// index.js - Barrel export
export { multiply, divide, PI } from './utils.js';
export { default as User } from './user.js';
export { fetchUser, fetchPosts, default as ApiClient } from './api.js';
```

```javascript
// main.js
import { multiply, User, ApiClient } from './index.js';
```

## Dynamic Imports

Dynamic imports allow you to load modules conditionally or on-demand.

```javascript
// Dynamic import with async/await
async function loadModule() {
  const { multiply, divide } = await import('./utils.js');
  console.log(multiply(3, 4));
}

// Dynamic import with promises
function loadModule() {
  import('./utils.js')
    .then(module => {
      console.log(module.multiply(3, 4));
    })
    .catch(error => {
      console.error('Failed to load module:', error);
    });
}

// Conditional loading
async function loadUserModule(needsUser) {
  if (needsUser) {
    const { default: User } = await import('./user.js');
    return new User('Guest', 'guest@example.com');
  }
  return null;
}
```

## Module Scope

Modules have their own scope, and variables are not automatically global.

```javascript
// module1.js
const privateVariable = "I'm private";
let counter = 0;

export function increment() {
  counter++;
  return counter;
}

export function getCounter() {
  return counter;
}

// This won't be accessible from other modules
// console.log(privateVariable); // Error if imported
```

```javascript
// module2.js
import { increment, getCounter } from './module1.js';

console.log(increment()); // 1
console.log(getCounter()); // 1
// console.log(counter); // Error: counter is not defined
```

## Practical Examples

### Example 1: E-commerce System

```javascript
// product.js
export class Product {
  constructor(id, name, price, category) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.category = category;
    this.inStock = true;
  }
  
  applyDiscount(percentage) {
    this.price = this.price * (1 - percentage / 100);
    return this;
  }
  
  getFormattedPrice() {
    return `$${this.price.toFixed(2)}`;
  }
}

export function createProduct(id, name, price, category) {
  return new Product(id, name, price, category);
}

export const CATEGORIES = {
  ELECTRONICS: 'electronics',
  CLOTHING: 'clothing',
  BOOKS: 'books',
  HOME: 'home'
};
```

```javascript
// cart.js
import { Product, CATEGORIES } from './product.js';

export class ShoppingCart {
  constructor() {
    this.items = [];
    this.total = 0;
  }
  
  addItem(product, quantity = 1) {
    const existingItem = this.items.find(item => item.product.id === product.id);
    
    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.items.push({ product, quantity });
    }
    
    this.calculateTotal();
  }
  
  removeItem(productId) {
    this.items = this.items.filter(item => item.product.id !== productId);
    this.calculateTotal();
  }
  
  calculateTotal() {
    this.total = this.items.reduce((sum, item) => {
      return sum + (item.product.price * item.quantity);
    }, 0);
  }
  
  getTotal() {
    return this.total.toFixed(2);
  }
  
  getItemCount() {
    return this.items.reduce((count, item) => count + item.quantity, 0);
  }
  
  clear() {
    this.items = [];
    this.total = 0;
  }
}
```

```javascript
// main.js
import { createProduct, CATEGORIES } from './product.js';
import { ShoppingCart } from './cart.js';

// Create products
const laptop = createProduct(1, 'Gaming Laptop', 1299.99, CATEGORIES.ELECTRONICS);
const shirt = createProduct(2, 'Cotton T-Shirt', 24.99, CATEGORIES.CLOTHING);
const book = createProduct(3, 'JavaScript Guide', 39.99, CATEGORIES.BOOKS);

// Create shopping cart
const cart = new ShoppingCart();

// Add items to cart
cart.addItem(laptop, 1);
cart.addItem(shirt, 2);
cart.addItem(book, 1);

console.log(`Cart total: $${cart.getTotal()}`);
console.log(`Items in cart: ${cart.getItemCount()}`);
```

### Example 2: Utility Library

```javascript
// string-utils.js
export function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

export function reverse(str) {
  return str.split('').reverse().join('');
}

export function truncate(str, length) {
  if (str.length <= length) return str;
  return str.slice(0, length) + '...';
}

export function slugify(str) {
  return str
    .toLowerCase()
    .replace(/[^a-z0-9 -]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-');
}
```

```javascript
// date-utils.js
export function formatDate(date, format = 'YYYY-MM-DD') {
  const d = new Date(date);
  const year = d.getFullYear();
  const month = String(d.getMonth() + 1).padStart(2, '0');
  const day = String(d.getDate()).padStart(2, '0');
  
  return format
    .replace('YYYY', year)
    .replace('MM', month)
    .replace('DD', day);
}

export function addDays(date, days) {
  const result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
}

export function isWeekend(date) {
  const day = new Date(date).getDay();
  return day === 0 || day === 6;
}
```

```javascript
// math-utils.js
export function random(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

export function roundToDecimal(num, decimals) {
  return Math.round(num * Math.pow(10, decimals)) / Math.pow(10, decimals);
}

export function clamp(value, min, max) {
  return Math.min(Math.max(value, min), max);
}
```

```javascript
// index.js - Barrel export
export * from './string-utils.js';
export * from './date-utils.js';
export * from './math-utils.js';
```

```javascript
// main.js
import { capitalize, formatDate, random } from './index.js';

console.log(capitalize('hello world')); // "Hello world"
console.log(formatDate(new Date())); // "2024-01-15"
console.log(random(1, 10)); // Random number between 1 and 10
```

### Example 3: Configuration Management

```javascript
// config.js
const config = {
  api: {
    baseUrl: process.env.API_BASE_URL || 'https://api.example.com',
    timeout: 5000,
    retries: 3
  },
  database: {
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 5432,
    name: process.env.DB_NAME || 'myapp'
  },
  features: {
    enableLogging: process.env.ENABLE_LOGGING === 'true',
    enableCache: process.env.ENABLE_CACHE === 'true'
  }
};

export default config;
```

```javascript
// logger.js
import config from './config.js';

class Logger {
  constructor() {
    this.enabled = config.features.enableLogging;
  }
  
  log(level, message, data = {}) {
    if (!this.enabled) return;
    
    const timestamp = new Date().toISOString();
    const logEntry = {
      timestamp,
      level,
      message,
      data
    };
    
    console.log(JSON.stringify(logEntry));
  }
  
  info(message, data) {
    this.log('INFO', message, data);
  }
  
  error(message, data) {
    this.log('ERROR', message, data);
  }
  
  warn(message, data) {
    this.log('WARN', message, data);
  }
}

export default new Logger();
```

```javascript
// api-client.js
import config from './config.js';
import logger from './logger.js';

export class ApiClient {
  constructor() {
    this.baseUrl = config.api.baseUrl;
    this.timeout = config.api.timeout;
    this.retries = config.api.retries;
  }
  
  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const requestOptions = {
      timeout: this.timeout,
      ...options
    };
    
    logger.info('Making API request', { url, options: requestOptions });
    
    try {
      const response = await fetch(url, requestOptions);
      const data = await response.json();
      
      logger.info('API request successful', { status: response.status });
      return data;
    } catch (error) {
      logger.error('API request failed', { error: error.message });
      throw error;
    }
  }
  
  async get(endpoint) {
    return this.request(endpoint, { method: 'GET' });
  }
  
  async post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
  }
}
```

## Exercises

### Exercise 1: Calculator Module
Create a calculator module with basic operations (add, subtract, multiply, divide) and a history feature. Import and use it in a main file.

### Exercise 2: User Management System
Create separate modules for User class, UserService, and UserValidator. Import them in a main file to create a complete user management system.

### Exercise 3: Weather App Modules
Create modules for weather data fetching, weather display, and weather utilities. Use dynamic imports to load modules based on user preferences.

### Exercise 4: Game Engine
Create a modular game engine with separate modules for Player, Game, Score, and Renderer classes.

### Exercise 5: Blog System
Create a blog system with modules for Post, Comment, User, and Blog classes. Use barrel exports to organize the modules.

## Key Takeaways

- Modules help organize code into separate, reusable files
- Use `export` to make functions, classes, or variables available to other modules
- Use `import` to use functionality from other modules
- Default exports allow one main export per module
- Named exports allow multiple exports per module
- Dynamic imports enable conditional module loading
- Modules have their own scope, preventing global namespace pollution
- Use barrel exports to create clean import interfaces

Modules are essential for building large, maintainable JavaScript applications. They promote code organization, reusability, and separation of concerns.
