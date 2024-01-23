<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'

import DrawerHeader from './DrawerHeader.vue'
import DrawerFooter from './DrawerFooter.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const { cartItems, totalPrice } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const isCartEmpty = computed(() => cartItems.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || isCartEmpty.value)

const addToOrders = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://8ca1b7098d059351.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })

    cartItems.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-60"></div>
  <div class="flex flex-col fixed top-0 right-0 w-96 h-full overflow-auto bg-white z-20 p-8">
    <DrawerHeader class="mb-8" />

    <div v-if="!totalPrice || orderId" class="flex items-center h-full">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else class="flex flex-col h-full">
      <CartItemList class="flex-1" />

      <DrawerFooter @add-to-orders="addToOrders" :button-disabled="buttonDisabled" />
    </div>
  </div>
</template>
