<script setup>
import DrawerHeader from './DrawerHeader.vue'
import DrawerFooter from './DrawerFooter.vue'
import CartItemList from './CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-60"></div>
  <div class="flex flex-col fixed top-0 right-0 w-96 h-full overflow-auto bg-white z-20 p-8">
    <DrawerHeader class="mb-8" />

    <CartItemList class="flex-1" />

    <DrawerFooter />
  </div>
</template>
