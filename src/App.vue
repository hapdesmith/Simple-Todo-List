<template>
  <div class="flex flex-col w-full font-sans text-stone-600 px-4 py-4">
    <h1 class="text-xl font-bold text-center mb-4">Simple Todo List</h1>
    <div class="flex flex-col mb-4">
      <input 
        placeholder="NEW TASK"
        type="text" 
        id="newTask" 
        v-model="newTask"
        class="mb-2 p-2 border-2 rounded focus:border-sky-600 hover:border-sky-600 transition ease-in-out duration-300"/> 
      <button class="bg-sky-600 hover:bg-sky-800 p-2 transition ease-in-out duration-300 text-stone-100 rounded text-sm font-medium" @click="addNewTask">ADD TASK</button>
    </div>
    <div>
      <h2 class="text-lg font-medium mb-4">Current Tasks :</h2>
      <checklist v-if="!!taskList.length" 
        v-for="task in taskList" 
        :key="task" 
        :title="task" 
        @onComplete="completeTask" 
        @onNotComplete="unCompleteTask"
        @onEdit="confirmEditTask"
        @onDelete="deleteTask"/>
      <p v-else>task masih kosong</p>
    </div>
    <div>
      <h2 class="text-lg font-medium mb-4">Completed Tasks :</h2>
      <ul v-if="!!completedTask.length" class="flex flex-row">
        <li class="mr-2 mb-2 px-3 py-2 border-2 rounded bg-emerald-400 text-stone-100" v-for="task in completedTask" :key="task">
          {{ task }}
        </li>
      </ul>
      <p v-else>completed task masih kosong</p>
    </div>
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
}
</style>