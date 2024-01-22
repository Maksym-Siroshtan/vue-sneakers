<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickOnPlus: Function,
  onClickFavorite: Function
})
</script>

<template>
  <li
    class="flex flex-col relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-1 hover:shadow-xl transition-all"
  >
    <img
      v-if="onClickFavorite"
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      @click="onClickFavorite"
      alt="Favorite"
      class="absolute top-10 left-8 w-10"
    />
    <img :src="imageUrl" alt="Sneakers-1" class="mb-2 rounded-3xl" />

    <p class="text-xl font-medium mb-4">{{ title }}</p>

    <div class="flex justify-between items-center">
      <div class="flex flex-col">
        <span class="text-lg text-slate-400 uppercase">Цена:</span>
        <b class="text-xl">{{ price }} руб.</b>
      </div>
      <img
        v-if="onClickOnPlus"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        @click="onClickOnPlus"
        alt="Add sneakers"
        class="w-10"
      />
    </div>
  </li>
</template>
