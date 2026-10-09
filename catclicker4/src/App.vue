<script setup>
import { computed, reactive, ref, onBeforeMount, onMounted } from 'vue'

const cats = reactive([
  { name: 'Garfield', img: '/img/cat1.jpg', clicks: 0 },
  { name: 'Estel', img: '/img/cat2.avif', clicks: 0 },
  { name: 'Cato', img: '/img/cat3.avif', clicks: 0 },
  { name: 'Limpa', img: '/img/cat4.jpg', clicks: 0 }
])

const selectedCatIndex = ref(0)
const adminVisible = ref(false)

const selectedCat = computed(() => cats[selectedCatIndex.value])

const adminForm = reactive({
  name: '',
  img: '',
  clicks: 0
})

function selectCat(index) {
  selectedCatIndex.value = index
  adminVisible.value = false
}

function incrementClicks() {
  selectedCat.value.clicks += 1
}

function openAdmin() {
  const currentCat = selectedCat.value
  adminForm.name = currentCat.name
  adminForm.img = currentCat.img
  adminForm.clicks = currentCat.clicks
  adminVisible.value = true
}

function closeAdmin() {
  adminVisible.value = false
}

function saveCatChanges() {
  const currentCat = selectedCat.value
  currentCat.name = adminForm.name.trim() || currentCat.name
  currentCat.img = adminForm.img.trim() || currentCat.img
  currentCat.clicks = Number(adminForm.clicks) || 0
  adminVisible.value = false
}

onBeforeMount(() => {
  console.log('Component before mount')
})

onMounted(() => {
  console.log('Component mounted')
})
</script>

<template>
  <div class="app-shell">
    <h1>CatClicker 4</h1>

    <div class="layout">
      <aside class="sidebar">
        <h2>Cat names</h2>
        <ul class="cat-list">
          <li v-for="(cat, index) in cats" :key="cat.name">
            <button
              type="button"
              class="cat-button"
              :class="{ active: selectedCatIndex === index }"
              @click="selectCat(index)"
            >
              {{ cat.name }}
            </button>
          </li>
        </ul>
      </aside>

      <main class="cat-panel">

        <button type="button" class="admin-toggle" @click="openAdmin">
          Admin
        </button>

        <section v-if="adminVisible" class="admin-panel">
          <h3>Admin area</h3>

          <label>
            Name
            <input v-model="adminForm.name" type="text" />
          </label>

          <label>
            URL
            <input v-model="adminForm.img" type="text" />
          </label>

          <label>
            Clicks
            <input v-model.number="adminForm.clicks" type="number" min="0" />
          </label>

          <div class="admin-actions">
            <button type="button" @click="closeAdmin">Cancel</button>
            <button type="button" class="save-button" @click="saveCatChanges">Save</button>
          </div>
        </section>

        <h2>{{ selectedCat.name }}</h2>
        <img
          :src="selectedCat.img"
          :alt="selectedCat.name"
          class="cat-image" width="300" height="300"
          @click="incrementClicks"
        />
        <p class="click-count">Number of clicks: {{ selectedCat.clicks }}</p>
      </main>
    </div>
  </div>
</template>

<style scoped>
</style>