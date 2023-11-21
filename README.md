# Online-Notes-Taking---APP-
Develop a simple to-do list application using JS frameworks and Database. Users should be able to add, edit, and delete tasks. This project introduces you to basic interactivity.

using HTML, CSS, and JavaScript:
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do List App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }

    #taskList {
      list-style-type: none;
      padding: 0;
    }

    li {
      margin: 10px 0;
    }

    button {
      margin-left: 5px;
    }
  </style>
</head>
<body>

  <h1>To-Do List</h1>

  <form id="taskForm">
    <label for="task">New Task:</label>
    <input type="text" id="task" required>
    <button type="submit">Add Task</button>
  </form>

  <ul id="taskList"></ul>

  <script>
    document.addEventListener('DOMContentLoaded', function () {
      const taskForm = document.getElementById('taskForm');
      const taskInput = document.getElementById('task');
      const taskList = document.getElementById('taskList');

      taskForm.addEventListener('submit', function (event) {
        event.preventDefault();
        addTask(taskInput.value);
        taskInput.value = '';
      });

      taskList.addEventListener('click', function (event) {
        if (event.target.tagName === 'BUTTON') {
          const taskItem = event.target.parentElement;
          const taskId = taskItem.dataset.id;

          if (event.target.classList.contains('delete')) {
            deleteTask(taskId);
          } else if (event.target.classList.contains('edit')) {
            editTask(taskItem);
          }
        }
      });

      function addTask(taskText) {
        const taskId = Date.now().toString();
        const taskItem = document.createElement('li');
        taskItem.dataset.id = taskId;
        taskItem.innerHTML = `
          <span>${taskText}</span>
          <button class="edit">Edit</button>
          <button class="delete">Delete</button>
        `;
        taskList.appendChild(taskItem);
        saveTask(taskId, taskText);
      }

      function deleteTask(taskId) {
        const taskItem = document.querySelector(`[data-id="${taskId}"]`);
        taskItem.remove();
        removeTaskFromStorage(taskId);
      }

      function editTask(taskItem) {
        const taskId = taskItem.dataset.id;
        const taskText = taskItem.querySelector('span').textContent;
        const newTaskText = prompt('Edit task:', taskText);

        if (newTaskText !== null) {
          taskItem.querySelector('span').textContent = newTaskText;
          updateTaskInStorage(taskId, newTaskText);
        }
      }

      function saveTask(taskId, taskText) {
        const tasks = JSON.parse(localStorage.getItem('tasks')) || {};
        tasks[taskId] = taskText;
        localStorage.setItem('tasks', JSON.stringify(tasks));
      }

      function removeTaskFromStorage(taskId) {
        const tasks = JSON.parse(localStorage.getItem('tasks')) || {};
        delete tasks[taskId];
        localStorage.setItem('tasks', JSON.stringify(tasks));
      }

      function updateTaskInStorage(taskId, taskText) {
        const tasks = JSON.parse(localStorage.getItem('tasks')) || {};
        tasks[taskId] = taskText;
        localStorage.setItem('tasks', JSON.stringify(tasks));
      }

      // Load tasks from local storage on page load
      function loadTasks() {
        const tasks = JSON.parse(localStorage.getItem('tasks')) || {};

        for (const taskId in tasks) {
          addTask(tasks[taskId]);
        }
      }

      loadTasks();
    });
  </script>

</body>
</html>

the example uses the localStorage to store tasks. Tasks are stored as key-value pairs, where the key is a unique identifier for each task, and the value is the task text. The tasks are loaded from localStorage on page load, and you can add, edit, and delete tasks interactively. Note that using localStorage has limitations, and in a real-world application, you might want to use a more robust database solution.
