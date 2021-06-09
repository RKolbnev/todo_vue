<template>
  <aside class="aside">
    <div class="add-list">
      <input
        type="text"
        placeholder="Введите название"
        v-model="input"
        v-on:keyup.enter="addList"/>
      <button @click="addList">Создать</button>
    </div>
    <div class="scroll">
      <the-list
        v-for="list in sortList"
        :key="list.id"
        :list="list"
        @addTask="addTask"
        @taskEvent="taskEvent"
        >
      </the-list>
      <div v-if="!lists" class="no-list">
        У вас нет списков с задачами
      </div>
    </div>
  </aside>
</template>

<script>
import TheList from './TheList'

export default {
  props: {
    lists: Object
  },
  emits: ['addList', 'addTask', 'showTask', 'checkTask', 'deleteTask'],
  data () {
    return {
      input: ''
    }
  },
  components: {
    'the-list': TheList
  },
  methods: {
    addList () {
      if (this.input.length > 0) {
        const body = {
          id: null,
          title: this.input,
          tasks: null
        }
        this.$emit('addList', body)
        this.input = ''
      }
    },
    addTask (body, path) {
      this.$emit('addTask', body, path)
    },
    taskEvent (event, idList, idTask) {
      this.$emit(event, idList, idTask)
    },
    // },
    // computed: {
    sortList () {
      if (this.lists) {
        let arr = Object.entries(this.lists)
        arr = arr.sort((a, b) => a[0] - b[0])
          .map(item => item[1])
        return arr
      }
    }
  }

}
</script>

<style >

</style>
