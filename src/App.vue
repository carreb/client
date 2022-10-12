<template>
  <div>
    <bombList></bombList>
    <footer class="footer"><p>If you have Champion rank on Wynncraft, please consider contributing to the site by using <a href="https://github.com/carreb/bombbay-conduit/releases">this mod</a>. This site will always remain free.</p></footer>
  </div>
</template>

<script>
import bombList from "./components/bombList.vue"

export default {
  name: 'App',
  components: {
    bombList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #FFF;
  margin-top: 60px;
}

body {
  background-color: #1a1a1a;
}

.footer {
  position: absolute;
  bottom: 5px;
  margin: auto;
  left: 0;
  right: 0;
}
</style>
