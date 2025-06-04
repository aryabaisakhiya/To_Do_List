# Ex03 To-Do List using JavaScript
## Date:

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
HTML:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neubrutalist To-Do List</title>
    <link rel="stylesheet" href="styles.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div class="container">
        <h1 class="title">TO-DO LIST</h1>
        <div class="input-container">
            <input type="text" id="taskInput" placeholder="Add a new task..." class="task-input">
            <button id="addTaskBtn" class="add-btn">ADD</button>
        </div>
        <ul id="taskList" class="task-list"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>
```
CSS
```
:root {
    --primary: #FF6B6B;
    --secondary: #4ECDC4;
    --dark: #292F36;
    --light: #F7FFF7;
    --accent: #FFE66D;
    --border: 4px solid var(--dark);
    --shadow: 8px 8px 0px var(--dark);
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Courier New', monospace;
}

body {
    background-color: var(--light);
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.container {
    background-color: white;
    border: var(--border);
    box-shadow: var(--shadow);
    padding: 30px;
    width: 100%;
    max-width: 500px;
}

.title {
    color: var(--dark);
    text-align: center;
    margin-bottom: 20px;
    font-size: 2.5rem;
    text-transform: uppercase;
    letter-spacing: -2px;
}

.input-container {
    display: flex;
    margin-bottom: 20px;
    gap: 10px;
}

.task-input {
    flex: 1;
    padding: 12px 15px;
    border: var(--border);
    font-size: 1rem;
    outline: none;
}

.task-input:focus {
    box-shadow: 4px 4px 0px var(--dark);
}

.add-btn {
    background-color: var(--primary);
    color: white;
    border: var(--border);
    padding: 0 20px;
    cursor: pointer;
    font-weight: bold;
    font-size: 1rem;
    transition: all 0.2s;
}

.add-btn:hover {
    background-color: var(--dark);
    transform: translate(2px, 2px);
    box-shadow: 4px 4px 0px var(--dark);
}

.task-list {
    list-style: none;
}

.task-item {
    display: flex;
    align-items: center;
    padding: 15px;
    background-color: white;
    border: var(--border);
    margin-bottom: 10px;
    transition: all 0.2s;
}

.task-item:hover {
    transform: translate(4px, 4px);
    box-shadow: 4px 4px 0px var(--dark);
}

.task-checkbox {
    appearance: none;
    width: 20px;
    height: 20px;
    border: var(--border);
    margin-right: 15px;
    cursor: pointer;
    position: relative;
}

.task-checkbox:checked {
    background-color: var(--secondary);
}

.task-checkbox:checked::after {
    content: "✓";
    position: absolute;
    color: var(--dark);
    font-weight: bold;
    left: 3px;
    top: -2px;
}

.task-text {
    flex: 1;
    font-size: 1.1rem;
}

.task-text.completed {
    text-decoration: line-through;
    color: #777;
}

.delete-btn {
    background: none;
    border: none;
    cursor: pointer;
    color: var(--dark);
    font-size: 1.2rem;
    margin-left: 10px;
    transition: transform 0.2s;
}

.delete-btn:hover {
    transform: scale(1.2);
    color: var(--primary);
}
```
JAVASCRIPT
```
document.addEventListener('DOMContentLoaded', function() {
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];


    function renderTasks() {
        taskList.innerHTML = '';
        
        if (tasks.length === 0) {
            const emptyMessage = document.createElement('li');
            emptyMessage.textContent = 'No tasks yet!';
            emptyMessage.classList.add('empty-message');
            taskList.appendChild(emptyMessage);
        } else {
            tasks.forEach((task) => {
                const taskItem = document.createElement('li');
                taskItem.classList.add('task-item');
                taskItem.dataset.id = task.id;

                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.classList.add('task-checkbox');
                checkbox.checked = task.completed;
                checkbox.addEventListener('change', () => toggleTaskComplete(task.id));

                const taskText = document.createElement('span');
                taskText.classList.add('task-text');
                if (task.completed) taskText.classList.add('completed');
                taskText.textContent = task.text;

                const deleteBtn = document.createElement('button');
                deleteBtn.classList.add('delete-btn');
                deleteBtn.innerHTML = '<i class="fas fa-trash"></i>';
                deleteBtn.addEventListener('click', () => deleteTask(task.id));

                taskItem.appendChild(checkbox);
                taskItem.appendChild(taskText);
                taskItem.appendChild(deleteBtn);

                taskList.appendChild(taskItem);
            });
        }
    }
    function addTask() {
        const text = taskInput.value.trim();
        if (text) {
            const newTask = {
                id: Date.now(),
                text,
                completed: false
            };
            tasks.push(newTask);
            saveTasks();
            taskInput.value = '';
            renderTasks();
        }
    }
    function toggleTaskComplete(id) {
        tasks = tasks.map(task => 
            task.id === id ? { ...task, completed: !task.completed } : task
        );
        saveTasks();
        renderTasks();
    }
    function deleteTask(id) {
        tasks = tasks.filter(task => task.id !== id);
        saveTasks();
        renderTasks();
    }
    function saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    addTaskBtn.addEventListener('click', addTask);
    taskInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') addTask();
    });
    renderTasks();
});
```

## OUTPUT
![image](https://github.com/user-attachments/assets/2f1159d2-7a6b-4d4e-8239-50b910189509)
![image](https://github.com/user-attachments/assets/a3cb615e-adbf-4e66-964e-57a27b96e221)



## RESULT
The program for creating To-do list using JavaScript is executed successfully.
