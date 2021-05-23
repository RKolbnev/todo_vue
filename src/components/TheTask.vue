<template>
  <div class="task">
    <span class="task-state" @click="checkTask">
      <img :src="isCheck" alt="" />
      <!-- <img v-if="!task.state" src="../assets/no-check.png" alt="" /> -->
      <!-- <img v-else src="../assets/check.png" alt="" /> -->
    </span>
    <span :class="{ 'task-title': true, checked: task.state }">
      {{ task.title }}
    </span>
    <span class="task-delete" @click="deleteTask">
      <img src="../assets/close.png" alt="" />
    </span>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  },
  emits: ['deleteTask', 'checkTask'],
  data () {
    return {
      src: ''
    }
  },
  methods: {
    deleteTask () {
      this.$emit('deleteTask', this.task.id)
    },
    checkTask () {
      this.$emit('checkTask', this.task.id)
    }
  },
  computed: {}
}
</script>

<style>

</style>
