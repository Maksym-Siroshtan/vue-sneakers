<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const onClickFavorite = () => {
  console.log('Favorite!')
}

const onClickAdd = () => {
  console.log('Added!')
}
</script>

<template>
  <ul class="grid grid-cols-4 gap-4">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :on-click-favorite="onClickFavorite"
      :on-click-add="onClickAdd"
    />
  </ul>
</template>
