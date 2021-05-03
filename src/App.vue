<template>
  <AuthenticationWall
    :options="options"
    @user="handle_user($event)">

    <h1>Your app</h1>
    <router-view />

  </AuthenticationWall>
</template>

<script>
import AuthenticationWall from '@/AuthenticationWall.vue'
export default {
  name: 'App',
  components: {
    AuthenticationWall
  },
  data: () => ({
    options: {
      login_url: 'https://api.authentication.maximemoreillon.com/login',
      identification_url: 'https://api.authentication.maximemoreillon.com/whoami',
    }
  }),
  methods: {
    handle_user(user) {
      console.log(user)
    }
  }

}
</script>
