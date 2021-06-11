<template>
  <div class="list">
    <div :class="{ 'list-name': true, 'list-name__open': isOpen }"
      @click="isOpen = !isOpen">
      <span class="list-img"
      @click="deleteList"
      @mouseenter="showDelete='basket3'"
      @mouseleave="showDelete='list-icon'">
        <img :src="require(`../assets/${showDelete}.png`)" alt="list"/>
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
  emits: ['deleteList', 'addTask', 'taskEvent'],

  data () {
    return {
      isOpen: false,
      showDelete: 'list-icon',
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
    deleteList () {
      if (this.showDelete === 'basket3') {
        this.$emit('deleteList', [this.list.id])
      }
    },
    taskEvent (event, id) {
      const elem = ['SPAN', 'BUTTON']
      const emit = ['showTask', 'deleteTask']
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
