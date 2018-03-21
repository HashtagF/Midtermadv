<template>
  <div class="Home container">
    <h1 class="pb-4">{{msg}}</h1>
    <div class="row justify-content-md-center">
      <div class="col-5">
        <b-form-input type="text"
                      required
                      placeholder="New Todo"
                      >
        </b-form-input>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      msg: 'Todo - Firebase',
      todos: [],
      newTodo: ''
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
