<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      books: [],
      author: "",
      title: "",
    };
  },

  methods: {
    async getBooks() {
      try {
        const books = await axios.get(
          "http://localhost:8080/books"
        );

        this.books = books.data;
      } catch (e) {
        console.log(e);
      }
    },

    async storeBook() {
      try {
        const book = await axios.post(
          "http://localhost:8080/books", 
          {
            author: this.author,
            title : this.title
          }
        ).then((response) => {
          response.data.book = null;
          this.getBooks();
        })

        console.log(book)
      } catch(e) {
        console.log(e);
      }
    },

    async deleteBook(id) {
      let x = window.confirm("You want to delete the book?");

      if (x) {
        const book = await axios.delete(
          "http://localhost:8080/books/" + id
        ).then(() => {
          const index = this.books.findIndex(book => book.id === id)
          if (~index)
          this.books.splice(index, 1)
        })

        console.log(book);
        alert("Book deleted!");
      }
    },
  },
};
</script>

<template>
  <div>
    <button @click="getBooks">Get books</button> <br />

    <div class="books" v-for="book in books" :key="book.id">
      <h2>{{ book.author }}</h2>
      <p>{{ book.title }}</p>
      <button @click="deleteBook(book.id)">Delete</button>
    </div>
  </div>  
  <div class="book-add">
    <h2>Add a book</h2>
    <input type="text" v-model="author" /> <br /><br />
    <input type="text" v-model="title" /> <br />
    <br />
    <button @click="storeBook">Store</button>
  </div>
</template>

<style scoped>

</style>
