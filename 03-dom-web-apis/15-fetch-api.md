# 15 - Fetch API

## What is the Fetch API?

The Fetch API is a modern JavaScript interface for making HTTP requests. It provides a more powerful and flexible way to fetch resources compared to the older XMLHttpRequest.

## Basic Fetch Usage

### Simple GET Request
```javascript
// Basic fetch request
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

### Async/Await with Fetch
```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error);
    }
}

fetchData();
```

## Fetch Options

### GET Request with Headers
```javascript
fetch('https://api.example.com/data', {
    method: 'GET',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer your-token-here'
    }
})
.then(response => response.json())
.then(data => console.log(data));
```

### POST Request with Data
```javascript
const userData = {
    name: 'John Doe',
    email: 'john@example.com'
};

fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(userData)
})
.then(response => response.json())
.then(data => console.log(data));
```

### PUT Request
```javascript
const updatedData = {
    name: 'Jane Doe',
    email: 'jane@example.com'
};

fetch('https://api.example.com/users/123', {
    method: 'PUT',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(updatedData)
})
.then(response => response.json())
.then(data => console.log(data));
```

### DELETE Request
```javascript
fetch('https://api.example.com/users/123', {
    method: 'DELETE',
    headers: {
        'Authorization': 'Bearer your-token-here'
    }
})
.then(response => {
    if (response.ok) {
        console.log('User deleted successfully');
    }
});
```

## Response Handling

### Checking Response Status
```javascript
async function fetchUser(id) {
    try {
        const response = await fetch(`https://api.example.com/users/${id}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const user = await response.json();
        return user;
    } catch (error) {
        console.error('Error fetching user:', error);
        throw error;
    }
}
```

### Different Response Types
```javascript
async function handleDifferentResponses() {
    const response = await fetch('https://api.example.com/data');
    
    // Check content type
    const contentType = response.headers.get('content-type');
    
    if (contentType.includes('application/json')) {
        const data = await response.json();
        console.log('JSON data:', data);
    } else if (contentType.includes('text/')) {
        const text = await response.text();
        console.log('Text data:', text);
    } else if (contentType.includes('image/')) {
        const blob = await response.blob();
        console.log('Image blob:', blob);
    }
}
```

### Response Headers
```javascript
async function getResponseHeaders() {
    const response = await fetch('https://api.example.com/data');
    
    // Get specific header
    const contentType = response.headers.get('content-type');
    const contentLength = response.headers.get('content-length');
    
    // Get all headers
    const headers = {};
    response.headers.forEach((value, key) => {
        headers[key] = value;
    });
    
    console.log('Headers:', headers);
}
```

## Error Handling

### Basic Error Handling
```javascript
async function fetchWithErrorHandling(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        if (error.name === 'TypeError') {
            console.error('Network error:', error.message);
        } else {
            console.error('HTTP error:', error.message);
        }
        throw error;
    }
}
```

### Advanced Error Handling
```javascript
class ApiError extends Error {
    constructor(message, status, statusText) {
        super(message);
        this.name = 'ApiError';
        this.status = status;
        this.statusText = statusText;
    }
}

async function fetchWithAdvancedErrorHandling(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new ApiError(
                `Request failed with status ${response.status}`,
                response.status,
                response.statusText
            );
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        if (error instanceof ApiError) {
            console.error('API Error:', error.message, error.status);
        } else if (error.name === 'TypeError') {
            console.error('Network Error:', error.message);
        } else {
            console.error('Unknown Error:', error.message);
        }
        throw error;
    }
}
```

## Practical Examples

### Example 1: API Client Class
```javascript
class ApiClient {
    constructor(baseUrl, defaultHeaders = {}) {
        this.baseUrl = baseUrl;
        this.defaultHeaders = {
            'Content-Type': 'application/json',
            ...defaultHeaders
        };
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseUrl}${endpoint}`;
        const config = {
            headers: {
                ...this.defaultHeaders,
                ...options.headers
            },
            ...options
        };
        
        try {
            const response = await fetch(url, config);
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            const contentType = response.headers.get('content-type');
            if (contentType && contentType.includes('application/json')) {
                return await response.json();
            } else {
                return await response.text();
            }
        } catch (error) {
            console.error('API request failed:', error);
            throw error;
        }
    }
    
    async get(endpoint, headers = {}) {
        return this.request(endpoint, { method: 'GET', headers });
    }
    
    async post(endpoint, data, headers = {}) {
        return this.request(endpoint, {
            method: 'POST',
            body: JSON.stringify(data),
            headers
        });
    }
    
    async put(endpoint, data, headers = {}) {
        return this.request(endpoint, {
            method: 'PUT',
            body: JSON.stringify(data),
            headers
        });
    }
    
    async delete(endpoint, headers = {}) {
        return this.request(endpoint, { method: 'DELETE', headers });
    }
}

// Usage
const api = new ApiClient('https://api.example.com');

// GET request
const users = await api.get('/users');

// POST request
const newUser = await api.post('/users', {
    name: 'John Doe',
    email: 'john@example.com'
});
```

### Example 2: File Upload with Progress
```javascript
class FileUploader {
    constructor(apiClient) {
        this.apiClient = apiClient;
    }
    
    async uploadFile(file, onProgress) {
        const formData = new FormData();
        formData.append('file', file);
        
        return new Promise((resolve, reject) => {
            const xhr = new XMLHttpRequest();
            
            xhr.upload.addEventListener('progress', (event) => {
                if (event.lengthComputable) {
                    const progress = (event.loaded / event.total) * 100;
                    onProgress(progress);
                }
            });
            
            xhr.addEventListener('load', () => {
                if (xhr.status === 200) {
                    resolve(JSON.parse(xhr.response));
                } else {
                    reject(new Error(`Upload failed: ${xhr.status}`));
                }
            });
            
            xhr.addEventListener('error', () => {
                reject(new Error('Upload failed'));
            });
            
            xhr.open('POST', `${this.apiClient.baseUrl}/upload`);
            xhr.send(formData);
        });
    }
}

// Usage
const uploader = new FileUploader(api);

document.getElementById('fileInput').addEventListener('change', async (event) => {
    const file = event.target.files[0];
    if (file) {
        try {
            const result = await uploader.uploadFile(file, (progress) => {
                console.log(`Upload progress: ${progress}%`);
            });
            console.log('Upload completed:', result);
        } catch (error) {
            console.error('Upload failed:', error);
        }
    }
});
```

### Example 3: Data Fetching with Caching
```javascript
class CachedApiClient extends ApiClient {
    constructor(baseUrl, defaultHeaders = {}) {
        super(baseUrl, defaultHeaders);
        this.cache = new Map();
        this.cacheTimeout = 5 * 60 * 1000; // 5 minutes
    }
    
    async get(endpoint, headers = {}, useCache = true) {
        const cacheKey = `${endpoint}_${JSON.stringify(headers)}`;
        
        if (useCache && this.cache.has(cacheKey)) {
            const cached = this.cache.get(cacheKey);
            if (Date.now() - cached.timestamp < this.cacheTimeout) {
                return cached.data;
            }
        }
        
        const data = await super.get(endpoint, headers);
        
        if (useCache) {
            this.cache.set(cacheKey, {
                data: data,
                timestamp: Date.now()
            });
        }
        
        return data;
    }
    
    clearCache() {
        this.cache.clear();
    }
}

// Usage
const cachedApi = new CachedApiClient('https://api.example.com');

// First call - fetches from API
const users1 = await cachedApi.get('/users');

// Second call - returns from cache
const users2 = await cachedApi.get('/users');
```

## Advanced Fetch Patterns

### Request Interceptors
```javascript
class FetchInterceptor {
    constructor() {
        this.requestInterceptors = [];
        this.responseInterceptors = [];
    }
    
    addRequestInterceptor(interceptor) {
        this.requestInterceptors.push(interceptor);
    }
    
    addResponseInterceptor(interceptor) {
        this.responseInterceptors.push(interceptor);
    }
    
    async fetch(url, options = {}) {
        // Apply request interceptors
        for (const interceptor of this.requestInterceptors) {
            const result = await interceptor(url, options);
            if (result) {
                [url, options] = result;
            }
        }
        
        const response = await fetch(url, options);
        
        // Apply response interceptors
        for (const interceptor of this.responseInterceptors) {
            const result = await interceptor(response);
            if (result) {
                return result;
            }
        }
        
        return response;
    }
}

// Usage
const interceptor = new FetchInterceptor();

// Add authentication interceptor
interceptor.addRequestInterceptor((url, options) => {
    const token = localStorage.getItem('authToken');
    if (token) {
        options.headers = {
            ...options.headers,
            'Authorization': `Bearer ${token}`
        };
    }
    return [url, options];
});

// Add error handling interceptor
interceptor.addResponseInterceptor(async (response) => {
    if (!response.ok) {
        if (response.status === 401) {
            // Handle unauthorized
            localStorage.removeItem('authToken');
            window.location.href = '/login';
        }
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response;
});
```

### Request Retry Logic
```javascript
async function fetchWithRetry(url, options = {}, maxRetries = 3) {
    let lastError;
    
    for (let i = 0; i < maxRetries; i++) {
        try {
            const response = await fetch(url, options);
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            return response;
        } catch (error) {
            lastError = error;
            
            if (i < maxRetries - 1) {
                const delay = Math.pow(2, i) * 1000; // Exponential backoff
                await new Promise(resolve => setTimeout(resolve, delay));
            }
        }
    }
    
    throw lastError;
}

// Usage
try {
    const response = await fetchWithRetry('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
} catch (error) {
    console.error('All retries failed:', error);
}
```

## Practice Exercises

### Exercise 1: Weather API Client
Create a weather API client that:
- Fetches current weather data
- Handles API errors gracefully
- Caches responses
- Provides fallback data

### Exercise 2: Blog API Client
Create a blog API client that:
- Fetches blog posts
- Creates new posts
- Updates existing posts
- Deletes posts
- Handles pagination

### Exercise 3: File Upload Service
Create a file upload service that:
- Uploads files with progress tracking
- Handles different file types
- Provides upload status
- Handles upload errors

### Exercise 4: Real-time Data Fetcher
Create a real-time data fetcher that:
- Polls an API endpoint
- Handles connection errors
- Provides real-time updates
- Manages polling intervals

### Exercise 5: API Rate Limiter
Create an API rate limiter that:
- Limits request frequency
- Queues requests
- Handles rate limit errors
- Provides retry logic

## Common Mistakes

### 1. Not Handling Response Status
```javascript
// Wrong - doesn't check if request was successful
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data));

// Correct - check response status
fetch('/api/data')
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

### 2. Not Handling Network Errors
```javascript
// Wrong - only handles HTTP errors
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data));

// Correct - handle both HTTP and network errors
fetch('/api/data')
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => {
        if (error.name === 'TypeError') {
            console.error('Network error:', error.message);
        } else {
            console.error('HTTP error:', error.message);
        }
    });
```

### 3. Not Using Proper Headers
```javascript
// Wrong - missing content type
fetch('/api/data', {
    method: 'POST',
    body: JSON.stringify(data)
});

// Correct - include proper headers
fetch('/api/data', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
});
```

## Key Takeaways

- Fetch API provides a modern way to make HTTP requests
- Always handle both HTTP and network errors
- Use proper headers for different content types
- Implement retry logic for unreliable networks
- Cache responses to improve performance
- Use interceptors for common functionality
- Practice with real APIs to understand different scenarios

## Next Steps

In the next lesson, we'll learn about:
- Destructuring assignment syntax
- Array destructuring
- Object destructuring
- Function parameter destructuring
- Nested destructuring and rest/spread

---

**Ready to unpack data structures? Let's learn about Destructuring! 🚀**
