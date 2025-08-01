<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import AppNavItem from './AppNavItem.vue'

const isFloating = ref(true)
let lastScroll = window.scrollY

const handleScroll = () => {
  const currentScroll = window.scrollY

  if (currentScroll < 100) {
    isFloating.value = true
  } else if (currentScroll < lastScroll && currentScroll > 100) {
    isFloating.value = true
  } else {
    isFloating.value = false
  }

  lastScroll = currentScroll
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <header
    :class="[
      'fixed top-0 left-0 w-full z-50 bg-[var(--color-primary)] transition-all duration-500 ease-in-out transform',
      isFloating
        ? 'translate-y-0 opacity-100 shadow-md'
        : '-translate-y-full opacity-100 pointer-events-none'
    ]"
  >
    <div class=" mx-auto flex items-center justify-between px-4 py-2 text-white">

      <!-- Logo -->
      <div class="flex-shrink-0">
        <img
          v-if="isLargeScreen"
          src="/public/icons/logo.png"
          alt="Logo"
          class="h-8"
        />
        <img
          v-else
          src="/public/icons/logo.png"
          alt="Logo Corto"
          class="h-8"
        />
      </div>

      <!-- Navegación (visible solo en escritorio) -->
      <nav class="hidden md:flex space-x-4 text-xl">
        <AppNavItem to="home">Inicio</AppNavItem>
        <AppNavItem to="about">About</AppNavItem>
        <AppNavItem to="about">Foros</AppNavItem>
        <AppNavItem to="about">Cualquier cosa</AppNavItem>
      </nav>

      <!-- Menú de usuario -->
      <div>
        <!-- Aquí irá el menú de usuario, por ahora solo placeholder -->
        <button class="hover:underline">Usuario</button>
      </div>
    </div>
  </header>
</template>
