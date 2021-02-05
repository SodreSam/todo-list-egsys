<template>

  <div class="container">
      
      <h2> Insira sua tarefa abaixo</h2>

     
      <input type="text" class="todo-list" placeholder="O que precisa ser feito?" v-model="newTodo">

      <input type="text" class="time" placeholder="Digite o tempo da tarefa no padrão 00h00m00s" v-model="time">

      <select  class="level" placeholder="Selecione a dificuldade" v-model="level">

          <option value="Selecione o nível de dificuldade da tarefa" selected> Selecione o nível de dificuldade da tarefa</option>
          <option value="Fácil">Fácil </option>
          <option value="Médio">Médio </option>
          <option value="Difícil">Difícil </option>

      </select> 

      <button type="submit" class="addtodo" v-on:click="addTodo">Adicionar tarefa</button> 

      <div>
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
          <todo-item v-for="todo in todosFilter" :key="todo.id" :todo="todo" :checkAll="!nRestantes" @removedTodo="removeTodo" @finishedEdit="finishedEdit"></todo-item>
        </transition-group>

          <div class="container-two">
              <div>
                <label>
                  <input type="checkbox" :checked="!nRestantes" @change="checkAllTodos" >
                  Marcar todas as tarefas
                  </label>
              </div>   
                <div> {{ restantes }} itens restantes </div>          
          </div>
      </div>

        <div class="container-two">
          <div>
            <button :class="{ active: filter == 'all' }" @click="filter = 'all'">Todas</button>

            <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Tarefas ativas</button>

            <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Concluídas</button>

          </div>

          <div>
            <transition name="fade">
                <button v-if="clearButton" @click="clearCompleted">Limpar tarefas concluídas</button>
            </transition>
          </div>
        </div>
  </div>

</template>

<script>
import TodoItem from './TodoItem'


export default {
  name: 'todo-list',
  components: {
    TodoItem,
  },
    data () {
    return {
      newTodo: '',
      idForTodo: 1,
      filter: 'all',
      todos: [],
      level: '',
      time: '',
    }
  },
  computed: {
    restantes() {
      return this.todos.filter( todo => !todo.concluido).length
    },
    nRestantes() {
      return this.restantes != 0
    },
    todosFilter() {
      if (this.filter == 'all'){

        return this.todos

      } else if (this.filter == 'active') {
        
        return this.todos.filter(todo => !todo.concluido)

      } else if ( this.filter == 'completed'){
        
        return this.todos.filter(todo => todo.concluido)
      }

      return this.todos

    },
    clearButton(){
      return this.todos.filter(todo => todo.concluido).length > 0
    },
  },
  methods: {
      addTodo(){

        if (this.newTodo.trim().length == 0) {
            return
        }
          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo, 
              time: this.time,
              level: this.level,
              concluido: false,
              editing: false,
      })
      this.newTodo = ''
      this.time = ''
      this.level= ''
      this.idForTodo++
      },      
       removeTodo(id) {
      const index = this.todos.findIndex((item) => item.id == id)
      this.todos.splice(index, 1)
      },
      checkAllTodos() {
        this.todos.forEach((todo) => todo.concluido = event.target.checked)
      },
      clearCompleted(){
        this.todos = this.todos.filter(todo => !todo.concluido)
      },
      finishedEdit(data){
       const index = this.todos.findIndex((item) => item.id == data.id)
       this.todos.splice(index, 1, data)
      }
  }
}


</script>

<style>

@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

@import './TodoList.css';

</style>