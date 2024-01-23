<script setup>
import { ref, reactive, watch, inject, onMounted } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

import CardList from '../components/CardList.vue'

const { cartItems, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)

const onClickOnPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://8ca1b7098d059351.mokky.dev/items', {
      params
    })
    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://8ca1b7098d059351.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post('https://8ca1b7098d059351.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://8ca1b7098d059351.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const data = localStorage.getItem('cartItems')
  cartItems.value = data ? JSON.parse(data) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
</script>

<template>
  <div class="flex justify-between items-center mb-10">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>
    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        :value="filters.sortBy"
        class="border rounded-md outline-none pl-4 pr-3"
      >
        <option value="title">По названию</option>
        <option value="price">По цене (по возрастанию)</option>
        <option value="-price">По цене (по убыванию)</option>
      </select>
      <div class="flex relative">
        <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
        <input
          @input="onChangeSearchInput"
          :value="filters.searchQuery"
          type="text"
          placeholder="Поиск..."
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-slate-400 placeholder:text-black focus:placeholder:text-slate-300"
        />
      </div>
    </div>
  </div>

  <CardList :items="items" @add-to-favorite="addToFavorite" @on-click-on-plus="onClickOnPlus" />
</template>
