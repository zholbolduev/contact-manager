<script>
import { RouterView } from 'vue-router'
import NavBar from '../src/components/NavBar.vue'

export default {
  name: 'App',
  components: { RouterView, NavBar }
}
</script>

<template>
  <NavBar />
  <RouterView />
</template>

<style></style>
