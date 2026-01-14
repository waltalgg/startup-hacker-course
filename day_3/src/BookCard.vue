<template>
  <div class="card">
    <!-- Обложка и большая звезда -->
    <div class="cover-wrapper">
      <img :src="props.book.cover" :alt="props.book.title" />
      <div class="big-rating">
        <span :class="{ empty: !props.book.rating }">
          ⭐ {{ props.book.rating || '—' }}
        </span>
      </div>
    </div>

    <!-- Наполнение карточки -->
    <h2>{{ props.book.title }}</h2>
    <p class="genre">{{ props.book.genre || 'Жанр не выбран' }}</p>
    <p>{{ props.book.description }}</p>

    <p v-if="props.book.adult" class="adult">🔞 18+</p>

    <!-- Рейтинг -->
    <div class="rating">
      <span v-for="star in 5" :key="star" :class="{ active: isActive(star) }" @click.stop="setRating(star)" @mouseenter="hoverRating = star" @mouseleave="hoverRating = 0">
        ★
      </span>
    </div>

    <!-- Взаимодействия -->
    <div class="actions">
      <button @click="$emit('edit', props.book)">✏️</button>
      <button @click="$emit('delete', props.book.id)">🗑️</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({book: Object})

// Временная подсветка для hover
const hoverRating = ref(0)
const isActive = (star) => {
  const current = Number(props.book.rating || 0)
  return hoverRating.value ? star <= hoverRating.value : star <= current
}

// Устанавливаем рейтинг по клику
const setRating = (value) => {
  props.book.rating = Number(value)
  hoverRating.value = 0
}
</script>
