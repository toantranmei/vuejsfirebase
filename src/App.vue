<template>
  <div id="app" class="container">
    <div class="card">
      <h4 class="card-header">Featured</h4>

      <div class="card-body">

        <h4 class="card-title">Special title treatment</h4>
        <form class="text-left" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="">Author</label>
            <input type="text" class="form-control" placeholder="Enter Author" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="">Title</label>
            <input type="text" class="form-control" placeholder="Enter Title" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="">URL</label>
            <input type="text" class="form-control" placeholder="Enter URL" v-model="newBook.url">
          </div>
          <button type="submit" class="btn btn-primary">Add book</button>
        </form>
      </div>

    </div>
    <div class="card mt-5">
      <h4 class="card-header">List book</h4>

      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">Author</th>
              <th scope="col">Title</th>
              <th scope="col">Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td><a v-bind:href="book.url">{{ book.author }}</a></td>
              <td>{{ book.title }}</td>
              <td><a href="" class="text-danger" v-on:click="removeBook(book)">DELETE</a></td>
            </tr>
          </tbody>
        </table>

      </div>

    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyD3g3KCnyfRqk2PB6JL6m034XCK53pbBZs',
  authDomain: 'shopmini-f1bba.firebaseapp.com',
  databaseURL: 'https://shopmini-f1bba.firebaseio.com',
  projectId: 'shopmini-f1bba',
  storageBucket: 'shopmini-f1bba.appspot.com',
  messagingSenderId: '870415412253'
}

let app = Firebase.initializeApp(config)

let db = app.database()

let booksRef = db.ref('books')

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = ''
    },
    removeBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book Removed!')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
