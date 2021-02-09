<template>
  <div id="app">
    <Header msg="Welcome to Your Vue.js App"/>
    <Index />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Index from './pages/Index.vue'

export default {
  name: 'App',
  components: {
    Header,
    Index
  }
}
</script>

<style lang="scss">
  @import './styles/main.scss';
</style>
