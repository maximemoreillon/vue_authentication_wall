<template>
  <AuthenticationWall :options="options">

    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>

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

}
</script>

<style>

</style>
