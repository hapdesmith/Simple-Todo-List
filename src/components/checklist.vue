<template>
  <div>
    <div v-if="isEditMode">
      <input type="text" v-model="newTaskName">
      <button @click="quitEditMode">cancel</button>
      <button @click="edit">confirm</button>
    </div>
    <div v-else>
      <input type="checkbox" :id="title" @click="recheck" v-model="checked" />
      <label :for="title">{{ title }}</label>
      <button @click="enterEditMode">edit</button>
      <button @click="delete">delete</button>
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