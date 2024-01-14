<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
// import Drawer from './components/Drawer.vue'
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 bg-white m-auto py-14 rounded-xl shadow-xl">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center mb-10">
        <h2 class="text-3xl font-bold">Все кроссовки</h2>
        <div class="flex gap-4">
          <select class="border rounded-md outline-none pl-4 pr-3">
            <option value="title">По названию</option>
            <option value="price">По цене (по возрастанию)</option>
            <option value="-price">По цене (по убыванию)</option>
          </select>
          <div class="flex relative">
            <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
            <input
              type="text"
              placeholder="Поиск..."
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-slate-400 placeholder:text-black focus:placeholder:text-slate-300"
            />
          </div>
        </div>
      </div>

      <CardList />
    </div>
  </div>
</template>
