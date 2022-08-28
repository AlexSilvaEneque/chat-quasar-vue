<script setup>
  import { inject, nextTick, ref, watchEffect } from 'vue'
  import { collection, query, onSnapshot, orderBy, limit } from 'firebase/firestore'
  import { db, auth } from 'src/firebase';

  const userGoogle = inject('userGoogle')
  const messages = ref([])
  const chatRef = ref(null)

  watchEffect((onCleanup) => {
    if (userGoogle.value) {
      const q = query(collection(db, 'chats'), orderBy('time', 'desc'), limit(10))
      const unsubcribe = onSnapshot(q, async (snapshot) => {
          snapshot.docChanges().forEach((change) => {
            if (change.type === "added") {
              messages.value.push({
                id: change.doc.id,
                ...change.doc.data()
              })                            
            }            
          })
          await nextTick() // TODO: es como un setTimout    
          chatRef.value.scrollTo(0, chatRef.value.scrollHeight)
      })
      onCleanup(unsubcribe) // TODO:
    }    
  })

</script>

<template>
  <q-page v-if="!userGoogle">
    <h3 class="text-center">Debes iniciar sesión</h3>
  </q-page>
  <q-page v-else padding>
    <div class="q-pa-md row justify-center scrollChat" ref="chatRef">
      <div style="width: 100%; max-width: 400px">
        <template
          v-for="(message, index) in messages"
          :key="index"
        >        
          <q-chat-message            
            :text="[ message.text ]"
            :sent="message.uid === auth.currentUser.uid"
            :name="message.displayName"
          />          
        </template>
      </div>
    </div>
  </q-page>
</template>

<style scoped>
  .scrollChat {
    height: calc(100vh - (120px));
    overflow-y: scroll;
  }
</style>