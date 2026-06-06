
<template>
  <div class="container">
    <h1>Task Manager</h1>

    <div class="add-task">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Enter a task"
      />

      <select v-model="newPriority">
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>

      <button @click="addTask">Add</button>
    </div>

    <p>{{ doneCount }} of {{ tasks.length }} done</p>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.done" />

        <span :class="{ completed: task.done }">
          {{ task.text }}
        </span>

        <span
          class="badge"
          :class="{
            high: task.priority === 'High',
            medium: task.priority === 'Medium',
            low: task.priority === 'Low'
          }"
        >
          {{ task.priority }}
        </span>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      newTask: '',
      newPriority: 'Low',
      tasks: [
        {
          text: 'Learn Vue',
          done: false,
          priority: 'High'
        }
      ]
    }
  },

  computed: {
    doneCount() {
      return this.tasks.filter(task => task.done).length
    }
  },
  methods: {
    addTask() {
      if (!this.newTask.trim()) return

      this.tasks.push({
        text: this.newTask,
        done: false,
        priority: this.newPriority
      })

      this.newTask = ''
      this.newPriority = 'Low'
    }
  }
}
</script>

<style>
.container {
  max-width: 600px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}

.add-task {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input,
select,
button {
  padding: 8px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.badge {
  color: white;
  padding: 4px 8px;
  border-radius: 5px;
  font-size: 12px;
}

.high {
  background-color: red;
}

.medium {
  background-color: orange;
}

.low {
  background-color: green;
}
</style>