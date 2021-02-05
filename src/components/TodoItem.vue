<template>
    <div class="todo-item">
        <div class="todo-item-left">

            <input type="checkbox" v-model="concluido" @change="doneEdit">

              <div v-if="!editing" class="todo-item-label" :class="{ concluido : concluido }">
                  {{ title }} em
                  {{ time }} com dificuldade
                  {{ level }}
              </div>

            <input v-else class="todo-item-edit" type="text" v-model="title" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.escape="cancelEdit" v-focus>

            </div>
        

          <div class="edit-item">

            <img src="../assets/edit.png"  @click="editTodo">

          </div>

          <div class="remove-item">

            <img src="../assets/delete.png"  @click="removeTodo(todo.id)">

          </div>
    </div>
</template>

<script>

export default {
    name: 'todo-item',
    props: {
        todo: {
            type: Object,
            required: true,
        },
        checkAll: {
            type: Boolean,
            required: true,
        }
    },
    data () {
    return {
        'id': this.todo.id,
        'title': this.todo.title,
        'time': this.todo.time,
        'level': this.todo.level,
        'concluido': this.todo.concluido,
        'editing': this.todo.editing,
        'beforeEditCache': '',

    }
    },
    directives: {
    focus: {
      inserted: function(el) {
        el.focus()
            }
        }
    },
    watch: {
        checkAll() {
            this.concluido = this.checkAll ? true : this.todo.concluido
        }
    },
    methods: {
        removeTodo(id){
            this.$emit('removedTodo', id)
        },
        editTodo() {
            this.beforeEditCache = this.title
            this.beforeEditCache = this.time
            this.editing = true
        },
        doneEdit() {
            if (this.title.trim() == ''){
                this.title = this.beforeEditCache
            }
                this.editing = false
                this.$emit('finishedEdit', {
                    'id': this.id,
                    'title': this.title,
                    'concluido':  this.concluido,
                    'editing': this.editing,      
                
                })
        },
        cancelEdit() {
            this.title = this.todo.title
            this.editing = false
        },
    }  
  }
</script>