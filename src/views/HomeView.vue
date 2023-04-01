<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <v-text-field v-model="search"
                              append-icon="mdi-magnify"
                              label="Pretraga"
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
                <v-card height="400" width="400" class="books" >
                    <div >Naziv autora: {{book.author}}</div> 
                   <div >{{book.description}}</div>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <v-pagination total-visible="4"
                              v-model="page"
                              :length="totalpages"></v-pagination>
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
                totalpages: 0,
                search: '',
                isLoading: false,
            }
        },
        created() {
            console.log('created')
            this.getData();
        },
        methods: {
            getData() {
                let api = "https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
                return this.axios.get(api, {
                    params: {
                        'api-key':'0Y8O2pzG1lcl1X1cOh1pY2i1jZnVuZMY',
                        'page': this.page,
                        'limit': 4,
                    }
                }).then((response) => {
                    console.log(response.data.books)
                    this.books = response.data.books
                    //this.totalBooks = this.books.length
                    //this.total_pages=this.totalBooks/4
                })
            },
            fetchEntriesDebounced() {
                clearTimeout(this._searchTimerId)
                this._searchTimerId = setTimeout(() => {
                    this.getData();
                }, 500)
            },
            searchEntries() {
                this.books = []
                this.page = 1
                this.fetchEntriesDebounced()
            },
            pretraga() {
                let api = "https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
                return this.axios.get(api, {
                    params: {
                        's': this.search,
                    }
                }).then((response) => {
                    console.log(response.data.books)
                    this.books = response.data.books
                    this.totalBooks = this.books.length
                    //this.totalpages=this.totalBooks/4
                    //this.isLoading = false
                })
            }
        },
        watch: {
            page: function () {
                this.getData();
            },
            pretraga(val) {
                if (!val) {
                    return
                }
                this.isLoading = true
                this.searchEntries()
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
    padding-bottom:550px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
  }
    .v-text-field{
      width:400px;
  }
</style>