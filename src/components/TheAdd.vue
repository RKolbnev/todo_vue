<template>
  <div>
    <div class="add-task"
      v-if="!inputState"
      @click="inputState = !inputState">
      <img :src="require(`../assets/${property[0]}.png`)"/>
      <span>{{property[1]}}</span>
    </div>
    <div class="input-item"
      v-else>
      <input type="text"
        @keyup.enter="addTask"
        v-model="inputValue"/>
      <button
        @click="addTask">
        {{inputValue.length > 0 ? 'Добавить' : 'Закрыть'}}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  emits: ['addTask'],
  props: {
    property: Array
  },
  data () {
    return {
      inputState: false,
      inputValue: ''
    }
  },
  methods: {
    addTask () {
      if (this.inputValue.length > 0) {
        this.$emit('addTask', this.inputValue)
      }
      this.inputState = false
      this.inputValue = ''
    }
  }
}
</script>

<style>

</style>
