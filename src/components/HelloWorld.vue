<template>
    <b-container class="mt-4 mb-4">
        <div class="rp-todoapp">
            <div class="rp-add">
                <input class="rp-newtodo" v-model="newTodo" @keyup.enter="addTodo" title="press enter for add item"/>
            </div>
            <ul v-if="todos" class="rp-todolist">
                <li
                        v-for="todo in todos"
                        class="rp-todo"
                        :key="todo.id"
                        :class="{ completed: todo.completed}">
                    <input class="rp-toggle" type="checkbox" v-model="todo.completed">
                    <label>{{ todo.title }}</label>
                    <button class="rp-remove" @click="removeTodo(todo)"></button>
                </li>
            </ul>
        </div>
    </b-container>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'HelloWorld',
    data () {
      return {
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
        font-size: 12px;
        color: #8b8f97;
        outline: none;
    }
    .rp-todoapp {
        width: 250px;
        margin: auto;
        background: #fff;
        position: relative;
        box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
        border: 1px solid rgba(0, 0, 0, 0.2);
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

    .rp-add {
        position: relative;
        padding-left: 50px;
        &::before {
            content: '';
            position: absolute;
            background: url("../assets/img/icon-add.png") no-repeat;
            height: 8px;
            width: 8px;
            left: 55px;
            top: 43%;
        }
        .rp-newtodo {
            border: none;
            width: 100%;
            cursor: pointer;
            height: 38px;
            padding-left: 20px;
            &:focus {
                box-shadow: 0 0 4px 2px #80bdff;
                cursor: default;
            }
        }
    }

    .rp-todolist {
        margin: 0;
        padding: 0;
        list-style: none;
        position: relative;
        &::after,
        &::before {
            content: '';
            width: 1px;
            height: calc(100% + 38px);
            background: #f2e3df;
            position: absolute;
            top: -38px;
            left: 38px;
        }
        &::after {
            left: 42px;
        }
        li {
            position: relative;
            font-size: 24px;
            border-bottom: 1px solid #e6ebed;
            padding-right: 16px;
            &:first-child {
                border-top: 1px solid #e6ebed;
            }
            &.completed {
                .rp-toggle {
                    background-position: 100% 0;
                }
                label {
                    text-decoration: line-through;
                }
            }
            &:hover .rp-remove {
                display: block;
            }
            label {
                padding: 8px 0;
                margin-left: 50px;
                margin-bottom: 0;
                display: block;
                line-height: 1;
                transition: color 0.4s;
            }
            .rp-remove {
                display: none;
                position: absolute;
                top: 0;
                right: 10px;
                bottom: 0;
                width: 10px;
                height: 10px;
                margin: auto;
                transition: color 0.2s ease-out;
                background: none;
                border: none;
                cursor: pointer;
                &::after {
                    content: '';
                    background: url("../assets/img/icon-trash.png") no-repeat center;
                    height: 12px;
                    width: 11px;
                    display: block;
                }
                &:hover {
                    color: #af5b5e;
                }
            }
            .rp-toggle {
                text-align: center;
                width: 17px;
                height: 17px;
                background: url("../assets/img/icon-checkbox.png") no-repeat;
                background-position: 0 0;
                position: absolute;
                top: 0;
                left: 12px;
                bottom: 0;
                margin: auto 0;
                -webkit-appearance: none;
                appearance: none;
                cursor: pointer;
            }
        }
    }
</style>
