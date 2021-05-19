<template>
  <div id="todolist">
    <div class="list_todo">
      <div 
        v-for="(value, key) in todos"
        v-bind:key="key"
        v-bind:value="value"
      >
          <p>
              <span class="value">- {{ value }}</span>
              <button @click="editTodo(key)">Edit</button>
          </p>
      </div>  
      <p>
          <input v-model="newTodo">
          <button @click="addTodo">Add Todo</button>
      </p>  
    </div>
    <div class="edit_todo">
      <div v-if="editTodoValue">
          <h3>Edit</h3>
          <p class="todo">{{ editTodoValue }}</p>
          <input v-model="editValue" value="editTodoValue">
          <button @click="updateTodo(editTodoKey)">Update</button>
          <button @click="removeTodo(editTodoKey)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [],
      newTodo: null,
      editValue: null,
      editTodoKey: null,
      editTodoValue: null
    }
  },
  mounted() {
    if (localStorage.getItem('todos')) {
      try {
        this.todos = JSON.parse(localStorage.getItem('todos'))
        console.log(this.todos)
      } catch(e) {
        localStorage.removeItem('todos')
      }
    }
  },
  methods: {
    addTodo() {
      if (!this.newTodo) {
        return
      }
      this.todos.push(this.newTodo)
      console.log(this.todos)
      this.newTodo = ''
      this.saveTodos()
    },
    editTodo(key) {
      this.editTodoKey = key
      this.editTodoValue = this.todos[key]
    },
    updateTodo(key) {
      if (!this.editValue) {
        return
      }
      this.todos.splice(key, 1, this.editValue)
      this.editValue = ''
      this.editTodoValue = null
      this.saveTodos()
    },
    removeTodo(key) {
      this.todos.splice(key, 1)
      this.editTodoValue = null
      this.saveTodos()
    },
    saveTodos() {
      const parsed = JSON.stringify(this.todos)
      localStorage.setItem('todos', parsed)
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#todolist {
  display: flex;
}
.list-todo {
  width: 40%;
}
.edit-todo {
  width: 60%;
  padding: 2em;
}
</style>
