<template>
  <!-- LOGIN PAGE -->
        <div class="container">
                <form class="form-signin" @submit.prevent="login">
                    <div class="text-center mb-4">
                      <img class="mb-4" src="../hacktiv8.png" alt="" width="72" height="72">         
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
            <div class="row" style="justify-content: center;">
              <div id="google-signin-button" class="m-3"></div>
              
              <!-- Button trigger modal -->
              <button type="button" class="btn btn-primary m-3" data-toggle="modal" data-target="#register">
                  Register
              </button>

            </div>

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
</template>

<script>
export default {
    name: 'LoginPage',
    props: [
      'baseUrl',
      'isLogin',
      'user',
      'newUser'
    ],
    data() {
      return {
        dismissSecs: 10,
        dismissCountDown: 0,
        showDismissibleAlert: false
      }
    },
    methods: {
      countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown
      },
      showAlert() {
        this.dismissCountDown = this.dismissSecs
      },

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
              this.$emit('changeLogin', true)    
              this.$emit('getTasks')
              this.$toasted.success('Successfully logged in').goAway(5000)
            })
            .catch( err => {
              this.$toasted.error(err.response.data.errors[0].message).goAway(5000)
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
              this.$toasted.success('Successfully register, please login').goAway(5000)
            })
            .catch( err => {
              console.log(err.response.data.errors[0].message)
              this.$toasted.error(err.response.data.errors[0].message).goAway(5000)
            })
        } else {
          this.$toasted.error('Password and confirm is not the same').goAway(5000)
        }
      },
      onSignIn(googleUser) {
        let id_token = googleUser.getAuthResponse().id_token
        axios({
          method: 'POST',
          url: this.baseUrl + '/users/googleSign',
          data: {
            id_token
          }
        })
          .then( result => {
            localStorage.setItem('token', result.data.access_token)
            this.user.email = ''
            this.user.password = ''
            this.$emit('changeLogin', true) 
            this.$emit('getTasks')
            this.$toasted.success('Successfully logged in').goAway(5000)
          })
          .catch( err => {
            console.log(err.responseJSON, 'err')
          })
      }
    },
    mounted() {
      gapi.signin2.render('google-signin-button', {
      onsuccess: this.onSignIn
    })
  }
}
</script>

<style>

</style>