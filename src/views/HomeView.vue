<script setup>
import InputButton from '../components/inputs/InputButton.vue'
import InputChip from '@/components/inputs/InputChip.vue'

</script>

<template>
  <h1>Home</h1>
  <p>This is the home page</p>
  <input-chip
    label="Vue.js"
    id="1"
    color="blue"
    closable
    @remove="(chip) => console.log('Removido:' + chip.id + chip.label)"
  />

  <input-chip
    label="Cargando"
    color="red"
    class="uppercase"
  />

  <input-chip
    label="Aleatorio1"
  />
  <input-chip
    label="Aleatorio2"
  />
  <input-chip
    label="Aleatorio3"
  />
  <br/><br/>
  <input-button primary>primary</input-button><br/><br/>
  <input-button secondary>secondary</input-button><br/><br/>
  <input-button success>success</input-button><br/><br/>
  <input-button save>save</input-button><br/><br/>
  <input-button warning>warning</input-button><br/><br/>
  <input-button cancel>cancel</input-button><br/><br/>
  <input-button edit>edit</input-button><br/><br/>
  <input-button danger>danger</input-button><br/><br/>
  <input-button delete>delete</input-button><br/><br/>
  <input-button info>info</input-button><br/><br/>
  <input-button info loading>loading</input-button><br/><br/>
  <input-button>boton</input-button><br/><br/>
  <input-button save loading>save</input-button><br/><br/>




</template>
