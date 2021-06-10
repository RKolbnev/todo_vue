  <template>
  <div>
    <app-header></app-header>
    <div class="main-content">
      <app-aside
        :lists="lists"
        @addList="addItem"
        @addTask="addItem"
        @deleteList="deleteItem"
        @deleteTask="deleteItem"
        @showTask="showTask">
      </app-aside>
      <app-main
        :info="activeTask"
        @addStep="addItem"
        @checkItem="checkItem"
        @deleteItem="deleteItem"
        @changeNotes="changeNotes">
      </app-main>
      <app-form></app-form>
    </div>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppMain from './components/AppMain'
import AppAside from './components/AppAside'
import AppForm from './components/AppForm'

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
    'app-main': AppMain,
    'app-form': AppForm
  },
  methods: {
    addItem (body, path = []) {
      // Правим экземпляр данных
      body.id = Date.now()

      // добавляем данные в структуру
      const len = path.length - 1
      this.lists ??= {}
      let item = this.lists

      for (let i = 0; i <= len - 1; i++) {
        item = item[path[i]]
      }
      if (path.length > 1) {
        item[path[len]] = item[path[len]] ?? {}
        item = item[path[len]]
      }
      item[body.id] = body

      // Отправляем данные
      path.push(body.id)
      this.sendData(body, path)
    },
    deleteItem (path, flag) {
      let item = this.lists
      for (let i = 0; i <= path.length - 2; i++) {
        item = item[path[i]]
      }

      delete item[path[path.length - 1]]
      this.deleteData(path)

      if (flag && this.activeTask?.task?.id === path[path.length - 1]) {
        this.activeTask = null
      }
    },
    checkItem (path) {
      let item = this.activeTask.task
      if (path.length === 0) {
        item.state = !item.state
      } else {
        item = item[path[0]][path[1]]
        item.state = !item.state
      }
      this.sendData({ state: item.state }, [...this.activeTask.path, ...path])
    },
    showTask (path) {
      const active = {
        path,
        task: this.lists[path[0]][path[1]][path[2]]
      }
      this.activeTask = active
    },

    changeNotes (value) { //! изменить работу функции (не стоит добавать html)
      this.activeTask.task.note = value
      this.sendData({ note: value }, this.activeTask.path)
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
      console.log('Send')
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
