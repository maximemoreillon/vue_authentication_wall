# Vue authentication wall

## Usage
```vue
<template>
  <AuthenticationWall
    :options="options"
    @user="handle_user($event)">

    <h1>Your app</h1>
    <router-view />

  </AuthenticationWall>
</template>

<script>
import AuthenticationWall from '@moreillon/vue_authentication_wall'
export default {
  name: 'App',
  components: {
    AuthenticationWall
  },
  data: () => ({
    options: {
      login_url: 'https://api.authentication.example.com/login',
      identification_url: 'https://api.authentication.example.com/whoami',
    }
  }),
  methods: {
    handle_user(user) {
      console.log(user)
    }
  }

}
</script>
```
