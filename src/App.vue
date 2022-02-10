<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
     
    <router-view></router-view>
<!-- 
     <login-form/> -->

     
  </div>
 
 
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
// import LoginForm from './components/LoginForm.vue'
// import RegisterForm from './components/RegisterForm.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    // RegisterForm,
    // LoginForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
