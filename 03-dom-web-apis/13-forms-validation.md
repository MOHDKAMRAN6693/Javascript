# 13 - Forms and Validation

## What are Forms?

Forms are HTML elements that allow users to input data and submit it to a server. JavaScript provides powerful tools for handling form interactions, validation, and data processing.

## Basic Form Handling

### HTML Form Structure
```html
<form id="contactForm" action="/submit" method="POST">
    <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required>
    </div>
    
    <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
    </div>
    
    <div class="form-group">
        <label for="message">Message:</label>
        <textarea id="message" name="message" rows="5" required></textarea>
    </div>
    
    <button type="submit">Submit</button>
</form>
```

### Basic Form Event Handling
```javascript
let form = document.getElementById("contactForm");

form.addEventListener("submit", function(event) {
    event.preventDefault(); // Prevent default form submission
    
    // Get form data
    let formData = new FormData(form);
    let data = Object.fromEntries(formData);
    
    console.log("Form data:", data);
    
    // Process form data
    processForm(data);
});

function processForm(data) {
    // Send data to server or process locally
    console.log("Processing:", data);
}
```

## Form Validation

### HTML5 Validation
```html
<form id="validationForm">
    <!-- Required field -->
    <input type="text" name="username" required>
    
    <!-- Email validation -->
    <input type="email" name="email" required>
    
    <!-- Pattern validation -->
    <input type="text" name="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" 
           title="Phone number format: 123-456-7890">
    
    <!-- Length validation -->
    <input type="password" name="password" minlength="8" maxlength="20">
    
    <!-- Number range validation -->
    <input type="number" name="age" min="18" max="100">
    
    <!-- URL validation -->
    <input type="url" name="website">
    
    <button type="submit">Submit</button>
</form>
```

### JavaScript Validation
```javascript
class FormValidator {
    constructor(formId) {
        this.form = document.getElementById(formId);
        this.errors = {};
        this.init();
    }
    
    init() {
        this.form.addEventListener("submit", (event) => {
            event.preventDefault();
            this.validateForm();
        });
        
        // Real-time validation
        this.form.addEventListener("input", (event) => {
            this.validateField(event.target);
        });
    }
    
    validateForm() {
        this.errors = {};
        let isValid = true;
        
        // Get all form fields
        let fields = this.form.querySelectorAll("input, textarea, select");
        
        fields.forEach(field => {
            if (!this.validateField(field)) {
                isValid = false;
            }
        });
        
        if (isValid) {
            this.submitForm();
        } else {
            this.displayErrors();
        }
    }
    
    validateField(field) {
        let value = field.value.trim();
        let fieldName = field.name;
        let isValid = true;
        let errorMessage = "";
        
        // Required field validation
        if (field.hasAttribute("required") && value === "") {
            isValid = false;
            errorMessage = `${fieldName} is required`;
        }
        
        // Email validation
        if (field.type === "email" && value !== "") {
            let emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(value)) {
                isValid = false;
                errorMessage = "Please enter a valid email address";
            }
        }
        
        // Phone validation
        if (field.name === "phone" && value !== "") {
            let phoneRegex = /^[0-9]{3}-[0-9]{3}-[0-9]{4}$/;
            if (!phoneRegex.test(value)) {
                isValid = false;
                errorMessage = "Phone number must be in format: 123-456-7890";
            }
        }
        
        // Password validation
        if (field.type === "password" && value !== "") {
            if (value.length < 8) {
                isValid = false;
                errorMessage = "Password must be at least 8 characters long";
            }
        }
        
        // Custom validation
        if (field.dataset.validation) {
            let validationResult = this.customValidation(field, value);
            if (!validationResult.isValid) {
                isValid = false;
                errorMessage = validationResult.message;
            }
        }
        
        this.showFieldError(field, isValid, errorMessage);
        return isValid;
    }
    
    customValidation(field, value) {
        let validationType = field.dataset.validation;
        
        switch (validationType) {
            case "username":
                if (value.length < 3) {
                    return { isValid: false, message: "Username must be at least 3 characters" };
                }
                if (!/^[a-zA-Z0-9_]+$/.test(value)) {
                    return { isValid: false, message: "Username can only contain letters, numbers, and underscores" };
                }
                break;
                
            case "password-strength":
                let strength = this.checkPasswordStrength(value);
                if (strength < 3) {
                    return { isValid: false, message: "Password is too weak" };
                }
                break;
                
            case "confirm-password":
                let passwordField = this.form.querySelector("input[name='password']");
                if (value !== passwordField.value) {
                    return { isValid: false, message: "Passwords do not match" };
                }
                break;
        }
        
        return { isValid: true, message: "" };
    }
    
    checkPasswordStrength(password) {
        let strength = 0;
        
        if (password.length >= 8) strength++;
        if (/[a-z]/.test(password)) strength++;
        if (/[A-Z]/.test(password)) strength++;
        if (/[0-9]/.test(password)) strength++;
        if (/[^a-zA-Z0-9]/.test(password)) strength++;
        
        return strength;
    }
    
    showFieldError(field, isValid, message) {
        let errorElement = field.parentNode.querySelector(".error-message");
        
        if (isValid) {
            field.classList.remove("error");
            if (errorElement) {
                errorElement.remove();
            }
        } else {
            field.classList.add("error");
            if (!errorElement) {
                let errorDiv = document.createElement("div");
                errorDiv.className = "error-message";
                errorDiv.textContent = message;
                field.parentNode.appendChild(errorDiv);
            } else {
                errorElement.textContent = message;
            }
        }
    }
    
    displayErrors() {
        console.log("Form validation failed:", this.errors);
    }
    
    submitForm() {
        let formData = new FormData(this.form);
        let data = Object.fromEntries(formData);
        
        console.log("Form submitted successfully:", data);
        // Send data to server
    }
}

// Initialize form validator
new FormValidator("validationForm");
```

## Form Data Handling

### FormData API
```javascript
function handleFormSubmit(event) {
    event.preventDefault();
    
    let form = event.target;
    let formData = new FormData(form);
    
    // Get individual values
    let name = formData.get("name");
    let email = formData.get("email");
    
    // Get all data as object
    let data = Object.fromEntries(formData);
    
    // Get all values for a specific field
    let hobbies = formData.getAll("hobbies");
    
    // Check if field exists
    if (formData.has("newsletter")) {
        console.log("User subscribed to newsletter");
    }
    
    // Append additional data
    formData.append("timestamp", Date.now());
    formData.append("source", "website");
    
    // Send to server
    sendToServer(formData);
}

async function sendToServer(formData) {
    try {
        let response = await fetch("/api/submit", {
            method: "POST",
            body: formData
        });
        
        if (response.ok) {
            console.log("Form submitted successfully");
        } else {
            console.log("Form submission failed");
        }
    } catch (error) {
        console.error("Error submitting form:", error);
    }
}
```

### Form Serialization
```javascript
function serializeForm(form) {
    let formData = new FormData(form);
    let data = {};
    
    for (let [key, value] of formData.entries()) {
        if (data[key]) {
            // Handle multiple values (checkboxes, etc.)
            if (Array.isArray(data[key])) {
                data[key].push(value);
            } else {
                data[key] = [data[key], value];
            }
        } else {
            data[key] = value;
        }
    }
    
    return data;
}

// Usage
let form = document.getElementById("myForm");
let data = serializeForm(form);
console.log(data);
```

## Advanced Form Features

### File Upload
```javascript
class FileUploader {
    constructor(formId) {
        this.form = document.getElementById(formId);
        this.maxFileSize = 5 * 1024 * 1024; // 5MB
        this.allowedTypes = ["image/jpeg", "image/png", "image/gif"];
        this.init();
    }
    
    init() {
        let fileInput = this.form.querySelector("input[type='file']");
        
        fileInput.addEventListener("change", (event) => {
            this.handleFileSelection(event.target.files);
        });
        
        this.form.addEventListener("submit", (event) => {
            event.preventDefault();
            this.uploadFiles();
        });
    }
    
    handleFileSelection(files) {
        let validFiles = [];
        let errors = [];
        
        Array.from(files).forEach(file => {
            if (this.validateFile(file)) {
                validFiles.push(file);
            } else {
                errors.push(`${file.name} is invalid`);
            }
        });
        
        if (errors.length > 0) {
            this.showErrors(errors);
        }
        
        this.validFiles = validFiles;
    }
    
    validateFile(file) {
        // Check file size
        if (file.size > this.maxFileSize) {
            return false;
        }
        
        // Check file type
        if (!this.allowedTypes.includes(file.type)) {
            return false;
        }
        
        return true;
    }
    
    async uploadFiles() {
        if (!this.validFiles || this.validFiles.length === 0) {
            this.showErrors(["No valid files to upload"]);
            return;
        }
        
        let formData = new FormData();
        
        this.validFiles.forEach((file, index) => {
            formData.append(`file${index}`, file);
        });
        
        try {
            let response = await fetch("/api/upload", {
                method: "POST",
                body: formData
            });
            
            if (response.ok) {
                console.log("Files uploaded successfully");
            } else {
                throw new Error("Upload failed");
            }
        } catch (error) {
            this.showErrors(["Upload failed: " + error.message]);
        }
    }
    
    showErrors(errors) {
        console.error("Upload errors:", errors);
        // Display errors to user
    }
}

new FileUploader("fileUploadForm");
```

### Dynamic Form Fields
```javascript
class DynamicForm {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.fieldCount = 0;
        this.init();
    }
    
    init() {
        this.container.innerHTML = `
            <form id="dynamicForm">
                <div id="fieldsContainer"></div>
                <button type="button" id="addField">Add Field</button>
                <button type="submit">Submit</button>
            </form>
        `;
        
        this.bindEvents();
    }
    
    bindEvents() {
        document.getElementById("addField").addEventListener("click", () => {
            this.addField();
        });
        
        document.getElementById("dynamicForm").addEventListener("submit", (event) => {
            event.preventDefault();
            this.handleSubmit();
        });
    }
    
    addField() {
        this.fieldCount++;
        let fieldContainer = document.createElement("div");
        fieldContainer.className = "field-container";
        fieldContainer.innerHTML = `
            <input type="text" name="field${this.fieldCount}" placeholder="Field ${this.fieldCount}">
            <button type="button" class="remove-field">Remove</button>
        `;
        
        document.getElementById("fieldsContainer").appendChild(fieldContainer);
        
        // Add remove functionality
        fieldContainer.querySelector(".remove-field").addEventListener("click", () => {
            fieldContainer.remove();
        });
    }
    
    handleSubmit() {
        let formData = new FormData(document.getElementById("dynamicForm"));
        let data = Object.fromEntries(formData);
        
        console.log("Dynamic form data:", data);
    }
}

new DynamicForm("dynamicFormContainer");
```

## Form Styling and UX

### CSS for Form Validation
```css
.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
}

.form-group input,
.form-group textarea,
.form-group select {
    width: 100%;
    padding: 10px;
    border: 2px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
    transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
    outline: none;
    border-color: #4CAF50;
}

.form-group input.error,
.form-group textarea.error,
.form-group select.error {
    border-color: #f44336;
}

.error-message {
    color: #f44336;
    font-size: 14px;
    margin-top: 5px;
}

.form-group input:valid {
    border-color: #4CAF50;
}

.form-group input:invalid {
    border-color: #f44336;
}

.submit-button {
    background-color: #4CAF50;
    color: white;
    padding: 12px 24px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s;
}

.submit-button:hover {
    background-color: #45a049;
}

.submit-button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
}
```

## Practice Exercises

### Exercise 1: Contact Form
Create a contact form with validation for:
- Name (required, min 2 characters)
- Email (required, valid email format)
- Phone (optional, valid phone format)
- Message (required, min 10 characters)

### Exercise 2: Registration Form
Create a registration form with:
- Username validation
- Password strength checking
- Confirm password matching
- Terms and conditions checkbox

### Exercise 3: Survey Form
Create a survey form with:
- Multiple choice questions
- Checkbox questions
- Rating scales
- Text area for comments

### Exercise 4: File Upload Form
Create a file upload form with:
- Multiple file selection
- File type validation
- File size limits
- Progress indication

### Exercise 5: Dynamic Form Builder
Create a form builder that allows users to:
- Add/remove form fields
- Change field types
- Set validation rules
- Preview the form

## Common Mistakes

### 1. Not Preventing Default Form Submission
```javascript
// Wrong - form will submit and page will refresh
form.addEventListener("submit", function(event) {
    console.log("Form submitted");
});

// Correct - prevent default behavior
form.addEventListener("submit", function(event) {
    event.preventDefault();
    console.log("Form submitted");
});
```

### 2. Not Validating on Both Client and Server
```javascript
// Client-side validation is not enough
function validateForm() {
    // Always validate on server too
    return true;
}
```

### 3. Not Handling Form Reset
```javascript
// Add form reset functionality
function resetForm() {
    document.getElementById("myForm").reset();
    // Clear any error messages
    document.querySelectorAll(".error-message").forEach(error => {
        error.remove();
    });
}
```

## Key Takeaways

- Forms are essential for user input and data collection
- Always validate data on both client and server side
- Use HTML5 validation attributes for basic validation
- Implement custom validation for complex requirements
- Handle form submission with JavaScript for better UX
- Provide clear error messages and feedback
- Test forms thoroughly with different input scenarios

## Next Steps

In the next lesson, we'll learn about:
- Local Storage and Session Storage
- Storing and retrieving data
- Data persistence
- Storage limits and best practices
- Working with JSON data

---

**Ready to store data locally? Let's learn about Local Storage! 🚀**
