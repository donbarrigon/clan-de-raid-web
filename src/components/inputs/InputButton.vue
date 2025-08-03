<template>
  <component
    :is="href ? 'a' : 'button'"
    :href="href"
    :class="buttonClasses"
    class="inline-flex items-center gap-2 rounded-xl px-4 py-3 text-white font-semibold transition-colors duration-200 focus:outline-none disabled:opacity-50"
    :disabled="loading"
  >
    <!-- Iconos -->
    <template v-if="loading">
      <icon-spinner />
    </template>
    <template v-else-if="hasIconSlot">
      <slot name="icon" />
    </template>
    <template v-else>
      <component
        :is="defaultIcon"
        class="w-4 h-4"
      />
    </template>

    <!-- Contenido del botón -->
    <slot />
  </component>
</template>

<script setup>
import { computed, useSlots } from 'vue'
import { CheckIcon, PencilIcon, XMarkIcon, TrashIcon } from '@heroicons/vue/24/solid'
import IconSpinner from '../icons/IconSpinner.vue'

const props = defineProps({
  href: String,
  loading: Boolean,

  primary: Boolean,
  secondary: Boolean,
  warning: Boolean,
  danger: Boolean,
  info: Boolean,
  success: Boolean,
  save: Boolean,
  edit: Boolean,
  cancel: Boolean,
  delete: Boolean,
})

const slots = useSlots()

const hasIconSlot = computed(() => !!slots.icon)

const defaultIcon = computed(() => {
  if (props.save) return CheckIcon
  if (props.edit) return PencilIcon
  if (props.cancel) return XMarkIcon
  if (props.delete) return TrashIcon
  return null
})

const buttonClasses = computed(() => {
  const base = []

  const map = {
  primary: 'bg-[var(--color-primary)] hover:bg-[var(--color-primary-hover)] active:bg-[var(--color-primary-active)]',
  secondary: 'bg-slate-50 hover:bg-slate-200 active:bg-slate-300 text-gray-800 hover:text-gray-900 active:text-black dark:bg-slate-800 dark:hover:bg-slate-900 dark:active:bg-slate-950 dark:text-gray-100 dark:hover:text-white dark:active:text-white border-solid border-2 border-[var(--color-secondary)] transition-colors duration-300',
  success: 'bg-emerald-600 hover:bg-emerald-700 active:bg-emerald-800',
  save: 'bg-emerald-600 hover:bg-emerald-700 active:bg-emerald-800',
  warning: 'bg-yellow-500 hover:bg-yellow-600 active:bg-yellow-700',
  cancel: 'bg-gray-500 hover:bg-gray-600 active:bg-gray-700',
  danger: 'bg-rose-600 hover:bg-rose-700 active:bg-rose-800',
  delete: 'bg-rose-600 hover:bg-rose-700 active:bg-rose-800',
  info: 'bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800',
  edit: 'bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800',
}

  for (const key in map) {
    if (props[key]) {
      base.push(map[key])
    }
  }

  if (base.length === 0) {
    base.push('bg-gray-400 hover:bg-gray-500')
  }

  return base.join(' ')
})
</script>

