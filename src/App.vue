<template>
  <div id="app">
<!--    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <photo3D />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import photo3D from './components/photo3D.vue'
export default {
  name: 'App',
  components: {
    photo3D
  }
}
</script>

<style>
    * {
        margin: 0;
        padding: 0;
    }
</style>
