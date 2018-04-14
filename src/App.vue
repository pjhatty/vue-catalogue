<template>
  <div id="app" class="container">
    <img width="25%" src="./assets/logo.png">
    <HelloWorld/>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add a New Book</h3>
      </div>
      <div class="panel-body">
         <form id="form" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Book List</h3>
      </div>
      <div class="panel-body">
        <table class="table  table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books" :key="book.index">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><span class="glyphicon glyphicon-trash" aria-hidden="true" @click="removeBook(book)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import Firebase from "firebase";

let config = {
  apiKey: "AIzaSyDuuHdUKc46a1D6klEEQrYpv4XE9sr3Z_k",
  authDomain: "cataloguesystem-6761f.firebaseapp.com",
  databaseURL: "https://cataloguesystem-6761f.firebaseio.com",
  projectId: "cataloguesystem-6761f",
  storageBucket: "cataloguesystem-6761f.appspot.com",
  messagingSenderId: "761001122539"
};

let app = Firebase.initializeApp(config);
let db = app.database();
let booksRef = db.ref("books");

export default {
  name: "app",
  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: "",
        author: "",
        url: "https://"
      }
    };
  },
  methods: {
    addBook() {
      booksRef.push(this.newBook);
      this.newBook.title = "";
      this.newBook.author = "";
      this.newBook.url = "https://";
    },
    removeBook(book) {
      booksRef.child(book[".key"]).remove();
    }
  },
  components: {
    HelloWorld
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table {
  text-align: left;
}
.form-group {
  text-align: left;
}
</style>
