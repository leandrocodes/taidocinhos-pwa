<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <input type="text" v-model="email" placeholder="email" />
    <input type="text" v-model="password" placeholder="senha" />
    <button @click="login">Login</button>
    <div v-if="loading"><p>carregando...</p></div>
    <div v-if="user">{{ user }}</div>
    <router-view />
  </div>
</template>

<script>
import login from './graphql/login.gql'
export default {
  data: () => ({
    email: '',
    password: '',
    user: null,
    loading: false
  }),
  methods: {
    async login() {
      this.loading = true
      try {
        this.user = await this.$apollo.mutate({
          mutation: login,
          variables: {
            email: this.email,
            password: this.password
          }
        })

        if (this.user) this.loading = false
      } catch (err) {
        this.loading = false
        console.log(err.graphQLErrors)
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
