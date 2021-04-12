<template>
  <form class="login_form" @submit.prevent="login()">

    <div class="">
      <label>Username</label>
      <input type="text" v-model="username">
    </div>

    <div class="">
      <label>Password</label>
      <input type="password" v-model="password">
    </div>

    <div class="">
      <input type="submit" value="Login">

      <button type="button" @click="login()">
        <span v-if="!processing">Login</span>
        <Loader v-if="processing" />

      </button>
    </div>






  </form>
</template>

<script>
/*
This component exchanges credentials for a JWT and manages the storage of the JWT in cookies
*/

import axios from 'axios'
import VueCookies from 'vue-cookies'
import Loader from '@moreillon/vue_loader'

export default {
  name: 'LoginForm',
  props: {
    options: Object,
  },
  //mixins: [StoreMixin],
  components: {
    Loader
  },
  data(){
    return {
      username: '',
      password: '',
      processing: false,
    }
  },
  methods: {
    login(){
      // Send credentials and get JWT

      const url = this.options.login_url
      const body = { username: this.username, password: this.password }

      this.snack = null
      this.processing = true

      axios.post(url, body)
      .then( ({data}) => {
        if(!data.jwt) return
        VueCookies.set('jwt', data.jwt)

        // clear the inputs
        this.username = ''
        this.password = ''

        this.$emit('loggedIn')
      })
      .catch( (error) => {
        if(error.response) alert(error.response.data)
        else alert(`Error while logging in`)
        console.error(error)
       })
      .finally(() => { this.processing = false })

    },

  },

}
</script>

<style scoped>
input[type="submit"] {
  display: none;
}

input {
  padding: 0.25em;
  border: none;
  border-bottom: 1px solid #444444;
}

form > * {
  margin: 1em;
}

form div {
  display: flex;
  justify-content: center;
  align-items: center;
}
form label {
  width: 80px;
}

form button {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 2em;
  width: 80px;
}

</style>
