<script setup>
import { reactive, ref } from 'vue'
import {onbeforeMount, onmounted} from 'vue'

const data = reactive({
  catActive: 0,
  cats: [
    { name: 'Garfield', img: '/img/cat1.jpg', clicks: 0 },
    { name: 'Estel', img: '/img/cat2.avif', clicks: 0 },
    { name: 'Cato', img: '/img/cat3.avif', clicks: 0 },
    { name: 'Limpa', img: '/img/cat4.jpg', clicks: 0 }
  ]
})

const visible = ref(false)

function getseleccionatcat(id) {
  data.catActive = id
  data.cats[id].clicks += 1
}

function visiblecat() {
  visible.value = !visible.value
}

onbeforeMount(() => {
  console.log('Component before mount')
})

onmounted(() => {
  console.log('Component mounted')
})
</script>

<template>
  <h1>CatClicker 4 amb Vue</h1>

  <button @click="visiblecat">
    {{ visible ? 'Ocultar gats' : 'Mostrar gats' }}
  </button>

  <ul v-if="visible">
    <li v-for="(cat, index) in data.cats" :key="index">
      <img :src="cat.img" :alt="cat.name" @click="getseleccionatcat(index)" width="200" />
      <p>{{ cat.name }} - clicks: {{ cat.clicks }}</p>
    </li>
  </ul>
</template>

<style scoped>
</style>
