<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cartItems, removeFromCart } = inject('cart')
</script>

<template>
  <ul class="flex flex-col gap-5 mb-8">
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @remove-from-cart="removeFromCart(item)"
    />
  </ul>
</template>
