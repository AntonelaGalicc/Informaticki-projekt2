<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-text-field v-model="search"
                      append-icon="mdi-magnify"
                      label="Pretraga po naslovima"
                      single-line
                      hide-details
                      :loading="isLoading"
                      @input="searchEntries"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="book in books"
             :key="book.title"
             cols="12"
             md="3"
      >
        <v-card class="books">
          <h4 class="book-title">{{ book.title }}</h4>
          <br><br><br><br><br><br>
          <br><br>
          <p class="book-author">Autor: {{ book.author }}</p>
          <br>
          <p>Cijena: {{ book.price }}</p>
          <br>
          <p>Izdavač: {{ book.publisher }}</p>
          <br>
          <p>Opis: {{ book.description }}</p>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-pagination total-visible="6"
                      v-model="page"
                      :length="Math.ceil(totalBooks / perPage)"
        ></v-pagination>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomeView',
  data() {
    return {
      books: [],
      page: 1,
      totalBooks: 0,
      perPage: 20,
      search: '',
      isLoading: false,
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      let api = 'https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json';
      this.isLoading = true;
      axios
        .get(api, {
          params: {
            'api-key': '0Y8O2pzG1lcl1X1cOh1pY2i1jZnVuZMY',
            'offset': 20 * (this.page - 1),
            'title': this.search,
          },
        })
        .then((response) => {
          this.books = response.data.results;
          this.totalBooks = response.data.num_results;
          this.isLoading = false;
        })
        .catch((error) => {
          console.error(error);
          this.isLoading = false;
        });
    },
    searchEntries() {
      this.books = [];
      this.page = 1;
      this.getData();
    },
  },
  watch: {
    page() {
      this.getData();
    },
    search(val) {
      if (!val) {
        return;
      }
      this.isLoading = true;
      this.searchEntries();
    },
  },
};
</script>

<style scoped>
.books {
  color: #0a0b0be2;
  padding-bottom: 300px;
  width: 400px;
  height: 100px;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  font-family: Verdana, Arial, Helvetica, sans-serif;
  font-weight: 20;
  font-variant-ligatures: normal;
  font-size: 11px;
  text-align: center;

}




</style>
