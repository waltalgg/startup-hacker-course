<script setup>
import { ref, computed } from 'vue'
import './App.css'
import BookCard from './BookCard.vue'

import duneCover from './assets/covers/dune_cover.png'
import kosinogCover from './assets/covers/kosinog_cover.png'
import solarisCover from './assets/covers/solaris_cover.png'
import earthCover from './assets/covers/earth_cover.png'
import leftHandCover from './assets/covers/left_hand_cover.png'
import picnicCover from './assets/covers/piknik_cover.png'

const books = ref([
  {
    id: 1,
    title: 'Дюна',
    description: 'Эпическая научно-фантастическая сага Фрэнка Герберта о власти, религии и экологии планеты Арракис.',
    genre: 'Научная фантастика',
    cover: duneCover,
    adult: false,
    rating: 4
  },
  {
    id: 2,
    title: 'Косиног. История о колдовстве',
    description: 'Фэнтезийный роман о магии, древних силах и судьбе человека между двумя мирами.',
    genre: 'Фэнтези',
    cover: kosinogCover,
    adult: true,
    rating: 3
  },
  {
    id: 3,
    title: 'Солярис',
    description: 'Философская фантастика Станислава Лема о попытке понять чуждый разум.',
    genre: 'Философская фантастика',
    cover: solarisCover,
    adult: false,
    rating: 4
  },
  {
    id: 4,
    title: 'Воспоминания о прошлом Земли',
    description: 'Цикл Лю Цысиня о первом контакте человечества с инопланетной цивилизацией.',
    genre: 'Твёрдая научная фантастика',
    cover: earthCover,
    adult: false,
    rating: 5
  },
  {
    id: 5,
    title: 'Левая рука Тьмы',
    description: 'Социальная фантастика Урсулы Ле Гуин о планете с уникальным устройством общества.',
    genre: 'Социальная фантастика',
    cover: leftHandCover,
    adult: true,
    rating: 3
  },
  {
    id: 6,
    title: 'Пикник на обочине',
    description: 'Роман братьев Стругацких о загадочной Зоне и её опасных артефактах.',
    genre: 'Научная фантастика',
    cover: picnicCover,
    adult: true,
    rating: 2
  }
])

const showAdult = ref(false)
const sortHighToLow = ref(true)

// Фильтр 18+
const filteredBooks = computed(() => {
  return books.value.filter(book => showAdult.value || !book.adult)
})

// Сортировка по рейтингу
const sortedBooks = computed(() => {
  return [...filteredBooks.value].sort((a, b) =>
      sortHighToLow.value ? b.rating - a.rating : a.rating - b.rating
  )
})
</script>

<template>
  <div class="container">
    <h1>📚 Мой рейтинг последниих прочитанных книг</h1>

    <div class="controls">
      <label>
        <input type="checkbox" v-model="showAdult" /> Показывать 18+
      </label>
      <button @click="sortHighToLow = !sortHighToLow">
        Сортировать по рейтингу {{ sortHighToLow ? '▼' : '▲' }}
      </button>
    </div>

    <div class="grid">
      <BookCard v-for="book in sortedBooks" :key="book.id" :book="book" />
    </div>
  </div>
</template>
