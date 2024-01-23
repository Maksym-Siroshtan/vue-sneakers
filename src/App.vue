<script setup>
import { ref, watch, provide, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cartItems = ref([])
const isDrawerOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => (acc += item.price), 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const openDrawer = () => {
  isDrawerOpen.value = true
}

const closeDrawer = () => {
  isDrawerOpen.value = false
}

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)

watch(isDrawerOpen, () => {
  if (isDrawerOpen.value) {
    document.documentElement.style.overflow = 'hidden'
  } else {
    document.documentElement.style.overflow = ''
  }
})

provide('cart', {
  cartItems,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
  totalPrice,
  vatPrice
})
</script>

<template>
  <Drawer v-if="isDrawerOpen" />
  <div class="w-4/5 bg-white m-auto mt-20 rounded-xl shadow-xl">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
