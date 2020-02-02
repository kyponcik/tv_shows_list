<template>
  <div id="app">
   
    <shows-list></shows-list>
      
  </div>

</template>

<script>
import ShowsList from './components/ShowsList.vue'

export default {
  name: 'app',
  components:{
    ShowsList
   
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
   }
</style>
