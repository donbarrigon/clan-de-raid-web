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
            <EyeIcon v-if="showPassword" class="w-5 h-5" />
            <EyeOffIcon v-else class="w-5 h-5" />
          </button>
        </div>

        <!-- Message -->
        <div v-if="message" :class="messageClasses">
          <LoaderIcon v-if="loading" class="w-4 h-4 animate-spin" />
          <component v-else :is="messageIcon" class="w-4 h-4 flex-shrink-0" />
          <span class="flex-1">{{ message }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineEmits, defineProps, onMounted } from 'vue'

// Iconos (estos deberían importarse de tu librería de iconos, aquí uso SVG inline)
const EyeIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 010-.639C3.423 7.51 7.36 4.5 12 4.5c4.639 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.639 0-8.573-3.007-9.963-7.178z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" /></svg>`
}

const EyeOffIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M3.98 8.223A10.477 10.477 0 001.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.45 10.45 0 0112 4.5c4.756 0 8.773 3.162 10.065 7.498a10.523 10.523 0 01-4.293 5.774M6.228 6.228L3 3m3.228 3.228l3.65 3.65m7.894 7.894L21 21m-3.228-3.228l-3.65-3.65m0 0a3 3 0 11-4.243-4.243m4.242 4.242L9.88 9.88" /></svg>`
}

const LoaderIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="animate-spin"><path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0l3.181 3.183a8.25 8.25 0 0013.803-3.7M4.031 9.865a8.25 8.25 0 0113.803-3.7l3.181 3.182m0-4.991v4.99" /></svg>`
}

const CheckCircleIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>`
}

const InfoIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" /></svg>`
}

const AlertTriangleIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126zM12 15.75h.007v.008H12v-.008z" /></svg>`
}

const AlertCircleIcon = {
  template: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0zm-9 3.75h.008v.008H12v-.008z" /></svg>`
}

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
    validator: (value) => ['success', 'info', 'warning', 'danger', 'default'].includes(value)
  },
  loading: {
    type: Boolean,
    default: false
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
    lightColor: 'text-green-600',
    darkColor: 'dark:text-green-400',
    lightBg: 'bg-green-50',
    darkBg: 'dark:bg-green-900/20',
    lightBorder: 'border-green-200',
    darkBorder: 'dark:border-green-800'
  },
  info: {
    icon: InfoIcon,
    lightColor: 'text-blue-600',
    darkColor: 'dark:text-blue-400',
    lightBg: 'bg-blue-50',
    darkBg: 'dark:bg-blue-900/20',
    lightBorder: 'border-blue-200',
    darkBorder: 'dark:border-blue-800'
  },
  warning: {
    icon: AlertTriangleIcon,
    lightColor: 'text-yellow-600',
    darkColor: 'dark:text-yellow-400',
    lightBg: 'bg-yellow-50',
    darkBg: 'dark:bg-yellow-900/20',
    lightBorder: 'border-yellow-200',
    darkBorder: 'dark:border-yellow-800'
  },
  danger: {
    icon: AlertCircleIcon,
    lightColor: 'text-red-600',
    darkColor: 'dark:text-red-400',
    lightBg: 'bg-red-50',
    darkBg: 'dark:bg-red-900/20',
    lightBorder: 'border-red-200',
    darkBorder: 'dark:border-red-800'
  },
  default: {
    icon: InfoIcon,
    lightColor: 'text-gray-500',
    darkColor: 'dark:text-gray-400',
    lightBg: 'bg-gray-50',
    darkBg: 'dark:bg-gray-800/50',
    lightBorder: 'border-gray-200',
    darkBorder: 'dark:border-gray-700'
  }
}

const messageConfig = computed(() => messageTypes[props.messageType] || messageTypes.default)
const messageIcon = computed(() => messageConfig.value.icon)

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
    w-full px-3 py-2 border rounded-md shadow-sm
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

const messageClasses = computed(() => {
  const config = messageConfig.value
  return `
    mt-2 px-3 py-2 rounded-md border text-sm flex items-center gap-2
    ${config.lightColor} ${config.darkColor}
    ${config.lightBg} ${config.darkBg}
    ${config.lightBorder} ${config.darkBorder}
  `.replace(/\s+/g, ' ').trim()
})

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
/* Estilos adicionales si son necesarios */
.animate-spin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
