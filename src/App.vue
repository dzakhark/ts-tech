<template>
  <div id="app">
    <SortForm />
  </div>
</template>

<script>
import SortForm from './components/SortForm.vue'

export default {
  name: 'app',
  components: {
    SortForm
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  cursor: pointer;
}
</style>
