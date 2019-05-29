<template>
  <div id="app">
      <Header />
      <AddTodo v-on:add-todo="addTodo" />
      <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
      
  </div>
</template>

<script>
import Header from "./components/layout/header"
import Todos from "./components/Todos"
import AddTodo from "./components/AddTodo"
import axios from 'axios'

export default {
    name: 'app',
    components: {
        Header,
        Todos,
        AddTodo
    },

    data() {
      return {
        todos: []
      }
    },
    methods: {
      deleteTodo(id) {
        this.todos = this.todos.filter(todo => todo.id != id);
      },
      addTodo(newTodo) {
        const { title, completed } = newTodo;

        axios.post('https://jsonplaceholder.typicode.com/todos', {
          title,
          completed
        })
        .then(res => this.todos = [...this.todos, res.data])
        
      }
    },
    created() {
      fetch('https://jsonplaceholder.typicode.com/todos', {
        method: 'get'
      }). then(function(res) {
            this.todos = res.data
      })
        // axios.get('https://jsonplaceholder.typicode.com/todos')
        // .then(res => this.todos = res.data)
        // .catch(err => console.log(err));
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
* {
  box-sizing: before-box;
  margin: 0;
  padding: 0;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #ffffff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
