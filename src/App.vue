<template>
  <div id="app">
    <AuthForm />
  </div>
</template>

<script>
import AuthForm from "./components/AuthForm/AuthForm.vue";

export default {
  name: 'App',
  components: {
    AuthForm
  }
}
</script>

<style lang="scss">
@import './styles/main.scss'
</style>
