<template>
  <div class="task"
    v-if="!deleteBtn"
    @click.right.prevent="deleteBtn = !deleteBtn"
  >
    <div>
      <input type="checkbox" :id="task.id" :checked="task.state">
      <span></span>
    </div>
    <span
      :class="{
        'task-text': true,
        'checked': task.state
      }">
      {{task.title}}
    </span>
  </div>
  <div v-else
    class="delete__button"
    @click.right.prevent="deleteBtn = !deleteBtn"
  >
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
