<template>
  <div>
  <h1> TODO LIST </h1>
  <div v-if = "!isEditing">
  <label> Add Todo Task: </label>
  <input type="text" v-model="todo" v-on:keyup.enter= "addTodo" value="addTodo" placeholder="what you want to do?">
  <button  @click= "addTodo">Add</button>
  <button @click= "cancelTodo()">Cancel</button>
  <ul> Typing: {{ todo }}</ul>
  </div>
  <div v-else>
   <input type="text" v-model="todo">
  <button type="submit" value="update" @click= "updateTodo">Update</button>
  </div>
  <div>
  <h2> My Current Task - {{ remaining }} </h2>
  <template v-if="todos.length == 0">
  <h3> None </h3>
  </template>
  <template v-else>
  <ol>
     <li v-for="(todo , index) in todos" :key="index"  :class="{ done }"  @click="$emit('on-toggle')">
        {{index + 1}} . {{ todo | capitalize }}
       <button @click="editTodo(index, todo)">Edit</button>
       <button v-on:click="removeTodo(index)">Remove</button>
     </li>
     <button @click= "clearall()">Clear All</button>
  </ol>
  </template>
  </div>
   </div>
</template>

<script>
export default {
  data () {
    return {
      isEditing: false,
      done: false,
      selectedIndex: '',
      todo: '',
      todos: []
    }
  },
  computed: {
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    }
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },
  methods: {
    addTodo () {
      if (this.todo.length > 0) {
        console.log('todo:' + this.todo)
        this.todos.splice(0, 0, this.todo)
        this.todo = ''
      }
    },
    editTodo (index, todo) {
      this.todo = todo
      this.selectedIndex = index
      this.isEditing = true
    },
    updateTodo () {
      this.todos.splice(this.selectedIndex, 1, this.todo)
      this.isEditing = false
      this.todo = ''
    },
    removeTodo (index) {
      this.todos.splice(index, 1)
    },
    cancelTodo () {
      this.todo = ''
    },
    clearall () {
      this.todos = []
    },
    toggleTodo (todo) {
      todo.done = !todo.done
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: blue;
  text-align: center;
}
h3 {
  color: red;
  text-align: center;
}
button {
  border: none;
  background-color: gray;
  color: black;
  padding: 5px 10px;
  text-align: center;
  display: inline-block;
  font-size: 15px;
  margin: 4px 2px;
  cursor: pointer;
}
input {
  padding: 7px 15px;
  border-color: red;
}
label {
  font-size: 24px;
}
.done {
  text-decoration: line-through;
}
</style>
