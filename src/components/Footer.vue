<script setup>
    import { inject, ref } from 'vue'
    import { collection, addDoc } from 'firebase/firestore'
    import { db, auth } from 'src/firebase';

    const userGoogle = inject('userGoogle')
    const text = ref('')
    const addText = async () => {

        if (!text.value.trim()) {
            // return alert('No envíe cosas en vacías!')
            text.value = ""
            return console.log('no puedes enviar mensajes vacíos')
        }

        await addDoc(collection(db, "chats"), {
            text: text.value,
            uid: auth.currentUser.uid, // TODO: esto no es reactivo, se puede usar la recuperacion de user por inject
            // uid: userGoogle.uid, // TODO: esto no es reactivo, se puede usar la recuperacion de user por inject
            time: Date.now(),
            displayName: auth.currentUser.displayName
        })
        .then(() => {
            // agregar un sonido de enviado
            text.value = ""
        })
        .catch(err => console.log(err))
    }
</script>

<template>
    <q-footer elevated class="bg-grey-8 text-white">
      <q-toolbar>
        <q-input
            dark
            dense
            standout
            label="Ingrese texto"
            v-model="text"
            class="full-width"
            @keyup.enter="addText"
        >
            <template v-slot:append>
                <q-icon
                    name="send"
                    class="cursor-pointer"
                    @click="addText"
                />
            </template>
        </q-input>
      </q-toolbar>
    </q-footer>
</template>