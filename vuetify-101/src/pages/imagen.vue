<template>
  <v-card title="Pagina 1" :text="txt" variant="tonal">
    <v-img :src="URL" height="300px"></v-img>
    <v-card-actions>
      <v-btn prepend-icon="$vuetify" variant="tonal" @click="refresh">Cambiar Imagen</v-btn>
    </v-card-actions>
  </v-card>

</template>

<script setup>
import { ref, onMounted } from 'vue'

const URL = ref("https://picsum.photos/500/300");
const txt = ref("");

async function refresh(){
  let respuesta=await fetch("https://picsum.photos/500/300");
  URL.value=respuesta.url;
}
async function obtenerChisteAleatorio() {
  try {
    const respuesta = await fetch('https://api.chucknorris.io/jokes/random');
    const datos = await respuesta.json();
    return datos.value; // El chiste está dentro del campo 'value' del objeto de respuesta
  } catch (error) {
    console.error('Error al obtener el chiste:', error);
    return null;
  }
}

async function obtenerYAlmacenarChiste() {
  try {
    const chiste = await obtenerChisteAleatorio();
    return chiste;
  } catch (error) {
    console.error('Error:', error);
    return null;
  }
}

async function ejemploDeUso() {
  const chisteObtenido = await obtenerYAlmacenarChiste();
  return chisteObtenido;
}

onMounted(async () => {
  txt.value = await ejemploDeUso();
});
</script>
