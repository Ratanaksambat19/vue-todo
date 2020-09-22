<template>
    
    <div class="nav-wrapper container">
      <form>
        <div class="input-field">
          <input id="search" type="search" required
                 v-model="newTodo"
                 @keypress="addTodo">
          <label class="label-icon" for="search"><i class="material-icons">Todo...</i></label>
        </div>
      </form>
      <div v-for="(todo, index) in todos"
            :key="todo.id"
            class="todo-item">

            <div class="card">
                <div class="todo-item-left">
                    <div class="card-content"
                         v-if="!todo.editing"
                         @dblclick="editTodo(todo)">
                        <p class="card-title">{{ todo.title }}</p>
                    </div>
                    <input type="text" 
                           v-model="todo.title" 
                           v-else
                           @blur="doneEdit(todo)"
                           @keyup.enter="doneEdit(todo)"
                           v-focus
                           class="card-title-edit">
                </div>
                
                <div class="card-action">
                    <p>
                    <label>
                        <input type="checkbox" v-model="todo.completed">
                        <span></span>
                    </label>
                    </p>
                    <a href="#" class="delete-btn"
                       @click="removeTodo(index)">Delete</a>
                </div>
            </div>
      </div>
      <div class="container">
          
              <label>
                  <input type="checkbox"
                        :checked="!anyRemaining"
                        @change="checkAllTodos"> 
                  <span>Check All</span>
              </label>
          
          {{ remaining }} items left
      </div>
    </div>
    
  
</template>

<script>
export default {
    name: "todo-list",
    data() {
        return {
            newTodo: "",
            idForTodo: 3,
            todos: [
                {
                    'id': 1,
                    'title': 'Finished Vue',
                    'completed': false,
                    'editing':false
                },
                {
                    'id': 2,
                    'title': 'Finished react',
                    'completed': true,
                    'editing':false
                },
            ]
        }
    },

    methods: {
        addTodo(e) {
            if(this.newTodo.trim().length == 0) {
                return
            }
            if (e.key == 'Enter') {
                this.todos.push({
                id: this.idForTodo,
                title: this.newTodo,
                completed: false,
            })

            this.newTodo = ''
            this.idForTodo++
            }
        },

        removeTodo(index) {
            this.todos.splice(index, 1)
        },

        editTodo(todo) {
            todo.editing = true
        },

        doneEdit(todo) {
                todo.editing = false
        },

        checkAllTodo() {
            this.todos.forEach((todo) => todo.completed = 
            event.target.checked)
        }
    },
    
    directives: {
        focus: {
            inserted: function (el) {
                el.focus()
            }
        }
    },

    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },

        anyRemaining() {
            return this.remaining != 0
        }
    },
}
</script>