<template>
  <div class="main">
    <div v-if="task" class="main__1">
      <div class="main-header" @click="checkTask">
        <img v-if="!task.state" src="../assets/no-check.png" alt="" />
        <img v-else src="../assets/check.png" alt="" />
        <span :class="{ 'task-title': true, checked: task.state }">
          {{ task.title }}
        </span>
        <span class="task-delete" @click="deleteTask">
          <img src="../assets/basket.png" alt="" />
        </span>
      </div>
      <div class="main-content">
        <div class="main-checkbox" @click="addStep">
          <img src="../assets/plus.png" alt="">
          <span>Добавить шаг</span>
        </div>
        <div class="main-notes">
          <div class="main-notes__header">
            <img src="../assets/notes.png" alt="">
            <span>Заметки</span>
          </div>
          <div contenteditable="true" class="main-notes__content"
            @input="changeNotes">{{task.notes}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  },
  methods: {
    changeNotes (e) {
      this.$emit('changeNotes', e.target.innerHTML)
    },
    addStep () {

    }
  }
}
</script>

<style></style>
