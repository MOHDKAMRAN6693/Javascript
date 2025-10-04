# JavaScript Interview Questions - Intermediate Level

## Async Programming

### 1. What is asynchronous programming in JavaScript?

**Answer:**
Asynchronous programming allows JavaScript to execute code without blocking the main thread. This is essential for handling operations that take time (like API calls, file reading, etc.) without freezing the user interface.

**Synchronous vs Asynchronous:**
```javascript
// Synchronous - blocks execution
console.log('Start');
for (let i = 0; i < 1000000000; i++) {
  // Heavy computation
}
console.log('End'); // This won't run until the loop finishes

// Asynchronous - non-blocking
console.log('Start');
setTimeout(() => {
  console.log('Async operation');
}, 1000);
console.log('End'); // This runs immediately
```

**Event Loop Concept:**
```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

// Output: 1, 4, 3, 2
// Explanation: Synchronous code runs first, then microtasks (Promises), then macrotasks (setTimeout)
```

### 2. What are callbacks and what are their problems?

**Answer:**
Callbacks are functions passed as arguments to other functions, to be executed later when an operation completes.

**Basic Callback Example:**
```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: 'John', age: 30 };
    callback(data);
  }, 1000);
}

fetchData((data) => {
  console.log('Received:', data);
});
```

**Callback Problems (Callback Hell):**
```javascript
// Nested callbacks become hard to read and maintain
getUser(1, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getLikes(comments[0].id, (likes) => {
        console.log('Final result:', likes);
        // This becomes deeply nested and hard to manage
      });
    });
  });
});

// Error handling becomes complex
getUser(1, (err, user) => {
  if (err) {
    console.error('Error getting user:', err);
    return;
  }
  
  getPosts(user.id, (err, posts) => {
    if (err) {
      console.error('Error getting posts:', err);
      return;
    }
    
    // More nested error handling...
  });
});
```

### 3. What are Promises and how do they work?

**Answer:**
Promises are objects that represent the eventual completion or failure of an asynchronous operation. They provide a cleaner way to handle async operations.

**Promise States:**
- **Pending**: Initial state, neither fulfilled nor rejected
- **Fulfilled**: Operation completed successfully
- **Rejected**: Operation failed

**Basic Promise Usage:**
```javascript
// Creating a Promise
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = Math.random() > 0.5;
    if (success) {
      resolve('Operation successful');
    } else {
      reject('Operation failed');
    }
  }, 1000);
});

// Using the Promise
promise
  .then(result => {
    console.log('Success:', result);
  })
  .catch(error => {
    console.error('Error:', error);
  })
  .finally(() => {
    console.log('Operation completed');
  });
```

**Promise Methods:**
```javascript
// Promise.all() - waits for all promises to resolve
const promises = [
  fetch('/api/user/1'),
  fetch('/api/user/2'),
  fetch('/api/user/3')
];

Promise.all(promises)
  .then(responses => {
    console.log('All users fetched:', responses);
  })
  .catch(error => {
    console.error('One or more requests failed:', error);
  });

// Promise.allSettled() - waits for all promises to settle (resolve or reject)
Promise.allSettled(promises)
  .then(results => {
    results.forEach((result, index) => {
      if (result.status === 'fulfilled') {
        console.log(`Promise ${index} succeeded:`, result.value);
      } else {
        console.log(`Promise ${index} failed:`, result.reason);
      }
    });
  });

// Promise.race() - returns the first promise to settle
Promise.race(promises)
  .then(result => {
    console.log('First promise resolved:', result);
  });
```

### 4. What is async/await and how does it work?

**Answer:**
`async/await` is syntactic sugar over Promises that makes asynchronous code look and behave more like synchronous code.

**Basic async/await:**
```javascript
// Async function always returns a Promise
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    const user = await response.json();
  return user;
  } catch (error) {
    console.error('Error fetching user:', error);
    throw error;
  }
}

// Using async/await
async function displayUser() {
  try {
    const user = await fetchUser(1);
    console.log('User:', user);
  } catch (error) {
    console.error('Failed to display user:', error);
  }
}
```

**Parallel Execution:**
```javascript
// Sequential (slower)
async function sequentialFetch() {
  const user1 = await fetchUser(1);
  const user2 = await fetchUser(2);
  const user3 = await fetchUser(3);
  return [user1, user2, user3];
}

// Parallel (faster)
async function parallelFetch() {
  const [user1, user2, user3] = await Promise.all([
    fetchUser(1),
    fetchUser(2),
    fetchUser(3)
  ]);
  return [user1, user2, user3];
}
```

**Error Handling:**
```javascript
async function handleErrors() {
  try {
    const user = await fetchUser(1);
    const posts = await fetchPosts(user.id);
    return { user, posts };
  } catch (error) {
    if (error.name === 'NetworkError') {
      console.error('Network error:', error.message);
    } else if (error.name === 'ValidationError') {
      console.error('Validation error:', error.message);
    } else {
      console.error('Unexpected error:', error);
    }
    throw error; // Re-throw if needed
  }
}
```

### 5. What is the difference between `setTimeout` and `setInterval`?

**Answer:**
- **`setTimeout`**: Executes a function once after a specified delay
- **`setInterval`**: Executes a function repeatedly at specified intervals

**setTimeout Examples:**
```javascript
// Basic setTimeout
setTimeout(() => {
  console.log('This runs after 1 second');
}, 1000);

// setTimeout with parameters
setTimeout((message, count) => {
  console.log(message, count);
}, 1000, 'Hello', 42);

// Clearing setTimeout
const timeoutId = setTimeout(() => {
  console.log('This will not run');
}, 1000);

clearTimeout(timeoutId); // Cancels the timeout
```

**setInterval Examples:**
```javascript
// Basic setInterval
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log(`Count: ${count}`);
  
  if (count >= 5) {
    clearInterval(intervalId); // Stop after 5 iterations
  }
}, 1000);

// Clock example
function startClock() {
  const intervalId = setInterval(() => {
    const now = new Date();
    console.log(now.toLocaleTimeString());
  }, 1000);
  
  // Return function to stop the clock
  return () => clearInterval(intervalId);
}

const stopClock = startClock();
// Call stopClock() to stop the clock
```

### 6. What are Web Workers and when would you use them?

**Answer:**
Web Workers allow you to run JavaScript code in background threads, separate from the main UI thread. This prevents blocking the UI during heavy computations.

**Basic Web Worker:**
```javascript
// main.js
const worker = new Worker('worker.js');

// Send message to worker
worker.postMessage({ type: 'CALCULATE', data: [1, 2, 3, 4, 5] });

// Listen for messages from worker
worker.onmessage = (event) => {
  console.log('Result from worker:', event.data);
};

// Handle errors
worker.onerror = (error) => {
  console.error('Worker error:', error);
};

// Terminate worker when done
worker.terminate();
```

```javascript
// worker.js
self.onmessage = (event) => {
  const { type, data } = event.data;
  
  if (type === 'CALCULATE') {
    // Heavy computation
    const result = data.reduce((sum, num) => sum + num * num, 0);
    
    // Send result back to main thread
    self.postMessage({ type: 'RESULT', result });
  }
};
```

**When to Use Web Workers:**
- Heavy mathematical calculations
- Image processing
- Data parsing/transformation
- Any CPU-intensive task that might block the UI

### 7. What is the Event Loop and how does it work?

**Answer:**
The Event Loop is JavaScript's mechanism for handling asynchronous operations. It continuously checks the call stack and task queues to execute code.

**Event Loop Components:**
1. **Call Stack**: Where synchronous code is executed
2. **Web APIs**: Browser APIs (setTimeout, DOM events, etc.
3. **Task Queue**: Where asynchronous callbacks wait
4. **Microtask Queue**: Higher priority than task queue

**Event Loop Process:**
```javascript
console.log('1'); // Synchronous - goes to call stack

setTimeout(() => console.log('2'), 0); // Goes to Web API, then task queue

Promise.resolve().then(() => console.log('3')); // Goes to microtask queue

console.log('4'); // Synchronous - goes to call stack

// Execution order:
// 1. Call stack: 1, 4
// 2. Microtask queue: 3
// 3. Task queue: 2
// Output: 1, 4, 3, 2
```

**Priority Order:**
1. Call stack (synchronous code)
2. Microtask queue (Promises, queueMicrotask)
3. Task queue (setTimeout, setInterval, DOM events)

### 8. What are the different ways to handle errors in async code?

**Answer:**
There are several ways to handle errors in asynchronous JavaScript:

**1. Try-Catch with async/await:**
```javascript
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching data:', error);
    throw error; // Re-throw if needed
  }
}
```

**2. Promise error handling:**
```javascript
function fetchData() {
  return fetch('/api/data')
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json();
    })
    .catch(error => {
      console.error('Error fetching data:', error);
      throw error;
    });
}
```

**3. Global error handling:**
```javascript
// Unhandled promise rejections
window.addEventListener('unhandledrejection', (event) => {
  console.error('Unhandled promise rejection:', event.reason);
  event.preventDefault(); // Prevent default browser behavior
});

// Global error handler
window.addEventListener('error', (event) => {
  console.error('Global error:', event.error);
});
```

**4. Error boundaries in async operations:**
```javascript
async function safeAsyncOperation() {
  try {
    const result = await riskyOperation();
    return { success: true, data: result };
  } catch (error) {
    return { success: false, error: error.message };
  }
}

// Usage
const result = await safeAsyncOperation();
if (result.success) {
  console.log('Operation succeeded:', result.data);
} else {
  console.error('Operation failed:', result.error);
}
```

### 9. What is the difference between `Promise.all()` and `Promise.allSettled()`?

**Answer:**
Both methods handle multiple promises, but they behave differently when some promises fail:

**Promise.all():**
- Fails fast - if any promise rejects, the entire operation fails
- Returns array of resolved values if all succeed
- Throws error if any promise rejects

**Promise.allSettled():**
- Waits for all promises to complete (resolve or reject)
- Always returns array of results with status information
- Never throws errors

**Examples:**
```javascript
const promises = [
  fetch('/api/user/1'),
  fetch('/api/user/2'),
  fetch('/api/user/999'), // This might fail
  fetch('/api/user/4')
];

// Promise.all() - fails if any promise rejects
Promise.all(promises)
  .then(responses => {
    console.log('All users fetched successfully');
    // Process all responses
  })
  .catch(error => {
    console.error('One or more requests failed:', error);
    // If user/999 fails, this catches the error
  });

// Promise.allSettled() - waits for all to complete
Promise.allSettled(promises)
  .then(results => {
    results.forEach((result, index) => {
      if (result.status === 'fulfilled') {
        console.log(`User ${index + 1} fetched:`, result.value);
      } else {
        console.log(`User ${index + 1} failed:`, result.reason);
      }
    });
  });
```

### 10. What are async iterators and generators?

**Answer:**
Async iterators and generators allow you to work with asynchronous data streams in a more elegant way.

**Async Generators:**
```javascript
async function* fetchUsers() {
  const userIds = [1, 2, 3, 4, 5];
  
  for (const id of userIds) {
    try {
      const response = await fetch(`/api/users/${id}`);
      const user = await response.json();
      yield user;
    } catch (error) {
      console.error(`Failed to fetch user ${id}:`, error);
    }
  }
}

// Using async generator
async function processUsers() {
  for await (const user of fetchUsers()) {
    console.log('Processing user:', user.name);
    // Process each user as it arrives
  }
}
```

**Async Iterators:**
```javascript
class AsyncDataStream {
  constructor(data) {
    this.data = data;
    this.index = 0;
  }
  
  async *[Symbol.asyncIterator]() {
    while (this.index < this.data.length) {
      // Simulate async operation
      await new Promise(resolve => setTimeout(resolve, 100));
      yield this.data[this.index++];
    }
  }
}

// Usage
const stream = new AsyncDataStream([1, 2, 3, 4, 5]);

for await (const value of stream) {
  console.log('Received:', value);
}
```

## Practice Questions

1. **What will be the output?**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
```

2. **Explain the output:**
```javascript
async function test() {
  console.log('A');
  await Promise.resolve();
  console.log('B');
}
test();
console.log('C');
```

3. **What's the difference between these two?**
```javascript
// Version 1
async function fetchData() {
  const user = await fetchUser();
  const posts = await fetchPosts(user.id);
  return { user, posts };
}

// Version 2
async function fetchData() {
  const [user, posts] = await Promise.all([
    fetchUser(),
    fetchPosts()
  ]);
  return { user, posts };
}
```

4. **Predict the output:**
```javascript
const promise = new Promise((resolve) => {
  console.log('1');
  resolve();
});

promise.then(() => console.log('2'));
console.log('3');
```

5. **What will this return?**
```javascript
async function test() {
  try {
    await Promise.reject('Error');
  } catch (error) {
    return 'Caught';
  }
  return 'Success';
}
test().then(console.log);
```

## Key Takeaways

- Understand the difference between synchronous and asynchronous code
- Master Promises and async/await syntax
- Know when to use parallel vs sequential execution
- Understand the Event Loop and execution order
- Learn proper error handling in async code
- Know the differences between Promise methods
- Understand when to use Web Workers
- Practice with async iterators and generators
