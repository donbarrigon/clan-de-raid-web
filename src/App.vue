<template>
  <app-navbar />
  <main class="w-full min-h-screen pt-24 pb-12 mx-4">
    <RouterView />
  </main>
</template>

<script setup>
  import AppNavbar from './components/layouts/AppNavbar.vue'
</script>

<style scoped>

</style>
