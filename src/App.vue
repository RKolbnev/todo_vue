<template>
  <div>
    <app-header></app-header>
    <div class="main-content">
      <app-aside
        :lists="lists"
        @addList="addItem"
        @addTask="addItem">
      </app-aside>
      <app-main
        :task="activeTask">
      </app-main>
    </div>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppMain from './components/AppMain'
import AppAside from './components/AppAside'

export default {
  data () {
    return {
      link: 'http://todo-vue-e2829-default-rtdb.firebaseio.com/todo.json',
      lists: null,
      activeTask: null
    }
  },
  components: {
    'app-header': AppHeader,
    'app-aside': AppAside,
    'app-main': AppMain
  },
  methods: {
    addItem (value, path = []) {
      // Создаем экземпляр данных
      const body = {
        id: this.idGenerate(),
        title: value
      }
      console.log(this.lists.target)
      // добавляем данные в структуру
      let level = this.lists ?? {}
      for (let i = 0; i < path.length; i++) {
        level = level[i]
      }
      level[body.id] = body
      this.lists = level

      this.sendData(body, path)
    },
    idGenerate () {
      const id = String(Math.random())
        .split('')
        .map(i => {
          if (i === '.') {
            return '_'
          } else {
            return String.fromCharCode(+i + 66)
          }
        })
        .join('')
      return id
    },
    async sendData (body, path) {
      // изменяем путь и отправляем данные на сервер
      path.push(body.id)
      path = path.map(id => `/${id}`).join('')
      const link = this.link.replace(/\.json/, `${path}.json`)

      await fetch(link, {
        method: 'PATCH',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify(body)
      })
    }
  },
  computed: {},
  async beforeMount () {
    const res = await fetch(this.link)
    this.lists = await res.json()
  }
}
</script>

<style>

</style>
