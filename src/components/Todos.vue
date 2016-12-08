<template>

  <section class="todoapp">

    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" v-model="newTodo" placeholder="Ajouter une tache" @keyup.enter="addTodo" />
    </header>
    <div class="main">
      <input type="checkbox" class="toggle-all" v-model="allDone" />
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :class="{completed : todo.completed, editing : todo === editing}">
          <div class="view">
            <input type="checkbox" v-model="todo.completed" class="toggle">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" v-focus="todo === editing" @blur="doneEdit" @keyup.esc="cancelEdit" />
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
      <ul class="filters">
        <li :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'"><a href="#">Toutes les tâches</a></li>
        <li :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'"><a href="#">A faire</a></li>
        <li :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'"><a href="#">Tâches effectuées</a></li>
      </ul>
      <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer finies</button>
    </footer>
  </section>

</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      todos: this.value,
      newTodo: '',
      filter: 'all', // done, todo, all
      allDone: false,
      editing: null,
      oldTodo: ''
    }
  },
  props: {
    value: {type: Array, default () { return [] }}
  },
  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })

      this.newTodo = ''
    },

    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
      this.$emit('input', this.todos)
    },

    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
      this.$emit('input', this.todos)
    },

    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },

    doneEdit () {
      this.editing = null
    },

    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    }

  },
  watch: {
    value (value) {
      this.todos = value
    }
  },
  computed: {

    allDone: {
      get () {
        this.remaining === 0
      },

      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },

    hasTodos () {
      return this.todos.length > 0
    },

    completed () {
      return this.todos.filter(todo => todo.completed).length
    },

    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },

    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    }
  },

  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(_ => {
          el.focus()
        })
      }
    }
  }

}
</script>

<style src="./todos.css"></style>
