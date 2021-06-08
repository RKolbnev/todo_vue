  <template>
  <div>
    <app-header></app-header>
    <div class="main-content">
      <app-aside
        :lists="lists"
        @addList="addItem"
        @addTask="addItem"
        @showTask="showTask"
        @checkTask="checkTask"
        @deleteTask="deleteTask"
        >
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
    addItem (body, path = []) {
      // Правим экземпляр данных
      body.id = Date.now()
      path.push(body.id)

      // добавляем данные в структуру
      this.lists ??= {}
      if (path.length === 1) this.lists[body.id] = body
      if (path.length > 2) {
        if (!this.lists[path[0]]?.tasks) {
          this.lists[path[0]].tasks = {}
        }
        this.lists[path[0]].tasks[body.id] = body
      }

      // Отправляем данные
      this.sendData(body, path)
    },
    checkTask (idList, idTask) {
      this.lists[idList].tasks[idTask].state = !this.lists[idList].tasks[idTask].state
      this.sendData({
        state: this.lists[idList].tasks[idTask].state
      }, [idList, 'tasks', idTask])
    },
    showTask (idList, idTask) {
      this.activeTask = this.lists[idList].tasks[idTask]
    },
    deleteTask (idList, idTask) {
      this.deleteData([idList, 'tasks', idTask])
      delete this.lists[idList].tasks[idTask]
    },
    async sendData (body, path) {
      // изменяем путь и отправляем данные на сервер
      path = path.map(id => `/${id}`).join('')
      const link = this.link.replace(/\.json/, `${path}.json`)

      await fetch(link, {
        method: 'PATCH',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify(body)
      })
    },
    async deleteData (path) {
      path = path.map((id) => `/${id}`).join('')
      const link = this.link.replace(/\.json/, `/${path}.json`)

      const res = await fetch(link, { method: 'DELETE' })
      if (res.ok) console.log('Task is delete')
    }
  },
  async beforeMount () {
    const res = await fetch(this.link)
    this.lists = await res.json()
  }
}
</script>

<style>

</style>
