<template>
  <div class="folder">
    <!-- <div class="folder-name" -->
    <div
      :class="{'folder-name': true, 'folder-name__open': folder.isOpen}"
      @click="openFolder">
      <span class="folder-img"><img src="../assets/folder-icon.png" alt="folder"></span>
      <span class="folder-title">{{folder.title}}</span>
      <span class="folder-btn"><img src="../assets/menu-icon.png" alt="menu"></span>
    </div>
    <div
      class="folder-tasks"
      v-if="folder.isOpen">
      <the-task
        v-for="task in folder.tasks"
        :key="task.title"
        :task="task"
        @checkTask="checkTask"
        @deleteTask="deleteTask"
      ></the-task>
      <div class="add-task"
        @click="inputState = !inputState"
        v-if="!inputState"
      >
        <img src="../assets/add-task.png" alt="Добавить задачу">
        <span >Добавить задачу</span>
      </div>
      <div
        class="input-item"
        v-else
      >
        <input type="text">
        <button>Добавить</button>
      </div>
    </div>
  </div>
</template>

<script>
import TheTask from './TheTask'

export default {
  props: {
    folder: Object
  },
  emits: [
    'openFolder',
    'addTask',
    'checkTask',
    'deleteTask'
  ],
  data () {
    return {
      inputState: false
    }
  },
  components: {
    'the-task': TheTask
  },
  methods: {
    openFolder () {
      this.$emit('openFolder', this.folder.id)
    },
    addTask () {
      const newTask = {
        title: 'Task name 1',
        id: 5,
        state: false,
        notes: '',
        checklist: []
      }
      this.$emit('addTask', this.folder.id, newTask)
    },
    checkTask (taskId) {
      this.$emit('checkTask', this.folder.id, taskId)
    },
    deleteTask (taskId) {
      this.$emit('deleteTask', this.folder.id, taskId)
    }
  }
}
</script>

<style lang="sass">
.folder
  cursor: pointer
  display: flex
  flex-direction: column
  margin: 0.5rem
  background-color: #f4f4f4
  border-radius: 0.25rem
  transition: 0.3s
  &:hover
    transition: 0.3s
    box-shadow: 9px 7px 0px 1px rgba(0, 0, 0, 0.4);
  &-name
    display: flex
    flex-direction: row
    flex-wrap: nowrap
    justify-content: space-between
    align-items: center
    padding: 0 0.25rem
  &-img, &-btn
    flex-basis: 10%
    & img
      width: 1.5rem
  &-title
    flex-basis: 70%
.add-task
  cursor: pointer
  display: flex
  flex-wrap: nowrap
  justify-content: center
  padding: 0.5rem 0
  & img
    width: 1rem
    margin-right: 0.5rem
  & span
    font-size: 0.8rem

.input-item
  display: flex
  flex-wrap: nowrap
  padding: 0.5rem
  justify-content: space-between
  & input
    width: fill
    padding: 0.2rem
    margin-right: 0.2rem
    border: 2px solid #b7c4c8
    border-radius: 0.25rem
    &:focus
      outline: none
  & button
    padding: 0.25rem 0.5rem
    color: #1f3a47
    background-color: #b7c4c8
    border: none
    border-radius: 0.25rem
.folder-name__open
  border-bottom: 2px solid #b7c4c8
</style>
