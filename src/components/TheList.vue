<template>
  <div class="list">
    <div
      :class="{ 'list-name': true, 'list-name__open': isOpen }"
      @click="isOpen = !isOpen">
      <span class="list-img">
        <img src="../assets/list-icon.png" alt="list"/>
      </span>
      <span class="list-title">{{ list.title }}</span>
      <span class="list-btn">
        <img src="../assets/menu-icon.png" alt="menu"/>
      </span>
    </div>
    <div class="list-tasks" v-if="isOpen">
      <the-task
        v-for="task in list.tasks"
        :key="task.id"
        :task="task">
      </the-task>
      <div class="add-task"
        v-if="!inputState"
        @click="inputState = !inputState">
        <img src="../assets/add-task.png" alt="Добавить задачу" />
        <span>Добавить задачу</span>
      </div>
      <div class="input-item"
        v-else>
        <input type="text"
          @keyup.enter="addTask"
          v-model="inputValue"/>
        <button
          @click="addTask">Добавить</button>
      </div>
    </div>
  </div>
</template>

<script>
import TheTask from './TheTask'

export default {
  props: {
    list: Object
  },
  components: {
    'the-task': TheTask
  },
  emits: ['addTask'],

  data () {
    return {
      inputValue: '',
      inputState: false,
      isOpen: false
    }
  },
  methods: {
    addTask () {
      this.$emit('addTask', this.inputValue, [this.list.id])
      this.inputValue = ''
    }
  }
}
</script>

<style>

</style>
