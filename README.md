# to-do-list-
my first coding 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Simple To-Do List</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 40px; background: #f7f7f7; }
    #todo-app { max-width: 400px; margin: auto; background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px #ccc;}
    h2 { text-align: center; }
    #new-task { width: 80%; padding: 8px; }
    #add-btn { padding: 8px 16px; }
    ul { list-style: none; padding: 0; }
    li { display: flex; align-items: center; margin: 8px 0; padding: 8px; background: #eee; border-radius: 4px; }
    li.done { text-decoration: line-through; opacity: 0.6; }
    .delete-btn { margin-left: auto; color: #fff; background: #e74c3c; border: none; border-radius: 4px; padding: 4px 8px; cursor: pointer; }
    .check-btn { margin-right: 10px; }
  </style>
</head>
<body>
  <div id="todo-app">
    <h2>To-Do List</h2>
    <input type="text" id="new-task" placeholder="Add a new task..." />
    <button id="add-btn">Add</button>
    <ul id="tasks"></ul>
  </div>
  <script>
    const taskInput = document.getElementById('new-task');
    const addBtn = document.getElementById('add-btn');
    const tasksUl = document.getElementById('tasks');

    // Load tasks from localStorage
    function loadTasks() {
      return JSON.parse(localStorage.getItem('tasks') || '[]');
    }

    // Save tasks to localStorage
    function saveTasks(tasks) {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    // Render tasks
    function renderTasks() {
      const tasks = loadTasks();
      tasksUl.innerHTML = '';
      tasks.forEach((task, idx) => {
        const li = document.createElement('li');
        li.className = task.done ? 'done' : '';
        // Checkbox to mark done
        const checkBtn = document.createElement('input');
        checkBtn.type = 'checkbox';
        checkBtn.className = 'check-btn';
        checkBtn.checked = task.done;
        checkBtn.onclick = () => {
          tasks[idx].done = !tasks[idx].done;
          saveTasks(tasks);
          renderTasks();
        };
        li.appendChild(checkBtn);
        // Task text
        li.appendChild(document.createTextNode(task.text));
        // Delete button
        const delBtn = document.createElement('button');
        delBtn.textContent = 'Delete';
        delBtn.className = 'delete-btn';
        delBtn.onclick = () => {
          tasks.splice(idx, 1);
          saveTasks(tasks);
          renderTasks();
        };
        li.appendChild(delBtn);
        tasksUl.appendChild(li);
      });
    }

    // Add new task
    addBtn.onclick = () => {
      const text = taskInput.value.trim();
      if (text) {
        const tasks = loadTasks();
        tasks.push({ text, done: false });
        saveTasks(tasks);
        taskInput.value = '';
        renderTasks();
      }
    };

    // Press Enter to add
    taskInput.addEventListener('keyup', (e) => {
      if (e.key === 'Enter') addBtn.click();
    });

    // Initial render
    renderTasks();
  </script>
</body>
</html>
