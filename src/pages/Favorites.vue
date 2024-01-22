<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://8ca1b7098d059351.mokky.dev/favorites?_relations=items'
    )

    favorites.value = data.map((favorite) => favorite.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-10">Закладки</h2>
  <CardList :items="favorites" is-button-not-showed />
</template>
