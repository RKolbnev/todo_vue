<template>
  <div class="header">
    <div class="logo">
      <img src="../assets/todo-logo.png" alt="">
    </div>
    <div class="search">
      <div class="search__wrap">
        <input type="text" v-model="searchValue" v-on:keyup.enter="search">
        <button class="btn" @click="search">
          Найти
        </button>
      </div>
      <div class="search__popup scroll" v-if="show">
        <div v-if="!data" class="preloader">
          <img src="../assets/Rocket.gif" alt="">
        </div>
        <div
          class="search__result"
          v-else
          v-for="(item, i) in parseData"
          :key="item"
          @click="showItem(i)">
          <span>{{item.length === 2 ? item[0] : ''}}</span>
          <span>{{item.length > 3 ? item : item[1]}}</span>
        </div>
        <button @click="show=false">Закрыть</button>
      </div>
    </div>
    <div class="corner">
      <div class="corner__moon">
        <img class='moon' src="../assets/moon.png" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: Array
  },
  emits: ['search', 'showSearchRes'],
  data () {
    return {
      searchValue: '',
      show: false,
      searchItems: {
        tasks: 'Задача',
        steps: 'Шаг',
        note: 'Заметки'
      }
    }
  },
  methods: {
    search () {
      if (this.searchValue.length > 0) {
        this.$emit('search', this.searchValue)
        this.searchValue = ''
        this.show = true
      }
    },
    showItem (i) {
      this.$emit('showSearchRes', this.data[i])
      this.show = false
    }
  },
  computed: {
    parseData () {
      const res = {}
      for (let i = 0; i <= this.data.length - 1; i++) {
        const len = this.data[i].length - 1
        let key
        if (this.data[i][len - 1] === 'title') {
          key = this.searchItems[this.data[i][len - 3]] ?? 'Список'
        } else {
          key = this.searchItems[this.data[i][len - 1]]
        }
        const value = this.data[i][len]
        if (key && value) {
          res[i] = [key, value]
        }
      }
      return Object.entries(res).length > 0 ? res : ['Поиск не дал результатов']
    }
  }
}
</script>

<style>
</style>
