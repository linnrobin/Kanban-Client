<template>
                <div class="row">
 
                    <div class="col columns bg-transparent border-set">
                        <div class="row title" id="backlog">
                            <h3>Backlog</h3>
                        </div>
                        <div class ="col overflowing">
                        <div class="row content" v-for="task in backlogs" :key="task.id">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)"><</button>
                                        </div>
                                        <div class="col full edit">    
                                            <button class="bg-transparent btn-block" @click.prevent="editTask(task.id, task.title)" data-toggle="modal" data-target="#editTask">Edit</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">></button>
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
                        <div class="row content" v-for="task in todos" :key="task.id">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)"><</button>
                                        </div>
                                        <div class="col full edit">    
                                            <button class="bg-transparent btn-block" @click.prevent="editTask(task.id, task.title)" data-toggle="modal" data-target="#editTask">Edit</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">></button>
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
                        <div class="row content" v-for="task in dones" :key="task.id">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)"><</button>
                                        </div>
                                        <div class="col full edit">    
                                            <button class="bg-transparent btn-block" @click.prevent="editTask(task.id, task.title)" data-toggle="modal" data-target="#editTask">Edit</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">></button>
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
                        <div class="row content" v-for="task in completeds" :key="task.id">
                            <!--START CARD HERE -->
                            <div class="card border-success mb-3" style="width: 100%; max-width: 18rem;">
                                <div class="card-header bg-success border-success">
                                    <div class="row">
                                        <div class="col full previous">
                                            <button class="bg-transparent btn-block" @click.prevent="previousCat(task.id, task.category)"><</button>
                                        </div>
                                        <div class="col full edit">    
                                            <button class="bg-transparent btn-block" @click.prevent="editTask(task.id, task.title)" data-toggle="modal" data-target="#editTask">Edit</button>
                                        </div>
                                        <div class="col full next">    
                                            <button class="bg-transparent btn-block" @click.prevent="nextCat(task.id, task.category)">></button>
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
</template>

<script>
export default {
    name: "MainContent",
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
            this.$toasted.success('Successfully deleted task').goAway(5000)
          })
          .catch( err => {
            console.log(err.response)
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
            this.$toasted.error('Cannot next because already in final state').goAway(5000)  
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
            this.$toasted.success('Task Status Updated').goAway(5000)
            })
            .catch( err => {
              console.log(err.response)
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
          this.$toasted.error('Cannot previous because already in final state').goAway(5000)    
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
                this.$toasted.success('Task Status Updated').goAway(5000)
            })
            .catch( err => {
              console.log(err.response)
            })
        }
      },
      editTask(id, title) {
        this.task.title = title
        this.task.id = id
      }
    }
}
</script>

<style>

</style>