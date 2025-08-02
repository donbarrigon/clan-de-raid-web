<script setup>
import InputButton from '../components/inputs/InputButton.vue'
import InputText from '../components/inputs/InputText.vue'
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const phone = ref('')
</script>

<template>
  <h1>Home</h1>
  <p>This is the home page1</p>
  <p>This is the home page2</p>
  <p>This is the home page3</p>
  <p>This is the home page4</p>
  <input-button primary>primary</input-button><br/>
  <input-button secondary>secondary</input-button><br/>
  <input-button success>success</input-button><br/>
  <input-button save>save</input-button><br/>
  <input-button warning>warning</input-button><br/>
  <input-button cancel>cancel</input-button><br/>
  <input-button edit>edit</input-button><br/>
  <input-button danger>danger</input-button><br/>
  <input-button delete>delete</input-button><br/>
  <input-button info>info</input-button><br/>
  <input-button info loading>loading</input-button><br/>
  <input-button>boton</input-button><br/>
  <input-button save loading>save</input-button><br/>


  <hr />
  <input-text label="Correo" email v-model="email" />
  email: {{ email }}

  <hr />
  <input-text label="Contraseña" password v-model="password" />
  password: {{ password }}

  <hr />
  <InputText
    v-model="phone"
    loading="true"
    label="Teléfono"
    messageType="info"
  />

</template>
