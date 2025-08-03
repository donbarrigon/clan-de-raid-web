<template>
  <span
    :class="['inline-flex items-center px-2 py-1 shrink-0 w-max rounded-md text-sm font-medium border gap-1 relative', chipClass, customClass]"
  >
    <!-- Ícono a la izquierda -->
    <component
      v-if="resolvedIcon"
      :is="props.icon"
      class="w-4 h-4 shrink-0"
    />

    <!-- Texto del chip -->
    <span>{{ label }}</span>

    <!-- Botón de cerrado -->
    <a
      v-if="closable"
      @click="$emit('remove', { id, label })"
      class="absolute -top-1 -right-1 rounded-full w-4 h-4 flex items-center justify-center text-xs hover:text-lg hover:cursor-pointer leading-none"
    >
      ×
    </a>
  </span>
</template>

<script setup>
import { computed } from 'vue'

// Props
const props = defineProps({
  color: String,
  icon: Object, // permite override del icono por props
  label: { type: String, required: true },
  id: [String, Number],
  closable: Boolean,
  class: { type: String, default: '' }
})

// Lógica para obtener el color
const chipClass = computed(() => {
  const array_colors = ['red', 'blue',  'green', 'yellow', 'indigo',
    // 'purple', 'pink', 'orange', 'teal', 'cyan', 'lime', 'emerald', 'violet', 'fuchsia', 'rose', 'sky',
  ]

  let color = array_colors.find(key => key === props.color) ??
    array_colors[Math.floor(Math.random() * array_colors.length)]

  return `text-${color}-600 dark:text-${color}-400 bg-em bg-${color}-50 dark:bg-${color}-900/20 border-${color}-200 dark:border-${color}-800`
})

const resolvedIcon = computed(() => props.icon ?? null)
const customClass = computed(() => props.class)
</script>
