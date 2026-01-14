<template>
  <div class="card">
    <div class="cover-wrapper">
      <img :src="book.cover" :alt="book.title" />
    </div>
    <h2>{{ book.title }}</h2>
    <p class="genre">{{ book.genre }}</p>
    <p>{{ book.description }}</p>
    <p v-if="book.adult" class="adult">🔞 18+</p>
    <div class="rating">
      <span v-for="star in 5" :key="star">{{ star <= book.rating ? '⭐' : '☆' }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  book: Object
})
</script>
