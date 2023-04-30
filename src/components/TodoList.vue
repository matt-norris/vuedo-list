<template>
  <div>
    <h2>My To-Do List</h2>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.completed" @change="deleteTask(task)">
        <span :class="{ 'completed': task.completed }">{{ task.description }}</span>
      </li>
    </ul>
    <form @submit.prevent="addTask">
      <input type="text" v-model="newTask" placeholder="Add a new task">
      <button type="submit">Add Task</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    const tasks = JSON.parse(localStorage.getItem('tasks')) || [
      {
        description: 'Task 1',
        completed: false
      },
      {
        description: 'Task 2',
        completed: true
      },
      {
        description: 'Task 3',
        completed: false
      }
    ]
    return {
      tasks,
      newTask: ''
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({
          description: this.newTask.trim(),
          completed: false
        })
        this.newTask = ''
        this.saveTasks()
      }
    },
    deleteTask(task) {
      if (task.completed) {
        const index = this.tasks.indexOf(task)
        this.tasks.splice(index, 1)
        this.saveTasks()
      }
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}
</script>

<style>
h2 {
  margin-bottom: 20px;
  text-align: center;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}

.completed {
  text-decoration: line-through;
}

input[type="checkbox"] {
  margin-right: 10px;
}

form {
  margin-top: 20px;
  display: flex;
  align-items: center;
}

form input[type="text"] {
  flex-grow: 1;
  margin-right: 10px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

form button {
  padding: 5px 10px;
  border-radius: 5px;
  border: none;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
}


form button:hover {
  background-color: #3e8e41;
}

</style>
