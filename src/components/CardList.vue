<script setup>
import Card from './Card.vue'

const onClickFavorite = () => {
  console.log('Favorite!');
}

const onClickAdd = () => {
  console.log('Added!');
}

</script>

<template>
  <ul class="grid grid-cols-4 gap-5">
    <Card
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      image-url="/sneakers/sneakers-1.jpg"
      :price="12999"
      :is-favorite="true"
      :is-added="true"
      :on-click-favorite="onClickFavorite"
      :on-click-add="onClickAdd"
    />
    <Card
      title="Мужские Кроссовки Nike Air Max 270"
      image-url="/sneakers/sneakers-2.jpg"
      :price="12999"
    />
    <Card
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      image-url="/sneakers/sneakers-3.jpg"
      :price="8499"
    />
    <Card
      title="Кроссовки Puma X Aka Boku Future Rider"
      image-url="/sneakers/sneakers-4.jpg"
      :price="8999"
    />
  </ul>
</template>
