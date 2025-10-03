# 21 - Promises and Async/Await

## What are Promises?

A Promise is an object representing the eventual completion or failure of an asynchronous operation. It provides a way to handle asynchronous operations more elegantly than callbacks.

## Creating Promises

### Basic Promise Creation
```javascript
let promise = new Promise((resolve, reject) => {
    // Asynchronous operation
    setTimeout(() => {
        resolve("Operation completed successfully!");
    }, 1000);
});

promise.then(result => {
    console.log(result); // "Operation completed successfully!"
});
```

### Promise with Error Handling
```javascript
let promise = new Promise((resolve, reject) => {
    let success = Math.random() > 0.5;
    
    if (success) {
        resolve("Success!");
    } else {
        reject("Error occurred!");
    }
});

promise
    .then(result => {
        console.log(result); // "Success!"
    })
    .catch(error => {
        console.log(error); // "Error occurred!"
    });
```

### Promise States
```javascript
let pendingPromise = new Promise((resolve, reject) => {
    // Promise is in pending state
    console.log("Promise is pending");
});

let resolvedPromise = Promise.resolve("Resolved immediately");
let rejectedPromise = Promise.reject("Rejected immediately");

console.log(pendingPromise); // Promise { <pending> }
console.log(resolvedPromise); // Promise { <resolved> }
console.log(rejectedPromise); // Promise { <rejected> }
```

## Working with Promises

### Chaining Promises
```javascript
function fetchUser(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve({ id: id, name: `User ${id}` });
        }, 1000);
    });
}

function fetchUserPosts(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve([`Post 1 by User ${userId}`, `Post 2 by User ${userId}`]);
        }, 1000);
    });
}

// Chain promises
fetchUser(1)
    .then(user => {
        console.log("User:", user);
        return fetchUserPosts(user.id);
    })
    .then(posts => {
        console.log("Posts:", posts);
    })
    .catch(error => {
        console.log("Error:", error);
    });
```

### Promise.all()
```javascript
let promise1 = new Promise(resolve => setTimeout(() => resolve("First"), 1000));
let promise2 = new Promise(resolve => setTimeout(() => resolve("Second"), 2000));
let promise3 = new Promise(resolve => setTimeout(() => resolve("Third"), 1500));

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results); // ["First", "Second", "Third"]
    })
    .catch(error => {
        console.log("One of the promises failed:", error);
    });
```

### Promise.race()
```javascript
let fastPromise = new Promise(resolve => setTimeout(() => resolve("Fast"), 500));
let slowPromise = new Promise(resolve => setTimeout(() => resolve("Slow"), 2000));

Promise.race([fastPromise, slowPromise])
    .then(result => {
        console.log(result); // "Fast"
    });
```

### Promise.allSettled()
```javascript
let promise1 = Promise.resolve("Success");
let promise2 = Promise.reject("Error");
let promise3 = Promise.resolve("Another success");

Promise.allSettled([promise1, promise2, promise3])
    .then(results => {
        console.log(results);
        // [
        //   { status: 'fulfilled', value: 'Success' },
        //   { status: 'rejected', reason: 'Error' },
        //   { status: 'fulfilled', value: 'Another success' }
        // ]
    });
```

## Async/Await

### Basic Async/Await
```javascript
async function fetchData() {
    try {
        let response = await fetch("https://api.example.com/data");
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.log("Error:", error);
    }
}

fetchData();
```

### Async Function with Promise
```javascript
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function asyncFunction() {
    console.log("Start");
    await delay(1000);
    console.log("After 1 second");
    await delay(1000);
    console.log("After 2 seconds");
    return "Completed";
}

asyncFunction()
    .then(result => {
        console.log(result); // "Completed"
    });
```

### Error Handling with Async/Await
```javascript
async function fetchUserData(userId) {
    try {
        let user = await fetchUser(userId);
        let posts = await fetchUserPosts(user.id);
        let comments = await fetchUserComments(user.id);
        
        return {
            user: user,
            posts: posts,
            comments: comments
        };
    } catch (error) {
        console.log("Error fetching user data:", error);
        throw error; // Re-throw if needed
    }
}
```

## Practical Examples

### Example 1: API Calls
```javascript
class ApiClient {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
    }
    
    async get(endpoint) {
        try {
            let response = await fetch(`${this.baseUrl}${endpoint}`);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return await response.json();
        } catch (error) {
            console.error("API call failed:", error);
            throw error;
        }
    }
    
    async post(endpoint, data) {
        try {
            let response = await fetch(`${this.baseUrl}${endpoint}`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(data)
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error("API call failed:", error);
            throw error;
        }
    }
}

// Usage
let api = new ApiClient("https://jsonplaceholder.typicode.com");

async function loadData() {
    try {
        let users = await api.get("/users");
        let posts = await api.get("/posts");
        
        console.log("Users:", users);
        console.log("Posts:", posts);
    } catch (error) {
        console.log("Failed to load data:", error);
    }
}

loadData();
```

### Example 2: File Upload with Progress
```javascript
class FileUploader {
    constructor() {
        this.uploadProgress = 0;
    }
    
    async uploadFile(file, onProgress) {
        return new Promise((resolve, reject) => {
            let xhr = new XMLHttpRequest();
            
            xhr.upload.addEventListener("progress", (event) => {
                if (event.lengthComputable) {
                    this.uploadProgress = (event.loaded / event.total) * 100;
                    onProgress(this.uploadProgress);
                }
            });
            
            xhr.addEventListener("load", () => {
                if (xhr.status === 200) {
                    resolve(xhr.response);
                } else {
                    reject(new Error(`Upload failed: ${xhr.status}`));
                }
            });
            
            xhr.addEventListener("error", () => {
                reject(new Error("Upload failed"));
            });
            
            let formData = new FormData();
            formData.append("file", file);
            
            xhr.open("POST", "/upload");
            xhr.send(formData);
        });
    }
}

// Usage
let uploader = new FileUploader();
let fileInput = document.getElementById("fileInput");

fileInput.addEventListener("change", async (event) => {
    let file = event.target.files[0];
    if (file) {
        try {
            let result = await uploader.uploadFile(file, (progress) => {
                console.log(`Upload progress: ${progress}%`);
            });
            console.log("Upload completed:", result);
        } catch (error) {
            console.log("Upload failed:", error);
        }
    }
});
```

### Example 3: Sequential vs Parallel Operations
```javascript
// Sequential operations (slower)
async function sequentialOperations() {
    console.time("Sequential");
    
    let result1 = await fetchData1(); // 1 second
    let result2 = await fetchData2(); // 1 second
    let result3 = await fetchData3(); // 1 second
    
    console.timeEnd("Sequential"); // ~3 seconds
    return [result1, result2, result3];
}

// Parallel operations (faster)
async function parallelOperations() {
    console.time("Parallel");
    
    let [result1, result2, result3] = await Promise.all([
        fetchData1(), // 1 second
        fetchData2(), // 1 second
        fetchData3()  // 1 second
    ]);
    
    console.timeEnd("Parallel"); // ~1 second
    return [result1, result2, result3];
}

// Mixed approach
async function mixedOperations() {
    // Start all operations
    let promise1 = fetchData1();
    let promise2 = fetchData2();
    
    // Wait for first two
    let [result1, result2] = await Promise.all([promise1, promise2]);
    
    // Use results for third operation
    let result3 = await fetchData3(result1, result2);
    
    return [result1, result2, result3];
}
```

## Advanced Promise Patterns

### Promise Retry Pattern
```javascript
async function retryOperation(operation, maxRetries = 3, delay = 1000) {
    for (let i = 0; i < maxRetries; i++) {
        try {
            return await operation();
        } catch (error) {
            if (i === maxRetries - 1) {
                throw error;
            }
            console.log(`Attempt ${i + 1} failed, retrying in ${delay}ms...`);
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}

// Usage
async function unreliableOperation() {
    if (Math.random() > 0.5) {
        throw new Error("Random failure");
    }
    return "Success!";
}

retryOperation(unreliableOperation, 3, 1000)
    .then(result => console.log(result))
    .catch(error => console.log("All retries failed:", error));
```

### Promise Timeout Pattern
```javascript
function withTimeout(promise, timeoutMs) {
    return Promise.race([
        promise,
        new Promise((_, reject) => {
            setTimeout(() => reject(new Error("Operation timed out")), timeoutMs);
        })
    ]);
}

// Usage
async function slowOperation() {
    await new Promise(resolve => setTimeout(resolve, 2000));
    return "Slow operation completed";
}

withTimeout(slowOperation(), 1000)
    .then(result => console.log(result))
    .catch(error => console.log("Error:", error.message));
```

### Promise Queue Pattern
```javascript
class PromiseQueue {
    constructor(concurrency = 1) {
        this.concurrency = concurrency;
        this.running = 0;
        this.queue = [];
    }
    
    async add(promiseFactory) {
        return new Promise((resolve, reject) => {
            this.queue.push({
                promiseFactory,
                resolve,
                reject
            });
            this.process();
        });
    }
    
    async process() {
        if (this.running >= this.concurrency || this.queue.length === 0) {
            return;
        }
        
        this.running++;
        const { promiseFactory, resolve, reject } = this.queue.shift();
        
        try {
            const result = await promiseFactory();
            resolve(result);
        } catch (error) {
            reject(error);
        } finally {
            this.running--;
            this.process();
        }
    }
}

// Usage
let queue = new PromiseQueue(2); // Max 2 concurrent operations

async function createTask(id, delay) {
    return () => new Promise(resolve => {
        setTimeout(() => {
            console.log(`Task ${id} completed`);
            resolve(id);
        }, delay);
    });
}

// Add tasks to queue
for (let i = 0; i < 5; i++) {
    queue.add(createTask(i, 1000));
}
```

## Practice Exercises

### Exercise 1: Promise Chain
Create a chain of promises that:
1. Fetches user data
2. Fetches user posts
3. Fetches post comments
4. Combines all data

### Exercise 2: Error Handling
Create a robust API client with:
- Retry logic
- Timeout handling
- Error recovery
- Logging

### Exercise 3: Parallel Processing
Create a function that:
- Fetches data from multiple APIs
- Processes data in parallel
- Handles partial failures
- Returns combined results

### Exercise 4: Async Queue
Implement an async queue that:
- Processes tasks sequentially
- Handles errors gracefully
- Provides progress updates
- Supports cancellation

### Exercise 5: Promise Utilities
Create utility functions for:
- Promise retry
- Promise timeout
- Promise debounce
- Promise throttle

## Common Mistakes

### 1. Forgetting to Handle Errors
```javascript
// Wrong - unhandled promise rejection
async function fetchData() {
    let response = await fetch("/api/data");
    let data = await response.json();
    return data;
}

// Correct - handle errors
async function fetchData() {
    try {
        let response = await fetch("/api/data");
        let data = await response.json();
        return data;
    } catch (error) {
        console.log("Error fetching data:", error);
        throw error;
    }
}
```

### 2. Not Awaiting Promises
```javascript
// Wrong - returns Promise, not value
async function getData() {
    let data = fetch("/api/data"); // Missing await
    return data;
}

// Correct - await the promise
async function getData() {
    let response = await fetch("/api/data");
    let data = await response.json();
    return data;
}
```

### 3. Mixing Promises and Async/Await
```javascript
// Inconsistent - mixing patterns
async function mixedFunction() {
    let data = await fetchData();
    return data.then(result => {
        return processData(result);
    });
}

// Better - consistent async/await
async function consistentFunction() {
    let data = await fetchData();
    let processed = await processData(data);
    return processed;
}
```

## Key Takeaways

- Promises provide a better way to handle asynchronous operations
- Use `async/await` for cleaner, more readable code
- Always handle errors with try/catch or .catch()
- Use Promise.all() for parallel operations
- Use Promise.race() for timeout scenarios
- Practice with real-world examples
- Understand the difference between sequential and parallel execution

## Next Steps

In the next lesson, we'll learn about:
- Error handling strategies
- Try/catch blocks
- Custom error types
- Error boundaries
- Logging and debugging

---

**Ready to handle errors like a pro? Let's learn about Error Handling! 🚀**
