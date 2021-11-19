<template>
  <Header />
  <router-view/>
</template>

<script>
import Header from '@/components/layout/Header.vue'

export default {
  name: 'App',
  components: {
    Header
  }
}
</script>

<style lang="scss">
body{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background: #eee;
}
</style>
