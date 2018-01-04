<template>
  <b-container class="hello mt-4 mb-4">
    <h1>{{ msg }}</h1>
      <input class="new-todo" v-model="newTodo" />
    <button @click="addTodo()">add new todo</button>
    <ul v-if="todos" class="todoapp todo-list">
      <li
        v-for="todo in todos"
        class="todo"
        :key="todo.id"
        :class="{ completed: todo.completed}">
        <input class="toggle" type="checkbox" v-model="todo.completed">
        <label>{{ todo.title }}</label>
        <button class="remove" @click="removeTodo(todo)"></button>
      </li>
    </ul>
  </b-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'todo list',
      newTodo: [],
      todos: [],
      errors: []
    }
  },
  methods: {
    addTodo: function () {
      let value = this.newTodo && this.newTodo.trim()
      if (!value) {
        return
      }
      this.todos.push({
        id: (new Date()).getTime(),
        title: value,
        completed: false
      })
      this.newTodo = ''
    },
    removeTodo: function (todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    }
  },
  computed: {
  },
  created () {
    axios.get(`http://jsonplaceholder.typicode.com/todos`)
      .then(response => {
        this.todos = response.data.splice(0, 10)
      })
      .catch(e => {
        this.errors.push(e)
      })
  }
}
</script>

<style scoped lang="scss">
  * {
    font-size: 16px;
  }
  .todoapp {
    background: #fff;
    margin: 10px 0 40px;
    position: relative;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
    &:before {
      content: '';
      position: absolute;
      right: 0;
      bottom: 0;
      left: 0;
      height: 50px;
      overflow: hidden;
      box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6, 0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6, 0 17px 2px -6px rgba(0, 0, 0, 0.2);
    }
  }

  .todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
    li {
      &.completed {
        .toggle::after {
          content: "+";
          font-size: 20px;
          line-height: 1;
          height: 20px;
          width: 20px;
          display: block;
          position: absolute;
          left: 0;
          top: 0;
          bottom: 0;
          right: 0;
          margin: auto;
        }
        label {
          text-decoration: line-through;
        }
      }
      &:hover .remove {
        display: block;
      }
      position: relative;
      font-size: 24px;
      border-bottom: 1px solid #ededed;
      label {
        word-break: break-all;
        padding: 15px 60px 15px 15px;
        margin-left: 45px;
        display: block;
        line-height: 1.2;
        transition: color 0.4s;
      }
      .remove {
        display: none;
        position: absolute;
        top: 0;
        right: 10px;
        bottom: 0;
        width: 40px;
        height: 40px;
        margin: auto 0 11px;
        font-size: 30px;
        color: #cc9a9a;
        transition: color 0.2s ease-out;
        background: none;
        border: none;
        cursor: pointer;
        &::after {
          content: '×';
        }
        &:hover {
          color: #af5b5e;
        }
      }
      .toggle {
        text-align: center;
        width: 40px;
        height: 40px;
        background: none;
        position: absolute;
        top: 0;
        bottom: 0;
        margin: auto 0;
        -webkit-appearance: none;
        appearance: none;
        border: 1px solid #999;
        border-radius: 50%;
      }
    }
  }
</style>
