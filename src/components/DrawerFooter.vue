<script setup>
import { inject } from 'vue'

defineProps({
  buttonDisabled: Boolean
})

const emit = defineEmits(['addToOrders'])

const { totalPrice, vatPrice } = inject('cart')
</script>

<template>
  <div>
    <div class="flex items-end mb-2">
      <span>Итого:</span>
      <div class="border-b border-dashed flex-1"></div>
      <b>{{ totalPrice }} руб.</b>
    </div>

    <div class="flex items-end mb-7">
      <span>Налог 5%:</span>
      <div class="border-b border-dashed flex-1"></div>
      <b>{{ vatPrice }} руб.</b>
    </div>

    <button
      :disabled="buttonDisabled"
      @click="emit('addToOrders')"
      class="flex justify-center gap-12 items-center w-full bg-lime-500 py-3 rounded-3xl text-white hover:bg-lime-600 active:bg-lime-700 disabled:bg-gray-300 transition pl-14"
    >
      Оформить заказ
      <img src="/arrow-next.svg" alt="Arrow-next" />
    </button>
  </div>
</template>
