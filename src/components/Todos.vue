<template>
    <section class="todoapp">
        <div class="container">
            <header class="header">
                    <h1 class="text-danger text-uppercase font-weight-bold">TODOS</h1>
                    <div class="form-group">
                        <input v-model="newTodo" @keyup.enter="addTodos" type="text" class="form-control" placeholder="Ajouter une tache">
                    </div>
            </header>
                <div class="card">
                    <div class="card-body">
                        <div class="group-check mb-4 ml-2">
                            <div class="input-group-prepend">
                                <div class="input-group-text mr-2">
                                    <input type="checkbox" v-model="allDone">
                                </div>
                                <span class="text-primary">Sélectionner toutes les taches</span>
                            </div>
                        </div>
                        <ul class="list-group">
                            <li class="list-group-item text-secondary list-unstyled" v-for="todo in filteredTodos" :class="{text_success : todo.completed, edit : editing === todo}">
                                <div class="form-check">
                                    <input class="form-check-input" type="checkbox" v-model="todo.completed">
                                    <label v-show="editing !== todo" class="form-check-label" :class="{text_success : todo.completed}" @dblclick="editTodo(todo)">
                                        {{ todo.name }}
                                    </label>
                                    <input type="text" v-show="editing === todo" v-model="todo.name" class="form-control wd" @keyup.enter="doneEdit" v-focus="editing === todo" @blur="doneEdit" @keyup.esc="cancelEdit()">
                                    <button class="btn-sm float-right btn-danger mr-0" value="supprimer" @click.prevent="delTodo(todo)"><i class="fa fa-trash"></i></button>
                                </div>
                            </li>
                        </ul>
                    </div>
                    <div class="card-footer text-secondary" v-show="todos.length > 0">
                        <div class="pb-5">
                            <span class="text-info"> <strong>{{ remaining }}</strong> Tache à faire</span>
                            <button class="btn-sm btn-dark float-right mt-4 text-white font-weight-bold" v-show="doneTodos"  @click.prevent="delDoTodos">supprimer les taches finies</button>
                        </div>
                        
                        <div class="container">
                            <ul class="nav justify-content-center">
                                <li class="nav-item">
                                    <a href="#" class="nav-link" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a>
                                </li>
                                <li class="nav-item">
                                    <a href="#" class="nav-link" :class="{selected: filter === 'toDo'}" @click.prevent="filter = 'toDo'">A faire</a>
                                </li>
                                <li class="nav-item">
                                    <a href="#" class="nav-link" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    value: {type: Array, default(){return []}}
  },

data(){
        return{
            todos: this.value,
            newTodos: '',
            filter: 'all',
            editing: null,
            oldTodo: ''
        }
    },
    watch: {
        value (value) {
            this.todos = value
        }
    },

    methods: {
        addTodos(){
            if(this.newTodo !== '')
            {
                this.todos.push({
                completed: false,
                name: this.newTodo
                })
            }
            this.newTodo = ''
        },

        delTodo(todo){
            this.todos = this.todos.filter(i => i !== todo)
            this.$emit('input', this.todos)
        },

        delDoTodos(){
            this.todos = this.todos.filter(todo => !todo.completed)
            this.$emit('input', this.todos)
        },

        editTodo(todo){
           this.editing = todo  
            this.oldTodo = todo.name
        },

        doneEdit(){
            this.editing = null
        },

        cancelEdit(){
            this.editing.name  = this.oldTodo
            this.doneEdit()
        }
    },
    computed: {
        allDone: {
            get(){
                return this.remaining === 0
            },

            set(value) {
                this.todos.forEach(todo => {
                    todo.completed = value
                })
            }
        },

        remaining(){
            return this.todos.filter(todo => !todo.completed).length
        },

        filteredTodos(){
            if(this.filter === "toDo")
            {
                return this.todos.filter(todo => !todo.completed)
            } else if(this.filter === "done"){
                return this.todos.filter(todo => todo.completed)
            }
            return this.todos
        },

        doneTodos(){
            return this.todos.filter(todo => todo.completed).length
        }
    },
    directives: {
        focus(el, value){
            if(value){
                Vue.nextTick(_ => {
                    el.focus()
                })
            }
        }
    }
}
</script>
<style src="./bootstrap.css"></style>
<style src="./fontawesome.css"></style>
<style>
.text_success{
    color: #76cc8a !important;
    text-decoration: line-through;
}
.nav-link:hover{
    color: #6c757d;
}
.selected{
    color: rgb(58, 46, 230);
    padding: 2px 5px;
    border: #dddcdc 1px solid;
    margin-top: 5px;
}
.selected:hover{
    color: rgb(58, 46, 230);
}
.edit{
    border: green 2px solid !important;
}
.wd{
    margin-bottom: 5px;
}
</style>
