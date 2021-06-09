<template>
  <div class="main">
    <div v-if="task" :id="task.task.id" class="main__1">
      <div class="main-header" >
        <input type="checkbox"
        :checked="task.task.state"
        @click="checkTask">
        <span class="main-header__input"></span>
        <span
          :class="{
            'main-header__text': true,
            'checked': task.task.state
          }">
          {{task.task.title}}
        </span>
        <span class="main-header__delete"
          @click="deleteTask">
          <img src="../assets/basket.png">
          <span>Удалить задачу</span>
        </span>
      </div>
      <div class="main-content">
        <div class="main-checkbox shadow">
          <div class="steps" v-if="task.task.steps">
            <div class="step"
              v-for="step in task.task.steps"
              :key="step.id">
              <input type="checkbox"
                :checked="step.state"
                @click="checkStep(step.id)">
              <span class="step__input"></span>
              <span class="step__title">{{step.title}}</span>
              <span class="step__delete"
                @click="deleteStep(step.id)">
                &times;
              </span>
            </div>
          </div>
          <the-add
            :property="addSettings"
            @addTask="addStep">
          </the-add>
        </div>
        <div class="main-notes shadow">
          <div class="main-notes__header">
            <img src="../assets/notes.png" alt="">
            <span>Заметки</span>
          </div>
          <div
            contenteditable="true"
            class="main-notes__content"
            v-html="task.task.note"
            @input="changeNotes">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TheAdd from './TheAdd'

export default {
  props: {
    task: Object
  },
  emits: ['changeNotes', 'checkTask', 'deleteTask', 'addStep', 'checkStep', 'deleteStep'],
  components: {
    'the-add': TheAdd
  },
  data () {
    return {
      notes: '',
      addSettings: {
        logo: 'plus',
        text: 'Добавить шаг',
        buttonClass: 'add-step'
      },
      notesTimeoutId: null
    }
  },
  methods: {
    changeNotes (e) {
      clearTimeout(this.notesTimeoutId)
      this.notesTimeoutId = setTimeout(() => {
        this.$emit('changeNotes', e.target.innerHTML)
      }, 3000)
    },
    addStep (value) {
      const body = {
        id: null,
        title: value,
        state: false
      }
      this.$emit('addStep', body)
    },
    checkTask () {
      this.$emit('checkTask')
    },
    deleteTask () {
      this.$emit('deleteTask')
    },
    checkStep (id) {
      this.$emit('checkStep', id)
    },
    deleteStep (id) {
      this.$emit('deleteStep', id)
    }
  }
}
</script>

<style></style>
