<template>
  <div id="app">
      <!-- <h1>− −− − −• − •• • •• − − •• − •• • − •• −−− − −• − •</h1> -->
      <H1>SVG MORSE GENERATOR</H1>
    <generator />
  </div>
</template>

<script>
import generator from './components/generator.vue'

export default {
  name: 'app',
  components: {
    generator
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=IBM+Plex+Mono');
</style>


</style>

<style lang="less" scoped>

#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-family: 'IBM Plex Mono', monospace;
   color: #2c3e50;

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;

  margin-top: 10px;
  padding:0 5vw;
  box-sizing: border-box;

  h1{
      font-weight:100;
      font-size:16px;
      margin:0;
      text-align:right;
  }



}

body{
    margin:0;
    padding:0;

}

</style>
