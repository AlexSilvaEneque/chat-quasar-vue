<script setup>
</script>

<template>
  <q-page padding>
    <div class="q-pa-md row justify-center">
        <div style="width: 100%; max-width: 400px">
        <q-chat-message
            name="me"
            avatar="https://cdn.quasar.dev/img/avatar3.jpg"
            stamp="7 minutes ago"
            sent
            text-color="white"
            bg-color="primary"
        >
            <div>
            Hey there!
            </div>

            <div>
            Have you seen Quasar?            
            </div>
        </q-chat-message>

        <q-chat-message
            name="Jane"
            avatar="https://cdn.quasar.dev/img/avatar5.jpg"
            bg-color="amber"
        >
            <q-spinner-dots size="2rem" />
        </q-chat-message>
        </div>
    </div>
  </q-page>
</template>