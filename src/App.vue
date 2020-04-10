<template>
      <div class = "container-fluid">
        <div v-if="!isLogin"> 
          <login-page
            :baseUrl='baseUrl'
            :isLogin='isLogin'
            :user='user'
            :newUser='newUser'
            @getTasks='getTasks'
            @changeLogin='changeLogin'
          ></login-page>
        </div>
        <div v-else>
          <main-page
            :baseUrl='baseUrl'
            :task='task'
            :tasks='tasks'
            :backlogs='backlogs'
            :todos='todos'
            :dones='dones'
            :completeds='completeds'
            @getTasks='getTasks'
            @changeLogin='changeLogin'
          ></main-page>
        </div>
         
    </div>  

</template>

<script>
import LoginPage from './views/LoginPage'
import MainPage from './views/MainPage'

export default {
    name:"App",
    components: {
      LoginPage,
      MainPage
    },
    data() {
        return {
            //localhost
            baseUrl: 'http://localhost:3000',

            //firebase
            // baseUrl: 'https://kanban-server-robin.herokuapp.com',

            isLogin: false,
            user: {
                email: "",
                password: ""
            },
            newUser: {
                email: "",
                password: "",
                confirm: ""
            },
            task: {
                title: "",
                category: ""
            },
            tasks: [],
            backlogs: [],
            todos: [],
            dones: [],
            completeds: []
        }
    },
    methods: {
      changeLogin(status) {
        this.isLogin = status
      },

      getTasks() {
        this.tasks = []
        this.backlogs = []
        this.todos = []
        this.dones = []
        this.completeds = []

        axios({
          method: 'GET',
          url: this.baseUrl + '/tasks',
          headers: {
            access_token: localStorage.token
          }
        })
          .then( result => {
            this.tasks = result.data.result
            console.log('task', this.tasks)
            this.separateCategory()
          })
          .catch( err => {
            console.log(err)
          })
      },
      separateCategory(){
        for( let i = 0; i < this.tasks.length; i++) {
          if(this.tasks[i].category == 'backlog') {
            this.backlogs.push(this.tasks[i])
          } else if (this.tasks[i].category == 'todo') {
            this.todos.push(this.tasks[i])
          } else if (this.tasks[i].category == 'done') {
            this.dones.push(this.tasks[i])
          } else if (this.tasks[i].category == 'completed') {
            this.completeds.push(this.tasks[i])
          }
        }
      }
  },
  created() {
    if(localStorage.token){
      this.isLogin = true
      this.getTasks()
    }
  }
}
</script>

<style>

</style>