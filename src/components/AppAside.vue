<template>
  <aside class="aside hide__aside">
    <div class="menu" @click="showAside"></div>
    <div class="add-list" >
      <input
        type="text"
        placeholder="Введите название"
        v-model="input"
        v-on:keyup.enter="addList"/>
      <button @click="addList">Создать</button>
    </div>
    <div class="scroll">
      <the-list
        v-for="list in sortList()"
        :key="list.id"
        :list="list"
        @deleteList="deleteList"
        @addTask="addTask"
        @taskEvent="taskEvent">
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
  emits: ['addList', 'deleteList', 'addTask', 'showTask', 'deleteTask'],
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
    deleteList (id) {
      this.$emit('deleteList', id)
    },
    addTask (body, path) {
      this.$emit('addTask', body, path)
    },
    taskEvent (event, idList, idTask) {
      const flag = event === 'deleteTask'
      this.$emit(event, [idList, 'tasks', idTask], flag)
    },
    showAside (e) {
      e.target.parentElement.classList.toggle('hide__aside')
    },
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
