<template>
  <div class="list">
    <div :class="{ 'list-name': true, 'list-name__open': isOpen }"
      @click="isOpen = !isOpen">
      <span class="list-img">
        <img src="../assets/list-icon.png" alt="list"/>
      </span>
      <span class="list-title">
        {{ list.title }}
      </span>
      <span class="list-btn">
        <img :class="{'list__open': isOpen}" src="../assets/arrow.png" alt="menu"/>
      </span>
    </div>
    <div class="list-tasks" v-if="isOpen">
      <the-task
        v-for="task in list.tasks"
        :key="task.id"
        :task="task"
        @click="taskEvent($event, task.id)">
      </the-task>
      <the-add
        @addTask="addTask"
        :property="addSettings"
      ></the-add>
    </div>
  </div>
</template>

<script>
import TheTask from './TheTask'
import TheAdd from './TheAdd'

export default {
  props: {
    list: Object
  },
  components: {
    'the-task': TheTask,
    'the-add': TheAdd
  },
  emits: ['addTask', 'taskEvent'],

  data () {
    return {
      isOpen: false,
      addSettings: {
        logo: 'add-task2',
        text: 'Добавить задачу',
        buttonClass: 'add-task'
      }
    }
  },
  methods: {
    addTask (title) {
      const body = {
        id: null,
        title,
        state: false,
        note: '',
        steps: null
      }
      const path = [this.list.id, 'tasks']
      this.$emit('addTask', body, path)
    },
    taskEvent (event, id) {
      const elem = ['SPAN', 'INPUT', 'BUTTON']
      const emit = ['showTask', 'checkTask', 'deleteTask']
      const idx = elem.indexOf(event.target.tagName)
      if (idx !== -1) {
        this.$emit('taskEvent', emit[idx], this.list.id, id)
      }
    }
  }
}
</script>

<style>

</style>
