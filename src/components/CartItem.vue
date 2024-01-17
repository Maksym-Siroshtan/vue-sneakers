<script setup>
defineProps({
  id: Number,
  title: String,
  price: Number,
  imageUrl: String
})

const emit = defineEmits(['removeFromCart'])
</script>

<template>
  <li class="flex gap-5 border border-slate-200 rounded-2xl p-5">
    <img :src="imageUrl" :alt="title" class="w-18 h-16 rounded-2xl" />
    <div>
      <p>{{ title }}</p>
      <div class="flex items-center justify-between">
        <b>{{ price }} руб.</b>
        <button class="opacity-40 hover:opacity-100 hover:-translate-y-px transition">
          <img @click="emit('removeFromCart')" src="/close.svg" alt="Close" />
        </button>
      </div>
    </div>
  </li>
</template>
