<template>
  <div class="min-h-screen bg-white dark:bg-slate-900 transition-colors">
    <div class="max-w-4xl mx-auto p-6">
      <!-- Header con toggle de dark mode -->
      <div class="flex justify-between items-center mb-8">
        <h1 class="text-3xl font-bold text-gray-900 dark:text-white">
          Componente InputText
        </h1>
        <button
          @click="toggleDarkMode"
          class="px-4 py-2 rounded-lg bg-gray-200 dark:bg-slate-700 text-gray-800 dark:text-gray-200 hover:bg-gray-300 dark:hover:bg-slate-600 transition-colors"
        >
          {{ isDark ? '☀️ Light' : '🌙 Dark' }}
        </button>
      </div>

      <div class="space-y-8">
        <!-- Sección: Layout Vertical -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Layout Vertical (por defecto)
          </h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <InputText
              v-model="formData.username"
              label="Nombre de usuario"
              placeholder="Ingresa tu nombre de usuario"
              message="Usuario disponible"
              messageType="success"
              required
            />

            <InputText
              v-model="formData.password"
              password
              label="Contraseña"
              placeholder="Ingresa tu contraseña"
              message="Mínimo 8 caracteres"
              messageType="info"
              required
            />

            <InputText
              v-model="formData.email"
              email
              label="Correo electrónico"
              placeholder="ejemplo@correo.com"
              :message="emailMessage"
              :messageType="emailMessageType"
            />

            <InputText
              v-model="formData.phone"
              tel
              label="Teléfono"
              placeholder="+1 (555) 123-4567"
              message="Verifica el formato del número"
              messageType="warning"
            />
          </div>
        </section>

        <!-- Sección: Layout Horizontal -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Layout Horizontal
          </h2>
          <div class="space-y-4">
            <InputText
              v-model="formData.website"
              url
              label="Sitio web"
              labelPosition="left"
              placeholder="https://ejemplo.com"
              message="URL válida"
              messageType="success"
            />

            <InputText
              v-model="formData.age"
              number
              label="Edad"
              labelPosition="left"
              placeholder="25"
              message="Debe ser mayor de 18"
              messageType="info"
              min="18"
              max="100"
            />
          </div>
        </section>

        <!-- Sección: Tipos de fecha y tiempo -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Fecha y Tiempo
          </h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <InputText
              v-model="formData.birthdate"
              date
              label="Fecha de nacimiento"
              message="Selecciona tu fecha de nacimiento"
              messageType="info"
            />

            <InputText
              v-model="formData.time"
              time
              label="Hora preferida"
              message="Formato 24 horas"
              messageType="default"
            />

            <InputText
              v-model="formData.datetime"
              datetime-local
              label="Fecha y hora"
              message="Fecha y hora local"
              messageType="info"
            />
          </div>
        </section>

        <!-- Sección: Estados especiales -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Estados Especiales
          </h2>
          <div class="space-y-4">
            <InputText
              v-model="formData.loading"
              label="Campo con carga"
              placeholder="Simulando validación..."
              message="Validando información..."
              messageType="info"
              :loading="isLoading"
            />

            <InputText
              modelValue="No se puede editar"
              label="Campo deshabilitado"
              disabled
              message="Este campo está deshabilitado"
              messageType="default"
            />

            <div class="flex gap-4">
              <button
                @click="simulateLoading"
                :disabled="isLoading"
                class="px-4 py-2 bg-blue-600 dark:bg-blue-500 text-white rounded-md hover:bg-blue-700 dark:hover:bg-blue-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
              >
                {{ isLoading ? 'Validando...' : 'Simular Carga' }}
              </button>

              <button
                @click="testValidation"
                class="px-4 py-2 bg-green-600 dark:bg-green-500 text-white rounded-md hover:bg-green-700 dark:hover:bg-green-600 transition-colors"
              >
                Probar Validación Email
              </button>
            </div>
          </div>
        </section>

        <!-- Sección: Ejemplos de código -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Ejemplos de Uso
          </h2>
          <div class="bg-gray-100 dark:bg-slate-800 p-4 rounded-lg">
            <h3 class="font-semibold mb-2 text-gray-900 dark:text-gray-100">
              Código de ejemplo:
            </h3>
            <pre class="text-sm text-gray-700 dark:text-gray-300 overflow-x-auto"><code>&lt;!-- Diferentes tipos de input --&gt;
&lt;InputText v-model="password" password label="Contraseña" /&gt;
&lt;InputText v-model="email" email label="Email" messageType="success" /&gt;
&lt;InputText v-model="phone" tel placeholder="Teléfono" /&gt;
&lt;InputText v-model="website" url labelPosition="left" /&gt;
&lt;InputText v-model="age" number min="0" max="100" /&gt;
&lt;InputText v-model="date" date required /&gt;
&lt;InputText v-model="time" time message="Hora" messageType="info" /&gt;

&lt;!-- Con estados especiales --&gt;
&lt;InputText
  v-model="data"
  :loading="true"
  message="Cargando..."
  messageType="info"
/&gt;</code></pre>
          </div>
        </section>

        <!-- Sección: Valores actuales -->
        <section>
          <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-200 mb-4">
            Valores Actuales (v-model)
          </h2>
          <div class="bg-gray-50 dark:bg-slate-800/50 p-4 rounded-lg">
            <pre class="text-sm text-gray-700 dark:text-gray-300">{{ JSON.stringify(formData, null, 2) }}</pre>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import InputText from '@/components/inputs/InputText.vue'

const isDark = ref(false)
const isLoading = ref(false)

// Datos del formulario
const formData = reactive({
  username: '',
  password: '',
  email: '',
  phone: '',
  website: '',
  age: '',
  birthdate: '',
  time: '',
  datetime: '',
  loading: ''
})

// Validación dinámica del email
const emailMessage = computed(() => {
  if (!formData.email) return ''
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(formData.email)
    ? 'Email válido'
    : 'Formato de email inválido'
})

const emailMessageType = computed(() => {
  if (!formData.email) return 'default'
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(formData.email) ? 'success' : 'danger'
})

// Métodos
const toggleDarkMode = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}

const simulateLoading = async () => {
  isLoading.value = true
  await new Promise(resolve => setTimeout(resolve, 2000))
  isLoading.value = false
}

const testValidation = () => {
  formData.email = formData.email === 'test@example.com'
    ? 'invalid-email'
    : 'test@example.com'
}
</script>

<style>
/* Configuración inicial de dark mode */
.dark {
  color-scheme: dark;
}
</style>
