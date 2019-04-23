<template>
  <div id="app">
    <h2>Impage Picker Demo</h2>
    <ImagePicker :max="10" :multiple="true"/>
  </div>
</template>

<script>
import ImagePicker from './components/ImagePicker.vue'

export default {
  name: 'App',
  components: {
    ImagePicker
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
