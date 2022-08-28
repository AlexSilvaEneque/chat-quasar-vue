<script setup>
    import { inject, ref } from 'vue'
    import { GoogleAuthProvider, signInWithPopup, signOut } from 'firebase/auth'
    import { auth } from 'src/firebase'
    import InfoUser from './InfoUser.vue'

    const rightDrawerOpen = ref(false)
    
    const userGoogle = inject('userGoogle')

    const toggleRightDrawer =  () => {
        rightDrawerOpen.value = !rightDrawerOpen.value
    }
    
    const accessGoogle = () => {
        const provider = new GoogleAuthProvider()    
        signInWithPopup(auth, provider)
          .catch(err => console.log(err))
    }

    const logoutGoogle = () => {
        signOut(auth)
          .catch(err => console.log(err))
    }

</script>

<template>
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          Chat with Firebase, Vue3 and Quasar
        </q-toolbar-title>

        <q-btn
          v-if="!userGoogle"
          label="Ingresar"
          color="secondary"
          @click="accessGoogle"
        />

        <q-btn
          v-if="userGoogle"
          label="Salir"
          color="deep-orange"
          @click="logoutGoogle"
        />

        <q-btn
          v-if="userGoogle"
          dense flat round
          icon="menu"
          @click="toggleRightDrawer"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      show-if-above
      v-model="rightDrawerOpen"
      side="right"
      bordered
      v-if="userGoogle"
    >
      <InfoUser />
    </q-drawer>
</template>