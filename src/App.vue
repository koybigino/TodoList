<template>
  <div>
    {{ todos }}
    <todos v-model="todos"></todos>
    <todos></todos>
    <todos v-model="todos"></todos>
    <button @click="addTodos">Ajouter une tache</button>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {
  data() {
    return {
      todos: [{
        name: 'Demo',
        completed: true
      }]
    }
  },
  methods: {
    addTodos(){
      this.todos.push({
        name:'Jean',
        completed: false
      })
    }
  },

  components: {
    Todos
  }
}
</script>

