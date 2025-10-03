# 27 - Todo App Project

## Project Overview

Build a complete Todo application with the following features:
- Add new todos
- Mark todos as complete/incomplete
- Delete todos
- Filter todos (All, Active, Completed)
- Local storage persistence
- Responsive design

## HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>My Todo App</h1>
        </header>
        
        <div class="todo-input">
            <input type="text" id="todoInput" placeholder="Add a new todo...">
            <button id="addBtn">Add</button>
        </div>
        
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>
        
        <ul id="todoList" class="todo-list"></ul>
        
        <div class="todo-stats">
            <span id="todoCount">0 items left</span>
            <button id="clearCompleted">Clear Completed</button>
        </div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
```

## CSS Styles

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
}

.container {
    max-width: 600px;
    margin: 0 auto;
    background: white;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    overflow: hidden;
}

header {
    background: #4a90e2;
    color: white;
    padding: 20px;
    text-align: center;
}

header h1 {
    font-size: 2rem;
    font-weight: 300;
}

.todo-input {
    padding: 20px;
    display: flex;
    gap: 10px;
}

#todoInput {
    flex: 1;
    padding: 12px;
    border: 2px solid #e1e1e1;
    border-radius: 5px;
    font-size: 16px;
    outline: none;
    transition: border-color 0.3s;
}

#todoInput:focus {
    border-color: #4a90e2;
}

#addBtn {
    padding: 12px 24px;
    background: #4a90e2;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s;
}

#addBtn:hover {
    background: #357abd;
}

.filters {
    padding: 0 20px;
    display: flex;
    gap: 10px;
    border-bottom: 1px solid #e1e1e1;
}

.filter-btn {
    padding: 10px 20px;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 14px;
    color: #666;
    transition: all 0.3s;
    border-bottom: 2px solid transparent;
}

.filter-btn:hover {
    color: #4a90e2;
}

.filter-btn.active {
    color: #4a90e2;
    border-bottom-color: #4a90e2;
}

.todo-list {
    list-style: none;
    max-height: 400px;
    overflow-y: auto;
}

.todo-item {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    border-bottom: 1px solid #e1e1e1;
    transition: background-color 0.3s;
}

.todo-item:hover {
    background-color: #f8f9fa;
}

.todo-item.completed {
    opacity: 0.6;
}

.todo-item.completed .todo-text {
    text-decoration: line-through;
    color: #999;
}

.todo-checkbox {
    margin-right: 15px;
    width: 20px;
    height: 20px;
    cursor: pointer;
}

.todo-text {
    flex: 1;
    font-size: 16px;
    color: #333;
}

.todo-actions {
    display: flex;
    gap: 10px;
}

.edit-btn, .delete-btn {
    padding: 5px 10px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    font-size: 12px;
    transition: background-color 0.3s;
}

.edit-btn {
    background: #f39c12;
    color: white;
}

.edit-btn:hover {
    background: #e67e22;
}

.delete-btn {
    background: #e74c3c;
    color: white;
}

.delete-btn:hover {
    background: #c0392b;
}

.todo-stats {
    padding: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #f8f9fa;
    color: #666;
    font-size: 14px;
}

#clearCompleted {
    background: #e74c3c;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 3px;
    cursor: pointer;
    font-size: 12px;
    transition: background-color 0.3s;
}

#clearCompleted:hover {
    background: #c0392b;
}

.empty-state {
    text-align: center;
    padding: 40px 20px;
    color: #999;
}

.empty-state h3 {
    margin-bottom: 10px;
    color: #666;
}

@media (max-width: 600px) {
    .container {
        margin: 10px;
        border-radius: 5px;
    }
    
    .todo-input {
        flex-direction: column;
    }
    
    .filters {
        flex-wrap: wrap;
    }
    
    .todo-stats {
        flex-direction: column;
        gap: 10px;
    }
}
```

## JavaScript Implementation

```javascript
class TodoApp {
    constructor() {
        this.todos = this.loadTodos();
        this.currentFilter = 'all';
        this.init();
    }

    init() {
        this.bindEvents();
        this.render();
    }

    bindEvents() {
        // Add todo
        document.getElementById('addBtn').addEventListener('click', () => this.addTodo());
        document.getElementById('todoInput').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') this.addTodo();
        });

        // Filters
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.addEventListener('click', (e) => this.setFilter(e.target.dataset.filter));
        });

        // Clear completed
        document.getElementById('clearCompleted').addEventListener('click', () => this.clearCompleted());
    }

    addTodo() {
        const input = document.getElementById('todoInput');
        const text = input.value.trim();
        
        if (text === '') return;

        const todo = {
            id: Date.now(),
            text: text,
            completed: false,
            createdAt: new Date()
        };

        this.todos.unshift(todo);
        this.saveTodos();
        this.render();
        input.value = '';
    }

    toggleTodo(id) {
        const todo = this.todos.find(t => t.id === id);
        if (todo) {
            todo.completed = !todo.completed;
            this.saveTodos();
            this.render();
        }
    }

    deleteTodo(id) {
        this.todos = this.todos.filter(t => t.id !== id);
        this.saveTodos();
        this.render();
    }

    editTodo(id, newText) {
        const todo = this.todos.find(t => t.id === id);
        if (todo && newText.trim() !== '') {
            todo.text = newText.trim();
            this.saveTodos();
            this.render();
        }
    }

    setFilter(filter) {
        this.currentFilter = filter;
        
        // Update filter buttons
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.classList.remove('active');
        });
        document.querySelector(`[data-filter="${filter}"]`).classList.add('active');
        
        this.render();
    }

    getFilteredTodos() {
        switch (this.currentFilter) {
            case 'active':
                return this.todos.filter(todo => !todo.completed);
            case 'completed':
                return this.todos.filter(todo => todo.completed);
            default:
                return this.todos;
        }
    }

    clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed);
        this.saveTodos();
        this.render();
    }

    render() {
        const todoList = document.getElementById('todoList');
        const filteredTodos = this.getFilteredTodos();
        
        if (filteredTodos.length === 0) {
            todoList.innerHTML = `
                <div class="empty-state">
                    <h3>No todos found</h3>
                    <p>${this.currentFilter === 'all' ? 'Add a new todo to get started!' : `No ${this.currentFilter} todos.`}</p>
                </div>
            `;
        } else {
            todoList.innerHTML = filteredTodos.map(todo => this.createTodoHTML(todo)).join('');
        }

        this.updateStats();
        this.bindTodoEvents();
    }

    createTodoHTML(todo) {
        return `
            <li class="todo-item ${todo.completed ? 'completed' : ''}" data-id="${todo.id}">
                <input type="checkbox" class="todo-checkbox" ${todo.completed ? 'checked' : ''}>
                <span class="todo-text">${this.escapeHtml(todo.text)}</span>
                <div class="todo-actions">
                    <button class="edit-btn">Edit</button>
                    <button class="delete-btn">Delete</button>
                </div>
            </li>
        `;
    }

    bindTodoEvents() {
        // Toggle completion
        document.querySelectorAll('.todo-checkbox').forEach(checkbox => {
            checkbox.addEventListener('change', (e) => {
                const todoId = parseInt(e.target.closest('.todo-item').dataset.id);
                this.toggleTodo(todoId);
            });
        });

        // Delete todo
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const todoId = parseInt(e.target.closest('.todo-item').dataset.id);
                this.deleteTodo(todoId);
            });
        });

        // Edit todo
        document.querySelectorAll('.edit-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const todoItem = e.target.closest('.todo-item');
                const todoId = parseInt(todoItem.dataset.id);
                const todoText = todoItem.querySelector('.todo-text');
                const currentText = todoText.textContent;
                
                const newText = prompt('Edit todo:', currentText);
                if (newText !== null) {
                    this.editTodo(todoId, newText);
                }
            });
        });
    }

    updateStats() {
        const totalTodos = this.todos.length;
        const activeTodos = this.todos.filter(todo => !todo.completed).length;
        const completedTodos = this.todos.filter(todo => todo.completed).length;
        
        document.getElementById('todoCount').textContent = `${activeTodos} item${activeTodos !== 1 ? 's' : ''} left`;
        
        const clearBtn = document.getElementById('clearCompleted');
        clearBtn.style.display = completedTodos > 0 ? 'block' : 'none';
    }

    saveTodos() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
    }

    loadTodos() {
        const saved = localStorage.getItem('todos');
        return saved ? JSON.parse(saved) : [];
    }

    escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }
}

// Initialize the app when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    new TodoApp();
});
```

## Features Implemented

### Core Features
- ✅ Add new todos
- ✅ Mark todos as complete/incomplete
- ✅ Delete todos
- ✅ Edit todos
- ✅ Filter todos (All, Active, Completed)
- ✅ Clear completed todos
- ✅ Local storage persistence
- ✅ Responsive design

### Advanced Features
- ✅ Real-time stats (items left)
- ✅ Keyboard shortcuts (Enter to add)
- ✅ Empty state messages
- ✅ Smooth animations
- ✅ Mobile-friendly design
- ✅ XSS protection with HTML escaping

## How to Use

1. **Adding Todos**: Type in the input field and click "Add" or press Enter
2. **Completing Todos**: Click the checkbox next to a todo
3. **Editing Todos**: Click the "Edit" button to modify a todo
4. **Deleting Todos**: Click the "Delete" button to remove a todo
5. **Filtering**: Use the filter buttons to view All, Active, or Completed todos
6. **Clearing**: Click "Clear Completed" to remove all completed todos

## Key Learning Points

### DOM Manipulation
- Creating and modifying HTML elements
- Event handling and delegation
- Working with forms and inputs
- Dynamic content updates

### Data Management
- Local storage for persistence
- Array methods for data manipulation
- Filtering and searching data
- State management

### User Experience
- Responsive design principles
- Interactive feedback
- Keyboard accessibility
- Visual feedback for user actions

### Code Organization
- Class-based architecture
- Separation of concerns
- Event binding and cleanup
- Error handling and validation

## Extensions and Improvements

### Possible Enhancements
1. **Drag and Drop**: Reorder todos by dragging
2. **Categories**: Add categories or tags to todos
3. **Due Dates**: Set and display due dates
4. **Search**: Search through todos
5. **Themes**: Dark/light mode toggle
6. **Animations**: Smooth transitions and animations
7. **Undo/Redo**: Undo last action
8. **Export/Import**: Save and load todo lists

### Code Improvements
1. **Error Handling**: Better error handling and validation
2. **Performance**: Virtual scrolling for large lists
3. **Accessibility**: ARIA labels and keyboard navigation
4. **Testing**: Unit tests for core functionality
5. **TypeScript**: Add type safety

## Common Issues and Solutions

### Issue 1: Todos not persisting
**Solution**: Check if localStorage is available and handle errors gracefully

### Issue 2: XSS vulnerabilities
**Solution**: Always escape HTML content when displaying user input

### Issue 3: Memory leaks
**Solution**: Remove event listeners when elements are deleted

### Issue 4: Performance with many todos
**Solution**: Implement virtual scrolling or pagination

## Next Steps

1. **Test the application** thoroughly
2. **Add error handling** for edge cases
3. **Implement additional features** from the enhancement list
4. **Optimize performance** for large datasets
5. **Add accessibility features** for better UX
6. **Write tests** to ensure reliability

---

**Congratulations! You've built a complete Todo application! 🎉**

This project demonstrates many important JavaScript concepts including DOM manipulation, event handling, local storage, and modern ES6+ features. Use this as a foundation to build more complex applications!
