<script>
import { useQuery } from '@vue/apollo-composable'
import ALL_BOOKS_QUERY from './graphql/allBooks.query.gql'
import AddBook from './components/AddBook.vue'
import { computed, ref } from 'vue'
import EditBook from './components/EditBook.vue'

export default {
  name: 'App',
  components: {
    AddBook,
    EditBook
},
  setup() {
    const searchTerm = ref('')
    const activeBook = ref(null) 
    const showNewBookForm = ref(false)
    const { result, loading, error } = useQuery(
      ALL_BOOKS_QUERY,
      () => ({
        search: searchTerm.value,
      }),
      () => ({
        debounce: 500,
        // enabled: searchTerm.value.length > 2,
      })
    )
    const books = computed(() => result.value?.allBooks ?? [])

    return { books, searchTerm, loading, error, activeBook, showNewBookForm }
  },
}

</script>

<template>
  <div>
    <div>
      <button v-if="!showNewBookForm" @click="showNewBookForm = true">
        Add a new book
      </button>
      <AddBook v-if="showNewBookForm" :search="searchTerm" @closeForm="showNewBookForm = false" />
    </div>
    <input type="text" v-model="searchTerm" />
    <p v-if="loading">Loading...</p>
    <p v-else-if="error">Something went wrong! Please try again</p>
    <template v-else>
      <p v-if="activeBook">
        Update "{{ activeBook.title }}"
        <div>
          <EditBook
          :book-id="activeBook.id"
          :initial-title="activeBook.title"
          :initial-description="activeBook.description"
          :initial-rating="activeBook.rating"
          :initial-author="activeBook.author"
          :initial-year="activeBook.year"
          @closeForm="activeBook = null"
        />
        </div>
      </p>
      <template v-else>
        <p v-for="book in books" :key="book.id">
          {{ book.title }} - {{ book.rating }} - {{ book.author }} - {{ book.year }}
          <button @click="activeBook = book">Edit book</button>
        </p>
      </template>
    </template>
  </div>
</template>


<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
