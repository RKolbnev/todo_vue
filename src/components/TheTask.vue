<template>
  <div class="task"
    v-if="!deleteBtn"
    @click.right.prevent="deleteBtn = !deleteBtn">
    <img src="../assets/turn-arrow.png" alt="">
    <span :class="{'task-text': true, 'checked': task.state}">
      {{task.title}}
    </span>
  </div>
  <div class="delete__button"
    v-else
    @click.right.prevent="deleteBtn = !deleteBtn">
    <button>Удалить задачу</button>
  </div>
</template>

<script>
export default {
  props: {
    task: Object
  },
  data () {
    return {
      deleteBtn: false
    }
  }
}
</script>

<style>

</style>
