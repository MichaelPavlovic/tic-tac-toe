<template>
  <div id="app">
    <Header />
    <Board />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Board from './components/Board.vue';

export default {
  name: 'App',
  components: {
    Header,
    Board
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
}
html, body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
