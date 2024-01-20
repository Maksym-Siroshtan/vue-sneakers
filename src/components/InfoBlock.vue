<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center w-70">
    <img :src="imageUrl" alt="Info-image" class="w-36" />
    <h2 class="text-2xl font-bold mt-4">{{ title }}</h2>
    <p class="text-xl text-slate-400 text-center mt-2">{{ description }}</p>
  </div>
</template>
