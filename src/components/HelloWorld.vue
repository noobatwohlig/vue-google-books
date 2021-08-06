<template>
  <div>
    <input type="search" v-on:input="searchBooks" v-model="searchQuery" />
    <h4 v-if="isLoading">Loading...</h4>
    <table style="text-align: left">
      <tr v-for="book in books" :key="book.id">
        <td>
          <a target="_blank" v-bind:href="book.volumeInfo.imageLinks.thumbnail">
            <img
              width="40px"
              height="40px"
              v-bind:src="book.volumeInfo.imageLinks.smallThumbnail"
            />
          </a>
        </td>
        <td>
          <a target="_blank" v-bind:href="book.volumeInfo.previewLink">
            {{ book.volumeInfo.title }}
          </a>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      books: [],
      searchQuery: "",
      isLoading: false,
    };
  },
  methods: {
    async searchBooks() {
      if (this.searchQuery.trim() != "") {
        this.isLoading = true;
        await fetch(
          `https://www.googleapis.com/books/v1/volumes?q=${this.searchQuery}`
        ).then((resp) => {
          resp.json().then((data) => {
            this.books = data.items;
          });
          console.log(this.books);
        });
        this.isLoading = false;
      }
    },
  },
};
</script>