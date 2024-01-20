<script setup>
import { ref, reactive, onMounted, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const cartItems = ref([])
const isDrawerOpen = ref(false)
const isOrderCreating = ref(false)
const isCartEmpty = computed(() => cartItems.value.length === 0)

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => (acc += item.price), 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))
const isCartButtonDisabled = computed(() => isOrderCreating.value || isCartEmpty.value)

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

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

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://8ca1b7098d059351.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

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
        parentId: item.id
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

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

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

watch(isDrawerOpen, () => {
  if (isDrawerOpen.value) {
    document.documentElement.style.overflow = 'hidden'
  } else {
    document.documentElement.style.overflow = ''
  }
})

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
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
    </div>
  </div>
</template>
