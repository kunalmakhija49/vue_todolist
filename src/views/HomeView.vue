<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <Todoapp/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Todoapp from '@/components/Todoapp.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    Todoapp
  }
}
</script>
