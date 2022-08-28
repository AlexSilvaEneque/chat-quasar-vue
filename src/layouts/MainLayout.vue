<script setup>
  import { inject, ref } from 'vue'
  import Navbar from 'src/components/Navbar.vue'
  import Footer from 'src/components/Footer.vue';  

  const userGoogle = inject('userGoogle')
</script>

<template>
  <q-layout view="hHr lpR fFr">

    <Navbar />

    <q-page-container>      
      <router-view />
    </q-page-container>

    <Footer v-if="userGoogle" />
  </q-layout>
</template>