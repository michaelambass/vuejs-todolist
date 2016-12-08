<template>
  <div id="app">

    <Todos v-model="todos"></Todos>
    <!-- <Todos v-model="todos"></Todos> -->

    <button @click="addTodo">Ajouter une tache</button>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {
  data () {
    return {
      todos: [{
        name: 'Demo',
        completed: true
      }]
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        name: 'Jean',
        completed: false
      })
    }
  },
  name: 'app',
  components: {
    Todos
  }
}
</script>
