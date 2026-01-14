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
import noImageCover from './assets/covers/no_image_cover.png'

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
const isFormOpen = ref(false)
const editingBookId = ref(null)

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

// Шаблон формы
const form = ref({
    title: '',
    description: '',
    cover: '',
    genre: '',
    adult: false,
    rating: 0
})

// Статистика
const totalBooks = computed(() => books.value.length)
const averageRating = computed(() => {
  const rated = books.value.filter(b => b.rating > 0) // Оставляем книги только с рейтингом > 0
  if(!rated.length) return '0.00'
  const avgRating = rated.reduce((sum,book) => sum + book.rating, 0) / rated.length // sum = 0
  return avgRating.toFixed(2)
})

// Открытие формы добавления
const openCreateForm = () => {
  resetForm()
  isFormOpen.value = true
}

// Сохранить книгу
const saveBook = () => {
  if (editingBookId.value){
    const book = books.value.find(b => b.id === editingBookId.value)
    Object.assign(book, form.value) // Записываем значения формы в книгу
  } else {
    books.value.push({
      id: Date.now(),
      ...form.value
    })
  }
  resetForm()
}
// Редактировать книгу
const openEditForm = (book) => {
  form.value = { ...book }
  editingBookId.value = book.id
  isFormOpen.value = true
}

// Удаление киниг
const deleteBook = (id) => {
  books.value = books.value.filter(b => b.id !== id) // Удаляем по id
}

// Сброс рейтинга
const resetRatings = () => {
  books.value.forEach(b => b.rating = 0) // Задаем у всех книг значение рейтинга = 0
}

// Очистка формы
const resetForm = () => {
  form.value = {
    title: '',
    description: '',
    cover: '',
    genre: '',
    adult: false,
    rating: 0
  }
  editingBookId.value = null
  isFormOpen.value = false
}

</script>

<template>
  <div class="container">
    <h1>📚 Рейтинг книг</h1>


    <!-- Управление фильтрами -->
    <div class="controls">
      <label>
        <input type="checkbox" v-model="showAdult" /> Показывать 18+
      </label>

      <button @click="sortHighToLow = !sortHighToLow">Сортировать по рейтингу {{ sortHighToLow ? '▼' : '▲' }}</button>
      <button @click="openCreateForm">➕ Добавить</button>
      <button @click="resetRatings">♻️ Сбросить рейтинги</button>
    </div>

    <!-- Статистика -->
    <div class="stats">
      <p>Всего книг: {{ totalBooks }}</p>
      <p>Средний рейтинг: {{ averageRating }}</p>
    </div>

    <!-- Форма -->
    <div v-if="isFormOpen" class="form">
      <input v-model="form.title" placeholder="Название">
      <textarea v-model="form.description" placeholder="Описание"></textarea>
      <input v-model="form.cover" placeholder="URL обложки" />
      <select v-model="form.genre">
        <option disabled value="">Выберите жанр</option>
        <option>Научная фантастика</option>
        <option>Фэнтези</option>
        <option>Социальная фантастика</option>
        <option>Философская фантастика</option>
        <option>Твёрдая научная фантастика</option>
        <option>Историческая фантастика</option>
        <option>Приключения</option>
        <option>Детектив</option>
        <option>Ужасы</option>
        <option>Романтика</option>
        <option>Киберпанк</option>
        <option>Эпическая фантастика</option>
      </select>
      <label><input type="checkbox" v-model="form.adult" /> 18+</label>
      <div class="form-actions">
        <button @click="saveBook">Сохранить</button>
        <button @click="resetForm">Отменить</button>
      </div>
    </div>

    <!-- Карточки -->
    <div class="grid">
      <BookCard v-for="book in sortedBooks" :key="book.id" :book="book" @edit="openEditForm" @delete="deleteBook"/>
    </div>
  </div>
</template>
