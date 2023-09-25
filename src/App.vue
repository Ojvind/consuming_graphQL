<script>
import { useQuery } from '@vue/apollo-composable'
import ALL_BOOKS_QUERY from './graphql/allBooks.query.gql'
import ALL_AUTHORS_QUERY from './graphql/allAuthors.query.gql'

export default {
  name: 'App',
  setup() {
    const { result } = useQuery(ALL_BOOKS_QUERY)
    console.log("RESULT")
    console.log(result)
    return { result }
  }
}

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div>
       <p v-for="book in result.allBooks" :key="book.id">
        {{ book.title }}
      </p>
    </div>
    <!-- <div>
      <p v-for="writer in result.writers.edges" :key="writer.id">
        {{ writer.name }}
      </p>
    </div> -->
  </header>
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
