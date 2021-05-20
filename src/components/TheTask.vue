<template>
  <div class="task">
    <span class="task-state"
      @click='checkTask'
    >
      <img
        v-if="!task.state"
        src="../assets/no-check.png"
        alt="">
      <img
        v-else
        src="../assets/check.png"
        alt="">
    </span>
    <span :class="{'task-title': true, 'checked': task.state}"> {{task.title}} </span>
    <span class="task-delete"
      @click="deleteTask"
    > <img src="../assets/close.png" alt=""> </span>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  },
  emits: [
    'deleteTask',
    'checkTask'
  ],
  data () {
    return {
    }
  },
  methods: {
    deleteTask () {
      this.$emit('deleteTask', this.task.id)
    },
    checkTask () {
      this.$emit('checkTask', this.task.id)
    }
  }
}
</script>

<style lang="sass">
.task
  display: flex
  flex-wrap: nowrap
  justify-content: space-between
  align-items: center
  padding: 0 0.5rem
  margin: 0.2rem 0
  &-state img
    display: block
    width: 1rem
  &-title
    flex-basis: 70%
  &-delete img
    display: block
    width: 1.5rem
.checked
  text-decoration: line-through
</style>
