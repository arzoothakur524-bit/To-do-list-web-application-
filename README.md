let tasks = [];


window.onload = function () {
  tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  showTasks();
};


function addTask() {
  const input = document.getElementById("taskInput");
  const text = input.value.trim();

  if (text === "") {
    alert("Enter a task!");
    return;
  }

  tasks.push({ text: text, done: false });
  input.value = "";

  saveTasks();
  showTasks();
}


function showTasks(filter = "all") {
  const list = document.getElementById("taskList");
  list.innerHTML = "";

  let filtered = tasks;

  if (filter === "done") {
    filtered = tasks.filter(t => t.done);
  } else if (filter === "pending") {
    filtered = tasks.filter(t => !t.done);
  }

  filtered.forEach((task, index) => {
    const li = document.createElement("li");
    li.className = task.done ? "done" : "";

    li.innerHTML = `
      <span onclick="toggleTask(${index})">${task.text}</span>
      <button class="delete-btn" onclick="deleteTask(${index})">X</button>
    `;

    list.appendChild(li);
  });

  document.getElementById("taskCount").innerText = tasks.length + " Tasks";
}


function toggleTask(index) {
  tasks[index].done = !tasks[index].done;
  saveTasks();
  showTasks();
}


function deleteTask(index) {
  tasks.splice(index, 1);
  saveTasks();
  showTasks();
}


function clearAll() {
  tasks = [];
  saveTasks();
  showTasks();
}


function filterTasks(type) {
  showTasks(type);
}


function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}


document.getElementById("taskInput").addEventListener("keypress", function(e) {
  if (e.key === "Enter") {
    addTask();
  }
});
