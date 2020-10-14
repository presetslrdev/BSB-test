<template>
  <div id="app">
    <Menu/>
    <div class="uk-container">
      <router-view/>
    </div>
  </div>
</template>

<script>
    import Menu from '@/components/menu'

    export default {
        name: 'App',
        components: {
            Menu,
        }
    }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
  }
</style>