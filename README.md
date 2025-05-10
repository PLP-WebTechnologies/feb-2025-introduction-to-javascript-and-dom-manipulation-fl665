# Introduction to JavaScript and DOM Manipulation

## Objectives

Write basic JavaScript functions.
Manipulate the DOM dynamically.
Respond to user interactions.

## Instructions

- Create a script.js file and link it to a HTML.
- Structure the document using DOCTYPE, html, head, and body.

>[!NOTE]
>  - Write JavaScript that:
>  - Changes text content dynamically.
>  - Modifies CSS styles via JavaScript.
>  - Adds or removes an element when a button is clicked.


# Tasks
- Create a well-structured HTML5 document.
- Use at least 5 different HTML elements.
- Ensure semantic correctness.

Happy Coding! 💻✨


//index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Manager</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        header {
            text-align: center;
            margin-bottom: 30px;
        }
        .task-controls {
            display: flex;
            margin-bottom: 20px;
            gap: 10px;
        }
        .task-controls input {
            flex-grow: 1;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            padding: 12px;
            background-color: #f9f9f9;
            margin-bottom: 8px;
            border-radius: 4px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .completed {
            text-decoration: line-through;
            color: #888;
            background-color: #f0f0f0;
        }
        .theme-toggle {
            position: absolute;
            top: 20px;
            right: 20px;
        }
        .dark-theme {
            background-color: #333;
            color: #fff;
        }
        .dark-theme li {
            background-color: #444;
            color: #eee;
        }
        .dark-theme .completed {
            color: #aaa;
            background-color: #555;
        }
        .task-count {
            text-align: center;
            margin-top: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <header>
        <h1 id="page-title">My Task Manager</h1>
        <p>Stay organized and productive</p>
    </header>

    <main>
        <button id="theme-toggle" class="theme-toggle">Toggle Dark Mode</button>
        
        <section class="task-controls">
            <input type="text" id="task-input" placeholder="Enter a new task...">
            <button id="add-task">Add Task</button>
        </section>

        <section>
            <h2>Tasks</h2>
            <ul id="task-list">
                <li>Create HTML structure <button class="complete-btn">Complete</button><button class="delete-btn">Delete</button></li>
                <li>Link JavaScript file <button class="complete-btn">Complete</button><button class="delete-btn">Delete</button></li>
                <li>Style the page <button class="complete-btn">Complete</button><button class="delete-btn">Delete</button></li>
            </ul>
        </section>

        <section class="task-count">
            <p>Total tasks: <span id="task-counter">3</span></p>
        </section>
    </main>

    <footer>
        <p>&copy; 2025 Task Manager App</p>
    </footer>

    <script src="script.js"></script>
</body>
</html>

//script.js
// Get references to DOM elements
const taskInput = document.getElementById('task-input');
const addTaskBtn = document.getElementById('add-task');
const taskList = document.getElementById('task-list');
const taskCounter = document.getElementById('task-counter');
const themeToggle = document.getElementById('theme-toggle');
const pageTitle = document.getElementById('page-title');

// Initialize the application
function init() {
    // Add event listeners
    addTaskBtn.addEventListener('click', addNewTask);
    taskInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addNewTask();
        }
    });
    
    themeToggle.addEventListener('click', toggleTheme);
    
    // Add event listeners to existing task buttons
    addEventListenersToExistingTasks();
    
    // Update the task counter on load
    updateTaskCounter();
    
    // Welcome message
    setTimeout(() => {
        pageTitle.textContent = "Welcome to Your Task Manager";
        pageTitle.style.color = "#4CAF50";
    }, 1000);
}

// Add event listeners to the buttons in existing tasks
function addEventListenersToExistingTasks() {
    const completeBtns = document.querySelectorAll('.complete-btn');
    const deleteBtns = document.querySelectorAll('.delete-btn');
    
    completeBtns.forEach(btn => {
        btn.addEventListener('click', toggleTaskCompletion);
    });
    
    deleteBtns.forEach(btn => {
        btn.addEventListener('click', deleteTask);
    });
}

// Add a new task to the list
function addNewTask() {
    const taskText = taskInput.value.trim();
    
    if (taskText !== '') {
        // Create new task elements
        const newTaskItem = document.createElement('li');
        newTaskItem.textContent = taskText + ' ';
        
        // Create complete button
        const completeBtn = document.createElement('button');
        completeBtn.textContent = 'Complete';
        completeBtn.classList.add('complete-btn');
        completeBtn.addEventListener('click', toggleTaskCompletion);
        
        // Create delete button
        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'Delete';
        deleteBtn.classList.add('delete-btn');
        deleteBtn.addEventListener('click', deleteTask);
        
        // Append buttons to the task item
        newTaskItem.appendChild(completeBtn);
        newTaskItem.appendChild(deleteBtn);
        
        // Add the new task to the list
        taskList.appendChild(newTaskItem);
        
        // Clear the input field
        taskInput.value = '';
        
        // Update task counter
        updateTaskCounter();
        
        // Flash notification
        showNotification('Task added successfully!');
    }
}

// Mark a task as complete or incomplete
function toggleTaskCompletion(e) {
    const taskItem = e.target.parentElement;
    taskItem.classList.toggle('completed');
    
    // Change button text based on completion status
    if (taskItem.classList.contains('completed')) {
        e.target.textContent = 'Undo';
    } else {
        e.target.textContent = 'Complete';
    }
}

// Delete a task from the list
function deleteTask(e) {
    const taskItem = e.target.parentElement;
    
    // Add a brief fade-out animation
    taskItem.style.opacity = '0';
    taskItem.style.transition = 'opacity 0.5s ease';
    
    // Remove the task after the animation
    setTimeout(() => {
        taskList.removeChild(taskItem);
        updateTaskCounter();
    }, 500);
}

// Update the task counter
function updateTaskCounter() {
    const taskCount = taskList.children.length;
    taskCounter.textContent = taskCount;
    
    // Change color based on number of tasks
    if (taskCount >= 5) {
        taskCounter.style.color = '#e74c3c'; // Red for many tasks
    } else if (taskCount >= 3) {
        taskCounter.style.color = '#f39c12'; // Orange for moderate tasks
    } else {
        taskCounter.style.color = '#27ae60'; // Green for few tasks
    }
}

// Toggle between light and dark themes
function toggleTheme() {
    document.body.classList.toggle('dark-theme');
    
    if (document.body.classList.contains('dark-theme')) {
        themeToggle.textContent = 'Toggle Light Mode';
    } else {
        themeToggle.textContent = 'Toggle Dark Mode';
    }
}

// Show a temporary notification
function showNotification(message) {
    // Create notification element
    const notification = document.createElement('div');
    notification.textContent = message;
    notification.style.position = 'fixed';
    notification.style.bottom = '20px';
    notification.style.right = '20px';
    notification.style.backgroundColor = '#4CAF50';
    notification.style.color = 'white';
    notification.style.padding = '10px 20px';
    notification.style.borderRadius = '4px';
    notification.style.opacity = '0';
    notification.style.transition = 'opacity 0.3s ease';
    
    // Add to document
    document.body.appendChild(notification);
    
    // Fade in
    setTimeout(() => {
        notification.style.opacity = '1';
    }, 10);
    
    // Remove after 3 seconds
    setTimeout(() => {
        notification.style.opacity = '0';
        setTimeout(() => {
            document.body.removeChild(notification);
        }, 300);
    }, 3000);
}

// Initialize the app when the DOM is fully loaded
document.addEventListener('DOMContentLoaded', init);
