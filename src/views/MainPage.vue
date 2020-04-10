<template>
  <!-- MAIN PAGE -->
        <div class="container-fluid">
            <header>
                <nav class="navbar fixed-top">
                    <img src="../hacktiv8.png" alt="" width="48" height="48">         
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
                                          <option value="" disabled>Choose Category</option>
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
</template>

<script>
export default {
    name: 'MainPage',
    props: [
        'baseUrl',
        'task',
        'tasks',
        'backlogs',
        'todos',
        'dones',
        'completeds'
    ], 
    methods: {
        logout() {
            localStorage.removeItem('token')
            this.$emit('changeLogin', false)

            var auth2 = gapi.auth2.getAuthInstance()
            auth2.signOut().then(function () {
              console.log('User signed out.')
            })
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
            this.$emit('getTasks')
            $('#addTask').modal('hide')
          })
          .catch( err => {
            console.log(err)
          })
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
            this.$emit('getTasks')
          })
          .catch( err => {
            console.log(err)
          })
      },
      nextCat(id, category) {
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
            this.$emit('getTasks')
            })
            .catch( err => {
              console.log(err)
            })
        }
      },
      previousCat(id, category) {
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
                this.$emit('getTasks')
            })
            .catch( err => {
              console.log(err)
            })
        }
      }

    }
}
</script>

<style>

</style>