<template>
  <div id="app">
    <Index />
  </div>
</template>

<script>
import Index from './components/Index.vue'

export default {
  name: 'App',
  components: {
    Index
  }
}
</script>

<style>
*{
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  font-family: Poppins;
}
@font-face {
  font-family: Poppins;
  src: url(assets/fonts/Poppins-Medium.woff);
}
</style>
