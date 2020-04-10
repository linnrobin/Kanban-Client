<template>
      <div class = "container-fluid">
        <!-- LOGIN PAGE -->
        <div v-if="!isLogin" class="container">
                <form class="form-signin" @submit.prevent="login">
                    <div class="text-center mb-4">
                      <img class="mb-4" src="hacktiv8.png" alt="" width="72" height="72">         
                      <h1>Kanban Board</h1>
                    </div>
    
                    <div class="form-label-group">
                      <input v-model="user.email" type="email" id="email" class="form-control" placeholder="Email address" required autofocus>
                      <label for="email">Email address</label>
                    </div>
                
                    <div class="form-label-group">
                      <input v-model="user.password" type="password" id="password" class="form-control" placeholder="Password" required>
                      <label for="password">Password</label>
                    </div>
                
                    <button class="btn btn-lg btn-primary btn-block" type="submit">Login</button>
                </form>
            <div class="g-signin2 form-signin" data-onsuccess="onSignIn"></div>
            <button @click.prevent="logout" class="btn btn-secondary">Logout</button>
            
            <!-- Button trigger modal -->
            <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#register">
                Register
            </button>

              <!-- Modal -->
              <div class="modal fade" id="register" data-backdrop="static" tabindex="-1" role="dialog" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                <div class="modal-dialog" role="document">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="staticBackdropLabel">Register</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      <form class="form-signin" @submit.prevent="register">
                          <div class="form-label-group">
                              <input v-model="newUser.email" type="email" id="registerEmail" class="form-control" placeholder="Email Address" required autofocus>
                              <label for="registerEmail">Email</label>
                          </div>
                          <div class="form-label-group">
                              <input v-model="newUser.password" type="password" id="registerPassword" class="form-control" placeholder="Password" required>
                              <label for="registerPassword">Password</label>
                          </div>
                          <div class="form-label-group">
                              <input v-model="newUser.confirm" type="password" id="registerConfirm" class="form-control" placeholder="Confirm Password" required>
                              <label for="registerConfirm">Confirm Password</label>
                          </div>
                          <button class="btn btn-primary" type="submit">Register</button>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
              <!-- END OF MODAL -->
        </div>
        <!-- END OF LOGIN PAGE   -->

        <!-- MAIN PAGE -->
        <div class="container-fluid" v-else>
            <header>
                <nav class="navbar fixed-top">
                    <img src="hacktiv8.png" alt="" width="48" height="48">         
                    <h1> Kanban Board </h1>
                    <div>
                        <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#addTask">
                            Add Task
                        </button>
            
                          <!-- Modal -->
                          <div class="modal fade" id="addTask" data-backdrop="" tabindex="-1" role="dialog" aria-labelledby="taskLabel" aria-hidden="true">
                            <div class="modal-dialog" role="document">
                              <div class="modal-content">
                                <div class="modal-header">
                                  <h5 class="modal-title" id="taskLabel">Add Task</h5>
                                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                    <span aria-hidden="true">&times;</span>
                                  </button>
                                </div>
                                <div class="modal-body">
                                  <form class="form-signin" @submit.prevent="addTask">
                                      <div class="form-label-group">
                                          <input v-model="task.title" type="text" id="taskTitle" class="form-control" placeholder="Task Title" required autofocus>
                                          <label for="taskTitle">Task Title</label>
                                      </div>

                                      <div class="form-group">
                                        <select v-model="task.category" id="taskCategory" class="custom-select" required>
                                          <option value="">Choose Category</option>
                                          <option value="backlog">Backlog</option>
                                          <option value="todo">To Do</option>
                                          <option value="done">Done </option>
                                          <option value="completed">Completed </option>
                                        </select>
                                      </div>

                                      <button class="btn btn-primary" type="submit">Add Task</button>
                                  </form>
                                </div>
                              </div>
                            </div>
                          </div>
                          <!-- END OF MODAL -->                 
                                
                          <button @click.prevent="logout" class="btn btn-secondary">Logout</button>
                    </div>
                </nav>    
            </header>
            <main class="medium">
                <div class="row">
                    <div class="col columns bg-transparent border-set">
                        <div class="row title" id="backlog">
                            <h3>Backlog</h3>
                        </div>
                        <div class ="col overflowing">
                        <div class="row content" v-for="task in backlogs">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)">Previous</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">Next</button>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-body text-success">
                                  <h5 class="card-title">{{ task.title }}</h5>
                                </div>
                                <div class="card-footer bg-danger border-success">
                                    <button class="bg-transparent btn-block" @click.prevent="destroy(task.id)" >Delete</button>
                                </div>
                            </div>
                            <!-- END OF CARD HERE -->
                        </div>
                        </div>
                    </div>
                    <div class="col columns bg-transparent border-set">
                        <div class="row title" id="todo">
                            <h3>To Do</h3>
                        </div>
                        <div class ="col overflowing">
                        <div class="row content" v-for="task in todos">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)">Previous</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">Next</button>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-body text-success">
                                    <h5 class="card-title">{{ task.title }}</h5>
                                  </div>
                                  <div class="card-footer bg-danger border-success">
                                      <button class="bg-transparent btn-block" @click.prevent="destroy(task.id)" >Delete</button>
                                  </div>
                              </div>
                              <!-- END OF CARD HERE -->                        
                        </div>
                        </div>
                    </div>
                    <div class="col columns bg-transparent border-set">
                        <div class="row title" id="done">
                            <h3>Done</h3>
                        </div>
                        <div class ="col overflowing">
                        <div class="row content" v-for="task in dones">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)">Previous</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">Next</button>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-body text-success">
                                    <h5 class="card-title">{{ task.title }}</h5>
                                  </div>
                                  <div class="card-footer bg-danger border-success">
                                      <button class="bg-transparent btn-block" @click.prevent="destroy(task.id)" >Delete</button>
                                  </div>
                              </div>
                              <!-- END OF CARD HERE -->
                        </div>
                        </div>
                    </div>
                    <div class="col columns bg-transparent border-set">
                        <div class="row title" id="completed">
                            <h3>Completed</h3>
                        </div>
                        <div class ="col overflowing">
                        <div class="row content" v-for="task in completeds">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)">Previous</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">Next</button>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-body text-success">
                                    <h5 class="card-title">{{ task.title }}</h5>
                                  </div>
                                  <div class="card-footer bg-danger border-success">
                                      <button class="bg-transparent btn-block" @click.prevent="destroy(task.id)" >Delete</button>
                                  </div>
                              </div>
                              <!-- END OF CARD HERE -->                        
                        </div>
                        </div>
                    </div>
                </div>
            </main>
            <footer>
                <div class="footer fixed-bottom" id="bottom">
                    <h4>Made By : Robin</h4>
                    <h6>Powered By : Bootstrap</h6>
                </div>
            </footer>
        </div>
        <!-- END OF MAIN PAGE -->    
    </div>  

</template>

<script>
export default {
    data() {
        return {
            //localhost
            baseUrl: 'http://localhost:3000',

            //firebase
            //baseUrl: '',

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
      login() {
          axios({
            method: 'POST',
            url: this.baseUrl + '/users/login',
            data: {
              email: this.user.email,
              password: this.user.password
            }
          })
            .then( result => {
              localStorage.setItem('token', result.data.access_token)
              this.user.email = ''
              this.user.password = ''
              this.isLogin = true    
              this.getTasks()
            })
            .catch( err => {
              console.log(err)
            })
      },
      register() {
        if (this.newUser.password === this.newUser.confirm) {
          axios({
            method: 'POST',
            url: this.baseUrl + '/users/register',
            data: {
              email: this.newUser.email,
              password: this.newUser.password
            }
          })
            .then( result => {
              this.newUser.email = ''
              this.newUser.password = ''
              this.newUser.confirm = ''
              $('#register').modal('hide');
            })
            .catch( err => {
              console.log(err)
            })
        } else {
          console.log('Password and confirm is not the same')
        }
      },
      addTask() {
        axios({
          method: "POST",
          url: this.baseUrl + '/tasks',
          data: {
            title: this.task.title,
            category: this.task.category
          },
          headers: {
            access_token: localStorage.token
          }
        })
          .then( result => {
            this.task.title = ''
            this.task.category = ''
            this.getTasks()
            $('#addTask').modal('hide')
          })
          .catch( err => {
            console.log(err)
          })
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
      },
      destroy(id) {
        axios({
          method: 'DELETE',
          url: this.baseUrl + '/tasks/' + id,
          headers: {
            access_token: localStorage.token
          }
        })
          .then( result => {
            this.getTasks()
          })
          .catch( err => {
            console.log(err)
          })
      },
      nextCat(id, category) {
        console.log('id: ', id)
        console.log('category: ', category)
        let nextCategory = ''

        if (category === 'backlog') {
          nextCategory = 'todo'
        } else if (category === 'todo') {
          nextCategory = 'done'
        } else if (category === 'done') {
          nextCategory = 'completed'
        }

        if (category === 'completed') {
          console.log('Cannot next because already in final state')  
        } else {
          axios({
            method: 'PUT',
            url: this.baseUrl + '/tasks/' + id,
            data: {
              category: nextCategory
            },
            headers: {
              access_token: localStorage.token
            }
          })
            .then( result => {
              this.getTasks()
            })
            .catch( err => {
              console.log(err)
            })
        }
      },
      previousCat(id, category) {
        console.log('id: ', id)
        console.log('category: ', category)
        let previousCategory = ''

        if (category === 'todo') {
          previousCategory = 'backlog'
        } else if (category === 'done') {
          previousCategory = 'todo'
        } else if (category === 'completed') {
          previousCategory = 'done'
        }

        if (category === 'backlog') {
          console.log('Cannot previous because already in final state')  
        } else {
          axios({
            method: 'PUT',
            url: this.baseUrl + '/tasks/' + id,
            data: {
              category: previousCategory
            },
            headers: {
              access_token: localStorage.token
            }
          })
            .then( result => {
              this.getTasks()
            })
            .catch( err => {
              console.log(err)
            })
        }
      },
      onSignIn(googleUser) {
        console.log('masuk sini')
        let id_token = googleUser.getAuthResponse().id_token
        axios({
          method: 'POST',
          url: this.baseUrl + '/users/googleSign',
          data: {
            id_token
          }
        })
          .then( data => {
            localStorage.setItem('token', data.access_token)
            this.user.email = ''
            this.user.password = ''
            this.isLogin = true
          })
          .catch( err => {
            console.log(err.responseJSON, 'err')
          })

      },
      logout() {
        localStorage.removeItem('token')
        this.isLogin = false

        //GOOGLE OAUTH SIGNOUT
        var auth2 = gapi.auth2.getAuthInstance()
        auth2.signOut().then(function () {
          console.log('User signed out.')
        })
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