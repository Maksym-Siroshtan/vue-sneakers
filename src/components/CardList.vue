<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isButtonNotShowed: Boolean
})

const emit = defineEmits(['addToFavorite', 'onClickOnPlus'])
</script>

<template>
  <ul v-auto-animate class="grid grid-cols-4 gap-4">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :is-favorite="item.isFavorite"
      :is-added="item.isAdded"
      :on-click-favorite="isButtonNotShowed ? null : () => emit('addToFavorite', item)"
      :on-click-on-plus="isButtonNotShowed ? null : () => emit('onClickOnPlus', item)"
    />
  </ul>
</template>
