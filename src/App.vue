<template>
  <div>
    <app-header></app-header>
    <div class="main-content">
      <app-aside
        :folders="folders"
        @openFolder="openFolder"
        @addFolder="addFolder"
        @addTask="addTask"
        @checkTask="checkTask"
        @deleteTask="deleteTask"
      ></app-aside>
      <app-main></app-main>
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
      folders: [
        {
          title: 'Test folder 1',
          id: 1,
          tasks: [
            {
              title: 'Task name 1',
              id: 1,
              state: false,
              notes: 'alskdl kansdlk asdk a;sld a',
              checklist: [
                {
                  desc: 'lkajsdlkjasd',
                  state: false
                },
                {
                  desc: 'asdl asdl s',
                  state: true
                }
              ]
            },
            {
              title: 'Task name 2',
              id: 2,
              state: false,
              notes: 'alskdl kansdlk asdk a;sld a',
              checklist: [
                {
                  desc: 'lkajsdlkjasd',
                  state: false
                },
                {
                  desc: 'asdl asdl s',
                  state: true
                }
              ]
            }
          ],
          isOpen: false
        },
        {
          title: 'Folder2',
          id: 2,
          tasks: [],
          isOpen: false
        }
      ]
    }
  },
  components: {
    'app-header': AppHeader,
    'app-aside': AppAside,
    'app-main': AppMain
  },
  methods: {
    openFolder (id) {
      this.folders.forEach((folder, i) => {
        if (folder.id === id) {
          this.folders[i].isOpen = !this.folders[i].isOpen
        }
      })
    },
    addFolder (newFolder) {
      this.folders.push(newFolder)
    },
    addTask (folderId, newTask) {
      this.folders.forEach((folder, i) => {
        if (folder.id === folderId) {
          this.folders[i].tasks.push(newTask)
        }
      })
    },
    checkTask (folderId, taskId) {
      this.folders.forEach((folder, idx) => {
        if (folder.id === folderId) {
          folder.tasks.forEach((task, i) => {
            if (task.id === taskId) {
              this.folders[idx].tasks[i].state = !this.folders[idx].tasks[i].state
            }
          })
        }
      })
    },
    deleteTask (folderId, taskId) {
      this.folders.forEach((folder, idx) => {
        if (folder.id === folderId) {
          folder.tasks.forEach((task, i) => {
            if (task.id === taskId) {
              this.folders[idx].tasks.splice(i, 1)
            }
          })
        }
      })
    }
  }
}
</script>

<style lang="sass">
body
  margin: 0
  padding: 0
  font-size: 14px
  font-family: Arial, Helvetica, sans-serif
  font-weight: normal
  box-sizing: border-box
.main-content
  display: flex
  flex-direction: row
  flex-wrap: nowrap
</style>
