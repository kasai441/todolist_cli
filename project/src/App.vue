<template>
  <div id="app">
    <h2>Memo</h2>
    <MemoList/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import MemoList from './components/MemoList.vue'

export default {
  name: 'App',
  components: {
    MemoList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
