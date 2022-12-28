<template>
  <BulletinBoard/>
</template>

<script>

import BulletinBoard from './components/BulletinBoard.vue'
export default {
  name: 'App',
  components: {
    BulletinBoard
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
