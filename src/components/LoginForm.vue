<template>
  <form
    class="login_form"
    @submit.prevent="login()">

    <h2>{{options.title}}</h2>

    <div>{{options.hint}}</div>

    <input
      type="text"
      v-model="username"
      placeholder="Username">

    <input
      type="password"
      v-model="password"
      placeholder="Password">

    <input type="submit" value="Login">

    <button
      type="button"
      @click="login()"
      :disabled="processing">
      <span v-if="!processing">Login</span>
      <Loader v-if="processing" />

    </button>

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
  transition: 0.25s;
}

input:focus {
  border-color: #c00000;
  outline: none;
}

input, button{
  margin-top: 2em;
}

form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

label {
  width: 80px;
}

button {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 2em;
  width: 80px;
  border: 1px solid #444444;
  background-color: transparent;
  cursor: pointer;
  transition: 0.25s;
}

button:disabled {
  border-color: #bbbbbb;
  color: #bbbbbb;
}

button:not(:disabled):hover {
  color: #c00000;
  border-color: #c00000;
}

</style>
