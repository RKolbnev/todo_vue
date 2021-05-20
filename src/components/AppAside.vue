<template>
  <aside class="aside">
    <div class="add-folder">
      <input type="text"
        placeholder="Введите название"
        v-model="input"
        v-on:keyup.enter="addFolder">
      <button
        @click="addFolder"
      >Создать</button>
    </div>
    <div class="scroll">
      <the-folder
        v-for="folder in folders"
        :key="folder.title"
        :folder="folder"
        @openFolder="openFolder"
        @addTask="addTask"
        @checkTask="checkTask"
        @deleteTask="deleteTask">
      </the-folder>
      <div v-if="!folders.length" class="no-folder">У вас нет папок с задачами</div>
    </div>
  </aside>
</template>

<script>
import TheFolder from './TheFolder'

export default {
  props: {
    folders: Array
  },
  emits: [
    'openFolder',
    'addFolder',
    'addTask',
    'checkTask',
    'deleteTask'
  ],
  data () {
    return {
      input: ''
    }
  },
  components: {
    'the-folder': TheFolder
  },
  methods: {
    openFolder (id) {
      this.$emit('openFolder', id)
    },
    addFolder () {
      const newFolder = {
        title: this.input,
        id: 3,
        tasks: []
      }
      this.$emit('addFolder', newFolder)
      this.input = ''
    },
    addTask (folderId, newTask) {
      this.$emit('addTask', folderId, newTask)
    },
    checkTask (folderId, taskId) {
      this.$emit('checkTask', folderId, taskId)
    },
    deleteTask (folderId, taskId) {
      this.$emit('deleteTask', folderId, taskId)
    }
  }
}
</script>

<style lang='sass'>
// SCROLl
.scroll
  height: 90%
  overflow-x: hidden
  overflow-y: auto
  &::-webkit-scrollbar
    width: 0.35em
  &::-webkit-scrollbar-thumb
    background-color: #ffffffa8
    width: 0.2em
    border-radius: 1em
// ASIDE
.aside
  left: 0
  height: 85vh
  width: 25rem
  box-sizing: border-box
  background: radial-gradient(circle at 100% 60%, #1f3a47 81%, darken(#1f3a47, 5%))
// ADD-FOLDER
.add-folder
  display: flex
  flex-direction: row
  flex-wrap: nowrap
  justify-content: space-between
  padding: 0.5rem
  & input
    padding: 0.3rem
    width: fill
    margin: 0
    margin-right: 0.5rem
    background-color:
    border: 2px solid white
    border-radius: 0.25rem
    &:focus
      outline: none
  & button
    padding: 0.2rem 0.5rem
    color: #1f3a47
    background-color: #b7c4c8
    border: none
    border-radius: 0.25rem
.no-folder
  margin-top: 1rem
  text-align: center
  color: #ffffff

</style>
