<template>
  <body>
    <div class="container">
  
    <h1>Task Tamer </h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a task..." />
    <button @click="addTask">Add</button>
  
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span class="task-text" :class="{ done: task.completed }" @click="toggleTask(index)">
            {{ task.text }}
          </span>
          <button @click="removeTask(index)">❌</button>
        </li>
      </ul>
      <button @click="toggleDarkMode">Toggle Dark Mode</button>

  </div>
    
  </body>
</template>

<script>
import { ref, onMounted, watch } from "vue";

export default {
  setup() {
    const tasks = ref(JSON.parse(localStorage.getItem("tasks")) || []);
    const newTask = ref("");
    const darkMode = ref(false);

    const addTask = () => {
      if (newTask.value.trim()) {
        tasks.value.push({ text: newTask.value, completed: false });
        newTask.value = "";
        saveTasks();}
      };
     const toggleTask = (index) => {
      tasks.value[index].completed = !tasks.value[index].completed;
      saveTasks();
    };

    const removeTask = (index) => {
      tasks.value.splice(index, 1);
      saveTasks();
    };

    const saveTasks = () => {
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
    };

    onMounted(() => {
      tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
    });

    const toggleDarkMode = () => {
      darkMode.value = !darkMode.value;
    };

    watch(darkMode, (value) => {
      document.body.classList.toggle('dark', value);
    });

    return { tasks, newTask, addTask, toggleTask, removeTask,toggleDarkMode };
  },
  
};
</script>

<style>
body{
  font-family:'Courier New';
  background-color: aliceblue;
  justify-self:center;

}

body.dark {
  background-color: #121212;
}

.container {
  width: 50vw;
  text-align: center;
  background-color: rgb(103, 189, 192);
  border-radius: 10;
  border: solid 2px black;
}
input {
  padding: 8px;
  width: 70%;
}
button {
  margin-left: 10px;
  padding: 8px;
}
ul {
  list-style: none;
  padding: 0;
  
}
li {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid #ccc;
}
.done {
  text-decoration: line-through;
  color: gray;
  cursor: pointer;
  background-color: green;
}
.task-text{
  border-radius: 5px;
  align-self: center;
  padding: 10px;
}
.task-text:hover {
  background-color: antiquewhite;
  cursor: pointer;
  transition: 0.5s;
}
</style>