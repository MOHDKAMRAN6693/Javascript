# 22 - Error Handling

Error handling is crucial for building robust JavaScript applications. It allows you to gracefully handle unexpected situations and provide meaningful feedback to users.

## 📚 Table of Contents
- [Types of Errors](#types-of-errors)
- [Try-Catch-Finally](#try-catch-finally)
- [Throwing Errors](#throwing-errors)
- [Custom Error Classes](#custom-error-classes)
- [Error Handling in Promises](#error-handling-in-promises)
- [Error Handling in Async/Await](#error-handling-in-asyncawait)
- [Global Error Handling](#global-error-handling)
- [Best Practices](#best-practices)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## Types of Errors

JavaScript has several built-in error types:

```javascript
// SyntaxError - Invalid syntax
// const x = ; // SyntaxError: Unexpected token ';'

// ReferenceError - Undefined variable
// console.log(undefinedVariable); // ReferenceError: undefinedVariable is not defined

// TypeError - Wrong type of value
// const str = "hello";
// str.push("world"); // TypeError: str.push is not a function

// RangeError - Value out of range
// const arr = new Array(-1); // RangeError: Invalid array length

// URIError - Invalid URI
// decodeURIComponent('%'); // URIError: URI malformed

// EvalError - Error in eval() function
// eval('const x = ;'); // SyntaxError (eval creates new execution context)
```

## Try-Catch-Finally

The `try-catch-finally` block is the primary error handling mechanism.

```javascript
function divide(a, b) {
  try {
    if (typeof a !== 'number' || typeof b !== 'number') {
      throw new TypeError('Both arguments must be numbers');
    }
    
    if (b === 0) {
      throw new Error('Division by zero is not allowed');
    }
    
    const result = a / b;
    console.log(`Result: ${result}`);
    return result;
  } catch (error) {
    console.error('Error occurred:', error.message);
    return null;
  } finally {
    console.log('Division operation completed');
  }
}

console.log(divide(10, 2));  // Result: 5, Division operation completed
console.log(divide(10, 0));  // Error occurred: Division by zero is not allowed, Division operation completed
console.log(divide("10", 2)); // Error occurred: Both arguments must be numbers, Division operation completed
```

## Throwing Errors

Use `throw` to create custom errors.

```javascript
function validateEmail(email) {
  if (!email) {
    throw new Error('Email is required');
  }
  
  if (typeof email !== 'string') {
    throw new TypeError('Email must be a string');
  }
  
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailRegex.test(email)) {
    throw new Error('Invalid email format');
  }
  
  return true;
}

// Usage
try {
  validateEmail('user@example.com'); // Valid
  validateEmail('invalid-email');    // Throws error
} catch (error) {
  console.error('Validation failed:', error.message);
}
```

## Custom Error Classes

Create custom error classes for specific error types.

```javascript
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
  }
}

class NetworkError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = 'NetworkError';
    this.statusCode = statusCode;
  }
}

class BusinessLogicError extends Error {
  constructor(message, code) {
    super(message);
    this.name = 'BusinessLogicError';
    this.code = code;
  }
}

// Usage
function processUser(user) {
  try {
    if (!user.name) {
      throw new ValidationError('Name is required', 'name');
    }
    
    if (!user.email) {
      throw new ValidationError('Email is required', 'email');
    }
    
    if (user.age < 18) {
      throw new BusinessLogicError('User must be at least 18 years old', 'AGE_RESTRICTION');
    }
    
    return { success: true, user };
  } catch (error) {
    if (error instanceof ValidationError) {
      console.error(`Validation error in field '${error.field}': ${error.message}`);
    } else if (error instanceof BusinessLogicError) {
      console.error(`Business logic error (${error.code}): ${error.message}`);
    } else {
      console.error('Unexpected error:', error.message);
    }
    throw error;
  }
}
```

## Error Handling in Promises

Handle errors in promises using `.catch()` or try-catch with async/await.

```javascript
// Using .catch()
function fetchUserData(userId) {
  return fetch(`/api/users/${userId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json();
    })
    .catch(error => {
      console.error('Failed to fetch user data:', error.message);
      throw error;
    });
}

// Usage
fetchUserData(123)
  .then(user => console.log('User data:', user))
  .catch(error => console.error('Error:', error.message));

// Using async/await
async function fetchUserDataAsync(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const user = await response.json();
    return user;
  } catch (error) {
    console.error('Failed to fetch user data:', error.message);
    throw error;
  }
}
```

## Error Handling in Async/Await

```javascript
async function processMultipleUsers(userIds) {
  const results = [];
  const errors = [];
  
  for (const userId of userIds) {
    try {
      const user = await fetchUserDataAsync(userId);
      results.push(user);
    } catch (error) {
      console.error(`Failed to process user ${userId}:`, error.message);
      errors.push({ userId, error: error.message });
    }
  }
  
  return { results, errors };
}

// Usage
processMultipleUsers([1, 2, 3, 999])
  .then(({ results, errors }) => {
    console.log('Successful results:', results.length);
    console.log('Errors:', errors.length);
  });
```

## Global Error Handling

Handle uncaught errors globally.

```javascript
// Handle uncaught exceptions
process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error);
  // Perform cleanup
  process.exit(1);
});

// Handle unhandled promise rejections
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection at:', promise, 'reason:', reason);
  // Perform cleanup
});

// Browser global error handling
window.addEventListener('error', (event) => {
  console.error('Global error:', event.error);
});

window.addEventListener('unhandledrejection', (event) => {
  console.error('Unhandled promise rejection:', event.reason);
  event.preventDefault(); // Prevent default browser behavior
});
```

## Best Practices

### 1. Specific Error Messages

```javascript
// Bad
throw new Error('Error');

// Good
throw new Error('Failed to connect to database: Connection timeout after 30 seconds');
```

### 2. Error Logging

```javascript
class Logger {
  static logError(error, context = {}) {
    const errorInfo = {
      message: error.message,
      stack: error.stack,
      timestamp: new Date().toISOString(),
      context
    };
    
    console.error('Error logged:', errorInfo);
    // Send to logging service
  }
}

function riskyOperation() {
  try {
    // Some risky operation
    throw new Error('Something went wrong');
  } catch (error) {
    Logger.logError(error, { operation: 'riskyOperation', userId: 123 });
    throw error;
  }
}
```

### 3. Error Recovery

```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url);
      if (response.ok) {
        return await response.json();
      }
      throw new Error(`HTTP ${response.status}`);
    } catch (error) {
      console.error(`Attempt ${attempt} failed:`, error.message);
      
      if (attempt === maxRetries) {
        throw new Error(`Failed after ${maxRetries} attempts: ${error.message}`);
      }
      
      // Wait before retry
      await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
    }
  }
}
```

## Practical Examples

### Example 1: File Processing with Error Handling

```javascript
class FileProcessor {
  constructor() {
    this.processedFiles = [];
    this.errors = [];
  }
  
  async processFile(filePath) {
    try {
      // Simulate file reading
      const content = await this.readFile(filePath);
      const processedContent = this.processContent(content);
      const result = await this.saveFile(filePath, processedContent);
      
      this.processedFiles.push({ filePath, result });
      return result;
    } catch (error) {
      this.errors.push({ filePath, error: error.message });
      throw error;
    }
  }
  
  async readFile(filePath) {
    // Simulate file reading
    if (!filePath) {
      throw new Error('File path is required');
    }
    
    if (filePath.includes('corrupt')) {
      throw new Error('File is corrupted and cannot be read');
    }
    
    return `Content of ${filePath}`;
  }
  
  processContent(content) {
    if (!content) {
      throw new Error('Content is empty');
    }
    
    return content.toUpperCase();
  }
  
  async saveFile(filePath, content) {
    // Simulate file saving
    if (filePath.includes('readonly')) {
      throw new Error('File is read-only and cannot be modified');
    }
    
    return { filePath, content, saved: true };
  }
  
  async processMultipleFiles(filePaths) {
    const results = [];
    
    for (const filePath of filePaths) {
      try {
        const result = await this.processFile(filePath);
        results.push({ success: true, filePath, result });
      } catch (error) {
        results.push({ success: false, filePath, error: error.message });
      }
    }
    
    return results;
  }
}

// Usage
const processor = new FileProcessor();
const files = ['file1.txt', 'corrupt.txt', 'readonly.txt', 'file2.txt'];

processor.processMultipleFiles(files)
  .then(results => {
    console.log('Processing results:');
    results.forEach(result => {
      if (result.success) {
        console.log(`✅ ${result.filePath}: Success`);
      } else {
        console.log(`❌ ${result.filePath}: ${result.error}`);
      }
    });
  });
```

### Example 2: API Client with Error Handling

```javascript
class ApiClient {
  constructor(baseUrl, options = {}) {
    this.baseUrl = baseUrl;
    this.timeout = options.timeout || 5000;
    this.retries = options.retries || 3;
  }
  
  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const requestOptions = {
      timeout: this.timeout,
      ...options
    };
    
    try {
      const response = await this.fetchWithTimeout(url, requestOptions);
      
      if (!response.ok) {
        throw new ApiError(
          `HTTP ${response.status}: ${response.statusText}`,
          response.status,
          response
        );
      }
      
      return await response.json();
    } catch (error) {
      if (error instanceof ApiError) {
        throw error;
      }
      
      throw new NetworkError(`Request failed: ${error.message}`);
    }
  }
  
  async fetchWithTimeout(url, options) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), this.timeout);
    
    try {
      const response = await fetch(url, {
        ...options,
        signal: controller.signal
      });
      clearTimeout(timeoutId);
      return response;
    } catch (error) {
      clearTimeout(timeoutId);
      if (error.name === 'AbortError') {
        throw new TimeoutError(`Request timed out after ${this.timeout}ms`);
      }
      throw error;
    }
  }
  
  async requestWithRetry(endpoint, options = {}) {
    let lastError;
    
    for (let attempt = 1; attempt <= this.retries; attempt++) {
      try {
        return await this.request(endpoint, options);
      } catch (error) {
        lastError = error;
        
        if (attempt === this.retries) {
          throw new Error(`Request failed after ${this.retries} attempts: ${error.message}`);
        }
        
        // Wait before retry (exponential backoff)
        await new Promise(resolve => 
          setTimeout(resolve, 1000 * Math.pow(2, attempt - 1))
        );
      }
    }
  }
}

class ApiError extends Error {
  constructor(message, status, response) {
    super(message);
    this.name = 'ApiError';
    this.status = status;
    this.response = response;
  }
}

class NetworkError extends Error {
  constructor(message) {
    super(message);
    this.name = 'NetworkError';
  }
}

class TimeoutError extends Error {
  constructor(message) {
    super(message);
    this.name = 'TimeoutError';
  }
}

// Usage
const apiClient = new ApiClient('https://api.example.com', {
  timeout: 5000,
  retries: 3
});

async function fetchUserData(userId) {
  try {
    const user = await apiClient.requestWithRetry(`/users/${userId}`);
    return user;
  } catch (error) {
    if (error instanceof ApiError) {
      console.error(`API Error (${error.status}): ${error.message}`);
    } else if (error instanceof NetworkError) {
      console.error(`Network Error: ${error.message}`);
    } else if (error instanceof TimeoutError) {
      console.error(`Timeout Error: ${error.message}`);
    } else {
      console.error(`Unexpected Error: ${error.message}`);
    }
    throw error;
  }
}
```

### Example 3: Form Validation with Error Handling

```javascript
class FormValidator {
  constructor() {
    this.errors = {};
  }
  
  validateField(fieldName, value, rules) {
    const fieldErrors = [];
    
    for (const rule of rules) {
      try {
        const result = this.applyRule(value, rule);
        if (!result.valid) {
          fieldErrors.push(result.message);
        }
      } catch (error) {
        fieldErrors.push(`Validation error: ${error.message}`);
      }
    }
    
    if (fieldErrors.length > 0) {
      this.errors[fieldName] = fieldErrors;
    } else {
      delete this.errors[fieldName];
    }
    
    return fieldErrors.length === 0;
  }
  
  applyRule(value, rule) {
    switch (rule.type) {
      case 'required':
        return {
          valid: value !== null && value !== undefined && value !== '',
          message: rule.message || 'This field is required'
        };
      
      case 'minLength':
        return {
          valid: value.length >= rule.value,
          message: rule.message || `Minimum length is ${rule.value}`
        };
      
      case 'maxLength':
        return {
          valid: value.length <= rule.value,
          message: rule.message || `Maximum length is ${rule.value}`
        };
      
      case 'email':
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return {
          valid: emailRegex.test(value),
          message: rule.message || 'Invalid email format'
        };
      
      case 'pattern':
        return {
          valid: rule.value.test(value),
          message: rule.message || 'Invalid format'
        };
      
      case 'custom':
        return rule.validator(value);
      
      default:
        throw new Error(`Unknown validation rule: ${rule.type}`);
    }
  }
  
  validateForm(formData, validationRules) {
    this.errors = {};
    let isValid = true;
    
    for (const [fieldName, rules] of Object.entries(validationRules)) {
      const fieldValue = formData[fieldName];
      const fieldValid = this.validateField(fieldName, fieldValue, rules);
      if (!fieldValid) {
        isValid = false;
      }
    }
    
    return { isValid, errors: this.errors };
  }
}

// Usage
const validator = new FormValidator();

const formData = {
  name: 'John Doe',
  email: 'john@example.com',
  password: 'password123',
  confirmPassword: 'password123'
};

const validationRules = {
  name: [
    { type: 'required', message: 'Name is required' },
    { type: 'minLength', value: 2, message: 'Name must be at least 2 characters' }
  ],
  email: [
    { type: 'required', message: 'Email is required' },
    { type: 'email', message: 'Please enter a valid email' }
  ],
  password: [
    { type: 'required', message: 'Password is required' },
    { type: 'minLength', value: 8, message: 'Password must be at least 8 characters' },
    { type: 'pattern', value: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/, message: 'Password must contain uppercase, lowercase, and number' }
  ],
  confirmPassword: [
    { type: 'required', message: 'Please confirm your password' },
    { type: 'custom', validator: (value) => ({
      valid: value === formData.password,
      message: 'Passwords do not match'
    })}
  ]
};

const result = validator.validateForm(formData, validationRules);

if (result.isValid) {
  console.log('Form is valid!');
} else {
  console.log('Form has errors:', result.errors);
}
```

## Exercises

### Exercise 1: Calculator with Error Handling
Create a calculator class that handles division by zero, invalid inputs, and overflow errors gracefully.

### Exercise 2: File Upload with Validation
Build a file upload system that validates file types, sizes, and handles upload errors.

### Exercise 3: Database Connection Manager
Create a database connection manager that handles connection failures, timeouts, and retries.

### Exercise 4: API Rate Limiter
Implement a rate limiter that handles API quota exceeded errors and implements exponential backoff.

### Exercise 5: User Authentication System
Build an authentication system with proper error handling for invalid credentials, expired tokens, and network failures.

## Key Takeaways

- Use try-catch-finally for synchronous error handling
- Create custom error classes for specific error types
- Handle errors in promises with .catch() or async/await
- Implement global error handling for uncaught exceptions
- Provide specific, actionable error messages
- Log errors with context for debugging
- Implement retry mechanisms for transient errors
- Validate inputs and handle edge cases
- Use error recovery strategies when possible
- Test error scenarios thoroughly

Proper error handling makes your applications more robust, user-friendly, and easier to debug and maintain.
