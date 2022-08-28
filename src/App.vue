<script setup>
  import { onAuthStateChanged } from 'firebase/auth'
  import { ref, provide } from 'vue'
  import { useQuasar } from 'quasar'
  import { auth } from './firebase'

  const $q = useQuasar()
  const userGoogle = ref('')

  provide('userGoogle', userGoogle)

  onAuthStateChanged(auth, (user) => {
    userGoogle.value = user
    setTimeout(() => {
      $q.loading.hide()
    }, 1000)    
  })

  $q.loading.show()

</script>

<template>
  <router-view
    v-if="userGoogle !== ''"
  />
</template>

<style>
  body {
    overflow-y: hidden;
  }
</style>