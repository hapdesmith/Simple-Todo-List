<template>
  <div class="mb-2 px-3 py-2 border-2 rounded border-stone-300 hover:bg-stone-300 transition ease-in-out duration-300">
    <div v-if="isEditMode" class="flex flex-row content-center justify-between">
      <input type="text" v-model="newTaskName" class="mr-2 px-1 border-2 rounded focus:border-sky-600 hover:border-sky-600 transition ease-in-out duration-300">
      <div>
        <button class="mr-2 bg-stone-400 hover:bg-stone-600 py-1 px-3 transition ease-in-out duration-300 text-stone-100 rounded text-sm font-medium" @click="quitEditMode">CANCEL</button>
        <button class="mr-2 bg-emerald-600 hover:bg-emerald-800 py-1 px-3 transition ease-in-out duration-300 text-stone-100 rounded text-sm font-medium" @click="edit">CONFIRM</button>
      </div>
    </div>
    <div class="flex flex-row content-center justify-between" v-else>
      <div>
        <input type="checkbox" :id="title" class="mr-1" @click="recheck" v-model="checked" />
        <label :for="title" class="text-md font-medium">{{ title }}</label>
      </div>
      <div>
        <button class="mr-2 bg-emerald-600 hover:bg-emerald-800 py-1 px-3 transition ease-in-out duration-300 text-stone-100 rounded text-sm font-medium" @click="enterEditMode">EDIT</button>
        <button class="mr-1 bg-rose-600 hover:bg-rose-800 py-1 px-3 transition ease-in-out duration-300 text-stone-100 rounded text-sm font-medium" @click="delete">DELETE</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'checklist',
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      checked: false,
      isEditMode: false,
      newTaskName: '',
    }
  },
  methods: {
    recheck() {
      this.checked ? this.$emit('onNotComplete', this.title) : this.$emit('onComplete', this.title);
    },
    enterEditMode() {
      this.isEditMode = true;
      this.newTaskName = this.title;
    },
    quitEditMode() {
      this.isEditMode = false;
      this.newTaskName = '';
    },
    edit() {
      this.$emit('onEdit', this.title, this.newTaskName);
      this.quitEditMode();
    },
    delete() {
      this.$emit('onDelete', this.title);
    },
  },
}
</script>

<style scoped>
</style>