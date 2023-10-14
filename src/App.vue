<script>
import { useQuery } from '@vue/apollo-composable'
import ALL_BOOKS_QUERY from './graphql/allBooks.query.gql'
import { ref } from 'vue'
// import ALL_AUTHORS_QUERY from './graphql/allAuthors.query.gql'

export default {
  name: 'App',
  setup() {
    const searchTerm = ref('') // create a reactive property with 'ref'
    const { result, loading, error } = useQuery(ALL_BOOKS_QUERY, () => ({ search: searchTerm.value }))

    return { result, searchTerm, loading, error } // don't forget to return 'searchTerm'!
  },
}

</script>

<template>
  <div>
    <input type="text" v-model="searchTerm" />
    <p v-if="loading">Loading...</p>
    <p v-else-if="error">Something went wrong! Please try again</p>
    <template v-else>
      <p v-for="book in result.allBooks" :key="book.id">
        {{ book.title }}
      </p>
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
