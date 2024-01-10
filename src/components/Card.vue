<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})
</script>

<template>
  <li
    class="relative bg-white border border-slate-100 rounded-3xl p-9 cursor-pointer hover:-translate-y-1 hover:shadow-xl transition-all"
  >
    <img
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      @click="onClickFavorite"
      alt="Favorite"
      class="absolute top-10 left-8"
    />
    <img :src="imageUrl" alt="Sneakers-1" class="mb-2 rounded-3xl w-44 h-30" />

    <p class="text-base font-bold mb-3">{{ title }}</p>

    <div class="flex justify-between items-center">
      <div class="flex flex-col">
        <span class="text-xs text-slate-400 uppercase">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>
      <img :src="isAdded ? '/checked.svg' : '/plus.svg'" @click="onClickAdd" alt="Add sneakers" />
    </div>
  </li>
</template>
