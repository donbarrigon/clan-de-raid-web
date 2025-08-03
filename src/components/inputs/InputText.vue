<template>
  <div :class="containerClasses">
    <div :class="wrapperClasses">
      <!-- Label -->
      <label v-if="label" :class="labelClasses">
        {{ label }}
        <span v-if="required" class="text-red-500 ml-1">*</span>
      </label>

      <div :class="inputContainerClasses">
        <!-- Input Container -->
        <div class="relative">
          <input
            :id="inputId"
            ref="inputRef"
            :type="inputType"
            :value="modelValue"
            @input="handleInput"
            :placeholder="placeholder"
            :disabled="disabled"
            :required="required"
            :class="inputClasses"
            v-bind="$attrs"
          />

          <!-- Password Toggle Button -->
          <button
            v-if="isPasswordType"
            type="button"
            :class="passwordToggleClasses"
            @click="togglePasswordVisibility"
            :disabled="disabled"
          >
            <EyeIcon v-if="showPassword" class="w-4 h-4" />
            <EyeSlashIcon v-else class="w-4 h-4" />
          </button>
        </div>

        <!-- Message -->
        <div v-if="message" :class="messageClasses">
          <component :is="messageIcon" class="w-4 h-4 flex-shrink-0" />
          <span class="flex-1">{{ message }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineEmits, defineProps, onMounted } from 'vue'
import IconSpinner from '../icons/IconSpinner.vue';
import { EyeIcon, EyeSlashIcon, CheckCircleIcon, InformationCircleIcon, ExclamationTriangleIcon, ExclamationCircleIcon } from '@heroicons/vue/24/solid';

// Props
const props = defineProps({
  // v-model
  modelValue: {
    type: [String, Number],
    default: ''
  },

  // Tipos de input (como atributos booleanos)
  password: {
    type: Boolean,
    default: false
  },
  email: {
    type: Boolean,
    default: false
  },
  tel: {
    type: Boolean,
    default: false
  },
  url: {
    type: Boolean,
    default: false
  },
  number: {
    type: Boolean,
    default: false
  },
  range: {
    type: Boolean,
    default: false
  },
  date: {
    type: Boolean,
    default: false
  },
  time: {
    type: Boolean,
    default: false
  },
  month: {
    type: Boolean,
    default: false
  },
  week: {
    type: Boolean,
    default: false
  },
  datetimeLocal: {
    type: Boolean,
    default: false
  },

  // Props del componente
  type: {
    type: String,
    default: 'text'
  },
  label: {
    type: String,
    default: ''
  },
  message: {
    type: String,
    default: ''
  },
  messageType: {
    type: String,
    default: 'default',
    validator: (value) => ['success', 'info', 'warning', 'danger', 'loading','default'].includes(value)
  },

  // Props adicionales
  placeholder: {
    type: String,
    default: ''
  },
  disabled: {
    type: Boolean,
    default: false
  },
  required: {
    type: Boolean,
    default: false
  },
  labelPosition: {
    type: String,
    default: 'top',
    validator: (value) => ['top', 'left'].includes(value)
  },
  id: {
    type: String,
    default: null
  }
})

// Emits
const emit = defineEmits(['update:modelValue', 'focus', 'blur', 'input'])

// Refs
const inputRef = ref(null)
const showPassword = ref(false)

// Computed
const inputId = computed(() => props.id || `input-${Math.random().toString(36).substr(2, 9)}`)

const isPasswordType = computed(() => props.password)

const inputType = computed(() => {
  if (props.password) return showPassword.value ? 'text' : 'password'
  if (props.email) return 'email'
  if (props.tel) return 'tel'
  if (props.url) return 'url'
  if (props.number) return 'number'
  if (props.range) return 'range'
  if (props.date) return 'date'
  if (props.time) return 'time'
  if (props.month) return 'month'
  if (props.week) return 'week'
  if (props.datetimeLocal) return 'datetime-local'
  return props.type
})

// Configuración de tipos de mensaje con dark mode
const messageTypes = {
  success: {
    icon: CheckCircleIcon,
    classes: 'text-green-600 dark:text-green-400 bg-green-50 dark:bg-green-900/20 border-green-200 dark:border-green-800',
  },
  info: {
    icon: InformationCircleIcon,
    classes: 'text-blue-600 dark:text-blue-400 bg-blue-50 dark:bg-blue-900/20 border-blue-200 dark:border-blue-800',
  },
  warning: {
    icon: ExclamationTriangleIcon,
    classes: 'text-yellow-600 dark:text-yellow-400 bg-yellow-50 dark:bg-yellow-900/20 border-yellow-200 dark:border-yellow-800',
  },
  danger: {
    icon: ExclamationCircleIcon,
    classes: 'text-red-600 dark:text-red-400 bg-red-50 dark:bg-red-900/20 border-red-200 dark:border-red-800',
  },
  loading: {
    icon: IconSpinner,
    classes: 'text-indigo-600 dark:text-indigo-400 bg-indigo-50 dark:bg-indigo-900/20 border-indigo-200 dark:border-indigo-800',
  },
  default: {
    icon: null,
    classes: 'text-gray-500 dark:text-gray-400 bg-gray-50 dark:bg-gray-800/50 border-gray-200 dark:border-gray-700',
  }
}

const messageConfig = computed(() => messageTypes[props.messageType] || messageTypes.default)
const messageIcon = computed(() => messageConfig.value.icon)
const messageClasses = computed(() => {
  const config = messageConfig.value
  return `
    mt-2 px-3 py-2 rounded-md border text-sm flex items-center gap-2
    ${config.classes}
  `.replace(/\s+/g, ' ').trim()
})

// Clases CSS con dark mode
const containerClasses = computed(() => 'w-full')

const wrapperClasses = computed(() => {
  return props.labelPosition === 'left'
    ? 'flex flex-col sm:flex-row sm:items-start gap-1 sm:gap-0'
    : 'block'
})

const labelClasses = computed(() => {
  const baseClasses = 'text-sm font-medium text-gray-700 dark:text-gray-300'
  return props.labelPosition === 'left'
    ? `flex-shrink-0 w-full sm:w-32 md:w-40 ${baseClasses} mb-1 sm:mb-0 sm:mr-4 flex items-center`
    : `block ${baseClasses} mb-2`
})

const inputContainerClasses = computed(() => {
  return props.labelPosition === 'left' ? 'flex-1 min-w-0' : 'w-full'
})

const inputClasses = computed(() => {
  const baseClasses = `
    w-full px-3 py-2 border rounded-md shadow-md
    focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500
    dark:focus:ring-blue-400 dark:focus:border-blue-400
    disabled:cursor-not-allowed
    transition-colors duration-200
    border-gray-300 bg-white text-gray-900
    dark:border-gray-600 dark:bg-slate-800 dark:text-gray-100
    disabled:bg-gray-50 disabled:text-gray-500
    dark:disabled:bg-gray-900 dark:disabled:text-gray-600
    placeholder:text-gray-400 dark:placeholder:text-gray-500
  `.replace(/\s+/g, ' ').trim()

  const passwordPadding = isPasswordType.value ? 'pr-10' : ''
  const disabledOpacity = props.disabled ? 'opacity-50' : ''

  return `${baseClasses} ${passwordPadding} ${disabledOpacity}`
})

const passwordToggleClasses = computed(() => `
  absolute inset-y-0 right-0 pr-3 flex items-center
  text-gray-400 hover:text-gray-600
  dark:text-gray-500 dark:hover:text-gray-300
  focus:outline-none transition-colors
  disabled:opacity-50 disabled:cursor-not-allowed
`.replace(/\s+/g, ' ').trim())

// Methods
const handleInput = (event) => {
  const value = event.target.value
  emit('update:modelValue', value)
  emit('input', event)
}

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value
}

const focus = () => {
  inputRef.value?.focus()
}

const blur = () => {
  inputRef.value?.blur()
}

// Exponer métodos para template refs
defineExpose({
  focus,
  blur,
  inputRef
})

// Manejo de eventos de focus y blur
onMounted(() => {
  if (inputRef.value) {
    inputRef.value.addEventListener('focus', (e) => emit('focus', e))
    inputRef.value.addEventListener('blur', (e) => emit('blur', e))
  }
})
</script>

<script>
// Configuración para herencia de atributos
export default {
  inheritAttrs: false
}
</script>

<style scoped>

</style>
