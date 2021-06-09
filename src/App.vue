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
        :task="activeTask"
        @checkTask="checkTask(activeTask.path[0], activeTask.path[2])"
        @deleteTask="deleteTask(activeTask.path[0], activeTask.path[2])"
        @addStep="addStep"
        @changeNotes="sendChanges"
        @checkStep="checkStep"
        @deleteStep="deleteStep"
        >
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
    addItem (body, path = []) { //! Оптимизировать для отправки всех видов
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
    showTask (idList, idTask) { //! Подумать над улучшением
      const active = {
        path: [idList, 'tasks', idTask],
        parentName: this.lists[idList].title,
        task: this.lists[idList].tasks[idTask]
      }
      this.activeTask = active
    },
    checkTask (idList, idTask) { //! Оптимизировать для шагов
      this.lists[idList].tasks[idTask].state = !this.lists[idList].tasks[idTask].state
      this.sendData({
        state: this.lists[idList].tasks[idTask].state
      }, [idList, 'tasks', idTask])
    },
    deleteTask (idList, idTask) { //! Оптимизировать для шагов
      this.deleteData([idList, 'tasks', idTask])
      delete this.lists[idList].tasks[idTask]
      this.activeTask = null
    },
    sendChanges (value) { //! Сменить название и изменить работу функции (не стоит добавать html)
      this.activeTask.task.note = value
      this.sendData({ note: value }, this.activeTask.path)
    },
    addStep (body) { //! Объеденить с добавлением задач
      body.id = Date.now()
      if (!this.activeTask.task?.steps) {
        this.activeTask.task.steps = {}
      }
      this.activeTask.task.steps[body.id] = body
      this.sendData({ steps: this.activeTask.task.steps }, this.activeTask.path)
    },
    checkStep (id) { //! Объеденить с добавлением задач
      this.activeTask.task.steps[id].state = !this.activeTask.task.steps[id].state
      this.sendData({ steps: this.activeTask.task.steps }, this.activeTask.path)
    },
    deleteStep (id) { //! Объеденить с добавлением задач
      delete this.activeTask.task.steps[id]
      this.deleteData([...this.activeTask.path, 'steps', id])
      if (Object.keys(this.activeTask.task.steps).length < 1) {
        this.activeTask.task.steps = null
      }
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
