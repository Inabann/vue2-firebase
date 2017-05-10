<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase app</h1>
    </div>
    
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Agregar Libro</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Titulo:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuth">Autor:</label>
            <input type="text" id="bookAuth" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Agregar libro">
        </form>
        
      </div>

    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Lista de Libros</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Titulo</th>
              <th>Autor</th>
              <th>Eliminar</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'
import Firebase from 'firebase'
import toastr from 'toastr'


let config = {
  apiKey: "AIzaSyC0XNGfMmtnPCKpHp9O622y7xMmn5Yqwuk",
  authDomain: "prueba-72f09.firebaseapp.com",
  databaseURL: "https://prueba-72f09.firebaseio.com",
  projectId: "prueba-72f09",
  storageBucket: "prueba-72f09.appspot.com",
  messagingSenderId: "554134964026"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('book');

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
    addBook: function(){
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.success("Libro Eliminado");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
