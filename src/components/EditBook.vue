<template>
  <form @submit.prevent="updateBook"> 
      <label for="title">
        Title
        <input type="text" id="title" required v-model.trim="newBook.title" />
      </label>
      <label for="author">
        Author
        <input type="text" id="author" required v-model.trim="newBook.author" />
      </label>
      <label for="description">
        Description (optional)
        <input type="text" id="description" v-model.trim="newBook.description" />
      </label>
      <label for="year">
        Year
        <input type="number" id="year" required v-model.number="newBook.year" />
      </label>
      <label for="rating">
        Rating (optional)
        <input type="number" id="rating" v-model.number="newBook.rating" />
      </label>
      <button type="submit">Submit</button>
      <button type="reset" @click="$emit('closeForm')">Close form</button>
    </form>

  
  <p v-if="loading">Updating...</p>
  <p v-if="error">{{ error }}</p>
</template>

<script>
import { reactive } from 'vue'
import UPDATE_BOOK_MUTATION from '../graphql/updateBook.mutation.gql'
import { useMutation } from '@vue/apollo-composable'

export default {
  emits: ['closeForm'],
  props: {
    bookId: {
      type: String,
      required: true,
    },
    initialTitle: {
      type: String,
      required: true,
    },
    initialDescription: {
      type: String,
      required: false,
    },
    initialRating: {
      type: Number,
      required: false,
    },
    initialAuthor: {
      type: String,
      required: false,
    },
    initialYear: {
      type: Number,
      required: false,
    },
  },
  setup(props, { emit }) {
    const newBook = reactive({
        title: props.initialTitle,
        author: props.initialAuthor,
        year: props.initialYear,
        rating: props.initialRating,
        description: props.initialDescription,
      })

    const { mutate: updateBook, onDone, loading, error } = useMutation(
      UPDATE_BOOK_MUTATION, 
      () => ({
        variables: {
          input: {
            id: props.bookId, 
            title: newBook.title, 
            description: newBook.description,
            rating: parseFloat(newBook.rating),
            author: newBook.author,
            year: newBook.year,
          }
        },
      })
    )
    onDone(() => {
      emit('closeForm')
    })
    return { newBook, updateBook, loading, error }
  },
}
</script>