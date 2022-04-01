<template>
  <div>
    <h1>Simple Todo List</h1>
    <label for="newTask">New Task Name</label> : 
    <input 
      type="text" 
      id="newTask" 
      v-model="newTask"/> 
    <button @click="addNewTask">Add Task</button>
    <h2>Current Tasks :</h2>
    <checklist v-if="!!taskList.length" 
      v-for="task in taskList" 
      :key="task" 
      :title="task" 
      @onComplete="completeTask" 
      @onNotComplete="unCompleteTask"
      @onEdit="confirmEditTask"
      @onDelete="deleteTask"/>
    <p v-else>task masih kosong</p>
    <h2>Completed Tasks :</h2>
    <ul v-if="!!completedTask.length">
      <li v-for="task in completedTask" :key="task">
        {{ task }}
      </li>
    </ul>
    <p v-else>completed task masih kosong</p>
  </div>
  
</template>

<script>
import checklist from './components/checklist.vue';
export default {
  name: 'App',
  components: {
    checklist,
  },
  data() {
    return {
      newTask: '',
      taskList: [ 
        'task 1',
        'task 2',
        'task 3',
        'task 4',
        'task 5',
      ],
      completedTask: [],
      enableGenTask: true,
    };
  },
  methods: {
    completeTask(e) {
      this.completedTask.push(e);
    },
    unCompleteTask(e) {
      this.completedTask = this.completedTask.filter(x => x !== e);
    },
    confirmEditTask(title, newTitle) {
      const index = this.taskList.indexOf(title);
      this.taskList.splice(index, 1,newTitle);
      const found = this.completedTask.indexOf(title);
      if (found != -1) {
        this.completedTask.splice(index, 1,newTitle);
      };
    },
    deleteTask(e) {
      this.unCompleteTask(e);
      this.taskList = this.taskList.filter(x => x !== e);
    },
    addNewTask() {
      if (!!this.newTask) this.taskList.push(this.newTask);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>