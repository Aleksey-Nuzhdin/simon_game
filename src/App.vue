<template>
  <div id="app">
    <SimonGame />
  </div>
</template>

<script>
import SimonGame from './components/SimonGame.vue'

export default {
  name: 'App',
  components: {
    SimonGame,
  }
}
</script>

<style lang="scss">
*{
  outline:none;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border: none;
  font-family: "Arial",'Lucida Grande', Tahoma, Verdana, sans-serif;
}
body{
  width: 100vw;
  height: 100vh;
}
ul, li{
  list-style: none;
}
#app {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
</style>
