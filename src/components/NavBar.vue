<script>
import { RouterLink } from 'vue-router'

export default {
  name: 'NavBar',
  components: { RouterLink }
}
</script>

<template>
  <nav class="navbar navbar-dark bg-success navbar-expand-sm">
    <div class="container">
      <RouterLink to="/" class="navbar-brand">
        <i class="fa fa-mobile"></i>
        Vue Contact Manager</RouterLink
      >
    </div>
  </nav>
</template>

<style></style>
