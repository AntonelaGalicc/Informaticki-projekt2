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
                              @input="pretraga"
                              >
                </v-text-field>
  
            </v-col>
  
        </v-row>
        <v-row >
            <v-col v-for="book in books"
                   :key="book.title"
                   cols="12"
                   md="3" 
                   >
                <v-card class="books" >
                    <h3>{{book.title}}</h3> 
                    <br><br><br>
                    <h5>Autor: {{book.author}}</h5> 
                    <br>
                    <h5>Cijena:{{book.price}}</h5>
                    <br>
                    <h5>Izdavač: {{book.publisher}}</h5>
                    <br>
                    <h5>Opis: {{book.description}}</h5>
                    
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <v-pagination total-visible="6"
                              v-model="page"
                              :length="Math.ceil(totalBooks/perPage)"></v-pagination>
            </v-col>
        </v-row>
    </v-container>
  </template>
  
  <script>
    export default {
        name: 'HomeView',
        data() {
    return {
      books: [],
      page: 1,
      totalBooks: 0,
      perPage: 20,
      search: '',
      author: '',
      isLoading: false,
      isLoadingAuthor: false,
      addDialog: false,
      successAlert: false
    }
  },
  created() {
    console.log('created')
    this.getData();
  },
  methods: {
    getData() {
      let api = "https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
      this.axios.get(api, {
        params: {
          'api-key': '0Y8O2pzG1lcl1X1cOh1pY2i1jZnVuZMY',
          'offset': 20 * (this.page - 1),
          'title': this.search,
          'author': this.author
        }
      }).then((response) => {
        console.log(response.data)
        this.books = response.data.results
        this.totalBooks = response.data.num_results
        this.isLoading = false
        this.isLoadingAuthor = false
      })
    },
    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId)
      this._searchTimerId = setTimeout(() => {
        this.getData()
      }, 500) /* 500ms throttle */
    },
    searchEntries() {
      this.books = []
      this.page = 1
      this.fetchEntriesDebounced()
    }
  },
  watch: {
    page: function() {
      this.getData();
    },
    search (val) {
      if (!val) {
        return
      }
      this.isLoading = true
      this.searchEntries()
    },
    author (val) {
      if (!val) {
        return
      }
      this.isLoadingAuthor = true
      this.searchEntries();
    }
  }
}
    </script>
<style>
    .image {
        width: 400px;
        height: 400px;
        margin-left: auto;
        margin-right: auto;
    }
    .books{
    color:#7c8f9fbe;
    padding-bottom:550px;
    width: 400px;
    height: 100px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-weight: 20;
    font-variant-ligatures: normal;
    font-size: 1rem;
    letter-spacing: 1px;
  }
    .v-text-field{
      width:400px;
  }
</style>