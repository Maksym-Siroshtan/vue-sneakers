<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
</script>

<template>
  <div class="w-4/5 bg-white m-auto my-14 rounded-xl shadow-xl">
    <Header />

    <div class="p-10">
      <h2 class="text-3xl font-bold mb-10">Все кроссовки</h2>

      <CardList />
    </div>
  </div>
</template>
