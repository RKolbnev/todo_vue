<template>
  <div class="main">
    <div v-if="info" :id="info.task.id" class="main__1">
      <div class="main-header" >
        <input type="checkbox"
        :checked="info.task.state"
        @click="checkItem(false)">
        <span class="main-header__input"></span>
        <span
          :class="{
            'main-header__text': true,
            'checked': info.task.state
          }">
          {{info.task.title}}
        </span>
        <span class="main-header__delete"
          @click="deleteItem(false)">
          <img src="../assets/basket.png">
          <span>Удалить задачу</span>
        </span>
      </div>
      <div class="main-content">
        <div class="main-checkbox shadow">
          <!-- <div class="steps" v-if="info.task.steps"> -->
          <div class="steps" v-if="haveSteps()">
            <div class="step"
              v-for="step in info.task.steps"
              :key="step.id">
              <input type="checkbox"
                :checked="step.state"
                @click="checkItem(step.id)">
              <span class="step__input"></span>
              <span class="step__title">{{step.title}}</span>
              <span class="step__delete"
                @click="deleteItem(step.id)">
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
            v-html="info.task.note"
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
    info: Object
  },
  emits: ['addStep', 'deleteItem', 'changeNotes', 'checkItem'],
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
    changeNotes (e) { //!
      clearTimeout(this.notesTimeoutId)
      // this.notesTimeoutId = setTimeout(() => {
      this.$emit('changeNotes', e.target.innerHTML)
      // }, 500)
    },

    addStep (value) {
      const body = {
        id: null,
        title: value,
        state: false
      }
      this.$emit('addStep', body, [...this.info.path, 'steps'])
    },
    checkItem (id) {
      const path = []
      if (id) {
        path.push('steps', id)
      }
      this.$emit('checkItem', path)
    },
    deleteItem (id) {
      const path = [...this.info.path]
      if (id) {
        path.push('steps', id)
      }
      this.$emit('deleteItem', path, !id)
    },
    haveSteps () {
      let res
      if (this.info.task?.steps) {
        res = Object.entries(this.info.task?.steps)?.length > 0
      }
      return res
    }
  }
}
</script>

<style></style>
