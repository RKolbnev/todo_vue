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
        v-for="list in lists"
        :key="list.id"
        :list="list"
        @addTask="addTask">
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
  emits: ['addList', 'addTask'],
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
      this.$emit('addList', this.input)
      this.input = ''
    },
    addTask (value, path) {
      this.$emit('addTask', value, path)
      console.log(value, path)
    }
  }

}
</script>

<style >

</style>
