# 26 - Performance Optimization

Performance optimization is crucial for creating fast, responsive JavaScript applications. This lesson covers various techniques and strategies to improve JavaScript performance.

## 📚 Table of Contents
- [Performance Measurement](#performance-measurement)
- [Memory Management](#memory-management)
- [DOM Optimization](#dom-optimization)
- [Event Optimization](#event-optimization)
- [Async Performance](#async-performance)
- [Algorithm Optimization](#algorithm-optimization)
- [Caching Strategies](#caching-strategies)
- [Bundle Optimization](#bundle-optimization)
- [Runtime Optimization](#runtime-optimization)
- [Practical Examples](#practical-examples)
- [Exercises](#exercises)

## Performance Measurement

### Performance API
```javascript
// Measure execution time
function measureTime(fn, label) {
  const start = performance.now();
  const result = fn();
  const end = performance.now();
  console.log(`${label}: ${end - start} milliseconds`);
  return result;
}

// Usage
const result = measureTime(() => {
  // Some expensive operation
  let sum = 0;
  for (let i = 0; i < 1000000; i++) {
    sum += i;
  }
  return sum;
}, 'Sum calculation');

// Performance marks and measures
performance.mark('start-operation');
// ... some operation ...
performance.mark('end-operation');
performance.measure('operation-duration', 'start-operation', 'end-operation');

const measure = performance.getEntriesByName('operation-duration')[0];
console.log(`Operation took: ${measure.duration}ms`);
```

### Memory Usage
```javascript
// Check memory usage
function checkMemoryUsage() {
  if (performance.memory) {
    const memory = performance.memory;
    console.log('Memory usage:', {
      used: `${Math.round(memory.usedJSHeapSize / 1024 / 1024)} MB`,
      total: `${Math.round(memory.totalJSHeapSize / 1024 / 1024)} MB`,
      limit: `${Math.round(memory.jsHeapSizeLimit / 1024 / 1024)} MB`
    });
  }
}

// Monitor memory leaks
class MemoryMonitor {
  constructor() {
    this.initialMemory = performance.memory?.usedJSHeapSize || 0;
    this.interval = setInterval(() => this.checkMemory(), 5000);
  }
  
  checkMemory() {
    const currentMemory = performance.memory?.usedJSHeapSize || 0;
    const growth = currentMemory - this.initialMemory;
    
    if (growth > 10 * 1024 * 1024) { // 10MB growth
      console.warn('Potential memory leak detected');
    }
  }
  
  stop() {
    clearInterval(this.interval);
  }
}
```

## Memory Management

### Avoiding Memory Leaks
```javascript
// Bad - creates memory leak
function createLeak() {
  const element = document.getElementById('myElement');
  const data = new Array(1000000).fill('data');
  
  element.onclick = function() {
    // This closure keeps references to element and data
    console.log('Clicked');
  };
}

// Good - proper cleanup
function createProperHandler() {
  const element = document.getElementById('myElement');
  const data = new Array(1000000).fill('data');
  
  function handleClick() {
    console.log('Clicked');
  }
  
  element.addEventListener('click', handleClick);
  
  // Cleanup function
  return function cleanup() {
    element.removeEventListener('click', handleClick);
    // Clear references
    element = null;
    data = null;
  };
}

// WeakMap for avoiding memory leaks
const cache = new WeakMap();

function expensiveOperation(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  }
  
  const result = /* expensive calculation */ obj.value * 2;
  cache.set(obj, result);
  return result;
}
```

### Object Pool Pattern
```javascript
class ObjectPool {
  constructor(createFn, resetFn, initialSize = 10) {
    this.createFn = createFn;
    this.resetFn = resetFn;
    this.pool = [];
    this.active = new Set();
    
    // Pre-populate pool
    for (let i = 0; i < initialSize; i++) {
      this.pool.push(this.createFn());
    }
  }
  
  acquire() {
    let obj;
    if (this.pool.length > 0) {
      obj = this.pool.pop();
    } else {
      obj = this.createFn();
    }
    
    this.active.add(obj);
    return obj;
  }
  
  release(obj) {
    if (this.active.has(obj)) {
      this.active.delete(obj);
      this.resetFn(obj);
      this.pool.push(obj);
    }
  }
  
  getActiveCount() {
    return this.active.size;
  }
  
  getPoolSize() {
    return this.pool.length;
  }
}

// Usage
const particlePool = new ObjectPool(
  () => ({ x: 0, y: 0, vx: 0, vy: 0, life: 0 }),
  (particle) => {
    particle.x = 0;
    particle.y = 0;
    particle.vx = 0;
    particle.vy = 0;
    particle.life = 0;
  }
);

// Use particles
const particle = particlePool.acquire();
particle.x = 100;
particle.y = 100;
particle.vx = 1;
particle.vy = 1;
particle.life = 100;

// Release when done
particlePool.release(particle);
```

## DOM Optimization

### Efficient DOM Manipulation
```javascript
// Bad - multiple reflows
function badDOMUpdate() {
  const element = document.getElementById('myElement');
  element.style.width = '100px';
  element.style.height = '100px';
  element.style.backgroundColor = 'red';
  element.style.margin = '10px';
}

// Good - batch DOM updates
function goodDOMUpdate() {
  const element = document.getElementById('myElement');
  
  // Use requestAnimationFrame for batching
  requestAnimationFrame(() => {
    element.style.cssText = `
      width: 100px;
      height: 100px;
      background-color: red;
      margin: 10px;
    `;
  });
}

// Document fragment for multiple elements
function createElements(elements) {
  const fragment = document.createDocumentFragment();
  
  elements.forEach(data => {
    const div = document.createElement('div');
    div.textContent = data.text;
    div.className = data.class;
    fragment.appendChild(div);
  });
  
  document.getElementById('container').appendChild(fragment);
}
```

### Virtual Scrolling
```javascript
class VirtualScroll {
  constructor(container, itemHeight, items, renderItem) {
    this.container = container;
    this.itemHeight = itemHeight;
    this.items = items;
    this.renderItem = renderItem;
    this.visibleStart = 0;
    this.visibleEnd = 0;
    this.containerHeight = 0;
    
    this.init();
  }
  
  init() {
    this.containerHeight = this.container.clientHeight;
    this.visibleCount = Math.ceil(this.containerHeight / this.itemHeight) + 2;
    
    this.container.addEventListener('scroll', () => this.handleScroll());
    this.updateVisibleItems();
  }
  
  handleScroll() {
    const scrollTop = this.container.scrollTop;
    this.visibleStart = Math.floor(scrollTop / this.itemHeight);
    this.visibleEnd = Math.min(
      this.visibleStart + this.visibleCount,
      this.items.length
    );
    
    this.updateVisibleItems();
  }
  
  updateVisibleItems() {
    const visibleItems = this.items.slice(this.visibleStart, this.visibleEnd);
    const offsetY = this.visibleStart * this.itemHeight;
    
    this.container.innerHTML = `
      <div style="height: ${this.items.length * this.itemHeight}px; position: relative;">
        <div style="transform: translateY(${offsetY}px);">
          ${visibleItems.map(this.renderItem).join('')}
        </div>
      </div>
    `;
  }
}

// Usage
const container = document.getElementById('scroll-container');
const items = Array.from({ length: 10000 }, (_, i) => ({ id: i, text: `Item ${i}` }));

const virtualScroll = new VirtualScroll(
  container,
  50, // item height
  items,
  (item) => `<div class="item">${item.text}</div>`
);
```

## Event Optimization

### Debouncing and Throttling
```javascript
// Debounce - execute after delay, cancel if called again
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

// Throttle - execute at most once per interval
function throttle(func, interval) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= interval) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}

// Usage
const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query);
  // Perform search
}, 300);

const throttledScroll = throttle((event) => {
  console.log('Scroll position:', event.target.scrollTop);
  // Handle scroll
}, 100);

// Event delegation
document.addEventListener('click', (event) => {
  if (event.target.matches('.button')) {
    handleButtonClick(event.target);
  }
});

// Passive event listeners
element.addEventListener('scroll', handleScroll, { passive: true });
```

### Intersection Observer
```javascript
class LazyLoader {
  constructor() {
    this.observer = new IntersectionObserver(
      (entries) => this.handleIntersection(entries),
      { rootMargin: '50px' }
    );
  }
  
  observe(element) {
    this.observer.observe(element);
  }
  
  handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        this.loadContent(entry.target);
        this.observer.unobserve(entry.target);
      }
    });
  }
  
  loadContent(element) {
    const src = element.dataset.src;
    if (src) {
      element.src = src;
      element.classList.add('loaded');
    }
  }
}

// Usage
const lazyLoader = new LazyLoader();
document.querySelectorAll('img[data-src]').forEach(img => {
  lazyLoader.observe(img);
});
```

## Async Performance

### Web Workers
```javascript
// main.js
const worker = new Worker('worker.js');

worker.postMessage({ type: 'CALCULATE', data: largeArray });

worker.onmessage = (event) => {
  const { type, result } = event.data;
  if (type === 'CALCULATE_COMPLETE') {
    console.log('Calculation result:', result);
  }
};

// worker.js
self.onmessage = (event) => {
  const { type, data } = event.data;
  
  if (type === 'CALCULATE') {
    const result = performHeavyCalculation(data);
    self.postMessage({ type: 'CALCULATE_COMPLETE', result });
  }
};

function performHeavyCalculation(data) {
  // Heavy computation
  return data.reduce((sum, item) => sum + item.value, 0);
}
```

### Promise Optimization
```javascript
// Sequential vs Parallel
async function sequentialRequests(urls) {
  const results = [];
  for (const url of urls) {
    const response = await fetch(url);
    results.push(await response.json());
  }
  return results;
}

async function parallelRequests(urls) {
  const promises = urls.map(url => 
    fetch(url).then(response => response.json())
  );
  return Promise.all(promises);
}

// Promise pooling
class PromisePool {
  constructor(concurrency = 5) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }
  
  async add(promiseFactory) {
    return new Promise((resolve, reject) => {
      this.queue.push({ promiseFactory, resolve, reject });
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
```

## Algorithm Optimization

### Efficient Data Structures
```javascript
// Use Map for frequent lookups
const userCache = new Map();

function getUser(id) {
  if (userCache.has(id)) {
    return userCache.get(id);
  }
  
  const user = fetchUserFromAPI(id);
  userCache.set(id, user);
  return user;
}

// Use Set for unique values
const uniqueIds = new Set();
function addId(id) {
  uniqueIds.add(id);
}

// Efficient array operations
function findDuplicates(arr) {
  const seen = new Set();
  const duplicates = new Set();
  
  for (const item of arr) {
    if (seen.has(item)) {
      duplicates.add(item);
    } else {
      seen.add(item);
    }
  }
  
  return Array.from(duplicates);
}

// Binary search for sorted arrays
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return -1;
}
```

### Memoization
```javascript
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Usage
const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(40)); // Much faster with memoization
```

## Caching Strategies

### LRU Cache
```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (this.cache.has(key)) {
      const value = this.cache.get(key);
      // Move to end (most recently used)
      this.cache.delete(key);
      this.cache.set(key, value);
      return value;
    }
    return undefined;
  }
  
  set(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    } else if (this.cache.size >= this.capacity) {
      // Remove least recently used (first item)
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    this.cache.set(key, value);
  }
}

// Usage
const cache = new LRUCache(3);
cache.set('a', 1);
cache.set('b', 2);
cache.set('c', 3);
cache.get('a'); // Moves 'a' to end
cache.set('d', 4); // Removes 'b' (least recently used)
```

### Service Worker Caching
```javascript
// service-worker.js
const CACHE_NAME = 'app-cache-v1';
const urlsToCache = [
  '/',
  '/styles/main.css',
  '/scripts/main.js',
  '/images/logo.png'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(urlsToCache))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        if (response) {
          return response;
        }
        return fetch(event.request);
      })
  );
});
```

## Bundle Optimization

### Code Splitting
```javascript
// Dynamic imports
async function loadModule() {
  const module = await import('./heavy-module.js');
  return module.default;
}

// Route-based splitting
const routes = {
  '/home': () => import('./pages/Home.js'),
  '/about': () => import('./pages/About.js'),
  '/contact': () => import('./pages/Contact.js')
};

async function navigate(path) {
  const component = await routes[path]();
  render(component.default);
}
```

### Tree Shaking
```javascript
// Only import what you need
import { debounce, throttle } from 'lodash-es';

// Instead of
import _ from 'lodash';
```

## Runtime Optimization

### Function Optimization
```javascript
// Avoid function creation in loops
// Bad
function badLoop() {
  const elements = document.querySelectorAll('.item');
  for (let i = 0; i < elements.length; i++) {
    elements[i].addEventListener('click', function() {
      console.log('Clicked item', i);
    });
  }
}

// Good
function goodLoop() {
  const elements = document.querySelectorAll('.item');
  function handleClick(event) {
    console.log('Clicked item', event.target);
  }
  
  for (let i = 0; i < elements.length; i++) {
    elements[i].addEventListener('click', handleClick);
  }
}

// Use const and let instead of var
// Bad
for (var i = 0; i < 10; i++) {
  setTimeout(() => console.log(i), 100); // Prints 10 ten times
}

// Good
for (let i = 0; i < 10; i++) {
  setTimeout(() => console.log(i), 100); // Prints 0-9
}
```

## Practical Examples

### Example 1: Performance Monitoring
```javascript
class PerformanceMonitor {
  constructor() {
    this.metrics = new Map();
    this.observers = [];
  }
  
  startTiming(name) {
    this.metrics.set(name, performance.now());
  }
  
  endTiming(name) {
    const startTime = this.metrics.get(name);
    if (startTime) {
      const duration = performance.now() - startTime;
      this.metrics.delete(name);
      this.notifyObservers({ name, duration });
      return duration;
    }
  }
  
  addObserver(callback) {
    this.observers.push(callback);
  }
  
  notifyObservers(data) {
    this.observers.forEach(callback => callback(data));
  }
  
  getMetrics() {
    return Array.from(this.metrics.entries());
  }
}

// Usage
const monitor = new PerformanceMonitor();

monitor.addObserver(({ name, duration }) => {
  if (duration > 100) {
    console.warn(`Slow operation: ${name} took ${duration}ms`);
  }
});

monitor.startTiming('data-processing');
// ... some operation ...
const duration = monitor.endTiming('data-processing');
console.log(`Operation took ${duration}ms`);
```

### Example 2: Efficient Data Processing
```javascript
class DataProcessor {
  constructor() {
    this.cache = new Map();
    this.batchSize = 1000;
  }
  
  processData(data, processor) {
    const cacheKey = JSON.stringify(data);
    if (this.cache.has(cacheKey)) {
      return this.cache.get(cacheKey);
    }
    
    const result = this.processInBatches(data, processor);
    this.cache.set(cacheKey, result);
    return result;
  }
  
  processInBatches(data, processor) {
    const results = [];
    
    for (let i = 0; i < data.length; i += this.batchSize) {
      const batch = data.slice(i, i + this.batchSize);
      const batchResult = processor(batch);
      results.push(...batchResult);
      
      // Yield control to prevent blocking
      if (i + this.batchSize < data.length) {
        // Use setTimeout to yield control
        return new Promise(resolve => {
          setTimeout(() => {
            resolve(this.processInBatches(data.slice(i + this.batchSize), processor));
          }, 0);
        });
      }
    }
    
    return results;
  }
  
  clearCache() {
    this.cache.clear();
  }
}

// Usage
const processor = new DataProcessor();
const largeDataset = Array.from({ length: 10000 }, (_, i) => ({ id: i, value: Math.random() }));

const result = processor.processData(largeDataset, (batch) => {
  return batch.map(item => ({ ...item, processed: true }));
});
```

### Example 3: Optimized Search
```javascript
class SearchEngine {
  constructor() {
    this.index = new Map();
    this.stopWords = new Set(['the', 'a', 'an', 'and', 'or', 'but', 'in', 'on', 'at', 'to', 'for', 'of', 'with', 'by']);
  }
  
  buildIndex(documents) {
    documents.forEach((doc, index) => {
      const words = this.tokenize(doc.text);
      words.forEach(word => {
        if (!this.index.has(word)) {
          this.index.set(word, new Set());
        }
        this.index.get(word).add(index);
      });
    });
  }
  
  tokenize(text) {
    return text.toLowerCase()
      .replace(/[^\w\s]/g, '')
      .split(/\s+/)
      .filter(word => word.length > 2 && !this.stopWords.has(word));
  }
  
  search(query) {
    const queryWords = this.tokenize(query);
    const results = new Map();
    
    queryWords.forEach(word => {
      if (this.index.has(word)) {
        this.index.get(word).forEach(docIndex => {
          results.set(docIndex, (results.get(docIndex) || 0) + 1);
        });
      }
    });
    
    return Array.from(results.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([docIndex, score]) => ({ docIndex, score }));
  }
}

// Usage
const documents = [
  { text: 'JavaScript is a programming language' },
  { text: 'Python is also a programming language' },
  { text: 'JavaScript and Python are both popular' }
];

const searchEngine = new SearchEngine();
searchEngine.buildIndex(documents);

const results = searchEngine.search('JavaScript programming');
console.log(results);
```

## Exercises

### Exercise 1: Performance Profiler
Create a performance profiler that can measure and track the performance of different functions and operations.

### Exercise 2: Memory Leak Detector
Build a tool that can detect potential memory leaks in JavaScript applications.

### Exercise 3: Optimized Data Structure
Implement an optimized data structure for frequent insertions, deletions, and lookups.

### Exercise 4: Caching System
Create a sophisticated caching system with different eviction policies (LRU, LFU, TTL).

### Exercise 5: Performance Dashboard
Build a real-time performance dashboard that monitors and displays various performance metrics.

## Key Takeaways

- Measure performance before and after optimizations
- Use appropriate data structures for your use case
- Minimize DOM manipulations and reflows
- Implement proper memory management to avoid leaks
- Use debouncing and throttling for event handlers
- Leverage Web Workers for CPU-intensive tasks
- Implement caching strategies for expensive operations
- Use code splitting and lazy loading for better initial load times
- Profile your application to identify bottlenecks
- Consider the trade-offs between optimization and code complexity

Performance optimization is an ongoing process that requires monitoring, profiling, and iterative improvements to achieve the best results.
