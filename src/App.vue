<template>
  <div class="container mt-6">
    <h1 class="title is-3 has-text-centered mb-5">Моя коллекция</h1>
    <div class="columns">
      <div class="column is-8-tablet is-offset-2-tablet">
        <form @submit.prevent="addBook" class="field has-addons mb-5">
          <input
          v-model="newBook"
          type="text"
          class="input"
          placeholder="Название"
          required
          >
          <input 
            v-model="newDescription"
            type="text" 
            class="input"
            placeholder="Описание книги"
            required
          ></input>

          <input type="file" accept="image/*" @change="handleFileUpload" class="input">

          <button class="button is-primary" type="submit">Добавить</button>
        </form>
      </div>
    </div>

    <div class="columns is-multiline">
      <div
      v-for="book in books"
      :key="book.id"
      class="column is-4-tablet mb-3"
      >
        <div class="card">
          <div class="card-content">
            <div class="title is-4">{{ book.title }}</div>

            <div class="card-image">
              <figure class="image is-4by3">
                <img :src="book.image" alt="Обложка книги">
              </figure>
            </div>

            <p class="text">{{ book.description }}</p>

            <button
            @click="removeBook(book.id)"
            class="button is-danger"
            >
            Удалить
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
  import { ref } from 'vue'
  const books = ref([])
  const newBook = ref('')
  const newDescription = ref('')
  const selectedImage = ref(null)

  const handleFileUpload = (event) => {
    const file = event.target.files[0]
    if (file) {
      selectedImage.value = URL.createObjectURL(file)
    }
  }
  const addBook = () => {
    books.value.push({
      id: Date.now(),
      title: newBook.value,
      description: newDescription.value,
      image: selectedImage.value
    })
    newBook.value = ''
    newDescription.value = ''
    selectedImage.value = null
  }

  const removeBook = (id) => {
    books.value = books.value.filter(book => book.id !== id)
  }
</script>