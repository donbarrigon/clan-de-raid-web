<template>
  <router-link
    :to="{ name: to }"
    class="relative px-3 py-2 transition-colors duration-200 outline-none"
    :class="{
      'text-[var(--color-secondary)]': isActive,
      'text-white': !isActive,
    }"
    @mouseover="hover = true"
    @mouseleave="hover = false"
    @focus="hover = true"
    @blur="hover = false"
  >
    <slot />
    <span
      class="absolute bottom-0 left-1/2 w-full h-[2px] transform -translate-x-1/2 transition-all duration-300"
      :class="{
        'bg-[var(--color-secondary)] scale-x-100': hover,
        'scale-x-0': !hover,
      }"
    ></span>
  </router-link>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'

const props = defineProps({
  to: {
    type: String,
    required: true
  }
})

const hover = ref(false)
const route = useRoute()

const isActive = computed(() => {
  return route.name === props.to
})
</script>
