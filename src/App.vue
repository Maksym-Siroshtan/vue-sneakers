<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cartItems = ref([])
const isDrawerOpen = ref(false)
const isOrderCreating = ref(false)
const isCartEmpty = computed(() => cartItems.value.length === 0)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => (acc += item.price), 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))
const isCartButtonDisabled = computed(() => isOrderCreating.value || isCartEmpty.value)

const openDrawer = () => {
  isDrawerOpen.value = true
}

const closeDrawer = () => {
  isDrawerOpen.value = false
}

const addToOrders = async () => {
  try {
    isOrderCreating.value = true
    const { data } = await axios.post('https://8ca1b7098d059351.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })

    cartItems.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isOrderCreating.value = false
  }
}

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

watch(isDrawerOpen, () => {
  if (isDrawerOpen.value) {
    document.documentElement.style.overflow = 'hidden'
  } else {
    document.documentElement.style.overflow = ''
  }
})

watch(
  cartItems,
  () => {
    localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)

provide('cart', {
  cartItems,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
  totalPrice,
  vatPrice,
  addToOrders,
  isCartButtonDisabled
})
</script>

<template>
  <Drawer v-if="isDrawerOpen" :total-price="totalPrice" />
  <div class="w-4/5 bg-white m-auto mt-20 rounded-xl shadow-xl">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
