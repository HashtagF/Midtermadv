<template>
  <div class="Home container">
    <img src="../assets/logo.svg" height="160px" width="auto">
    <div class="row justify-content-md-center">
      <div class="col-5">
        <b-alert variant="success"
                  :show="dismissCountDown"
                  dismissible
                  @dismissed="dismissCountdown=0"
                  @dismiss-count-down="countDownChanged">
            Add Todo Success {{dismissCountDown}} seconds...
          </b-alert>
        <b-form-input type="text"
                      placeholder="New Todo"
                      @keyup.native.enter="addTodoAndClearNewTodo"
                      icon="check"
                      size="is-large"
                      v-model="newTodo">
        </b-form-input>
        <div class="showTodos pt-3 " v-for="(todo, index) in todos" :key="index" >
          <label class="h4">   {{index + 1}}. {{todo.title}}</label>
          <button type="button" class="close float-right clearfix" aria-label="Close" @click="removeTodo(todo)">
            <span aria-hidden="true">&times;</span>
          </button>
          <div v-if="todo.complete">
            <b-form-input type="text"
                          placeholder="New Todo"
                          @keyup.native.enter="updateTodo(todo, todo.title)"
                          icon="check"
                          size="is-large"
                          v-model="todo.title">
            </b-form-input>
          </div>
          <button @click="editTodo(todo)" type="button" class="close float-right clearfix" >
            edit
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCmWtP5PY9Jezs94HjrzvfdxNIAhKcDDXU',
  authDomain: 'midtermadv.firebaseapp.com',
  databaseURL: 'https://midtermadv.firebaseio.com',
  projectId: 'midtermadv',
  storageBucket: 'midtermadv.appspot.com',
  messagingSenderId: '498210216713'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let todosRef = db.ref('todos')

export default {
  name: 'Home',
  firebase: {
    todos: todosRef
  },
  data () {
    return {
      newTodo: '',
      dismissSecs: 3,
      dismissCountDown: 0,
      edit: false
    }
  },
  methods: {
    addTodoAndClearNewTodo () {
      todosRef.push({
        title: this.newTodo,
        complete: false
      })
      this.newTodo = ''
      this.showAlert()
    },
    removeTodo (todo) {
      todosRef.child(todo['.key']).remove()
    },
    editTodo (todo) {
      todosRef.child(todo['.key'] + '/complete').set(true)
    },
    updateTodo (todo, text) {
      todosRef.child(todo['.key'] + '/title').set(text)
      todosRef.child(todo['.key'] + '/complete').set(false)
    },
    countDownChanged (dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    },
    showAlert () {
      this.dismissCountDown = this.dismissSecs
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
