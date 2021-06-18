<template>
  <div>
        <h2>Todo List</h2>
        <hr>
        <router-link  to='/' ><span class="home-link"> Home</span></router-link>
        <hr>
        <div class="input-select-position">
        <AddTodo 
          @add-todo='addTodo'
        />

        <select v-model="filter" >
            <option value="all">All</option>
            <option value="completed">Complited</option>
            <option value="not-completed">Not Complited</option>
        </select>
        </div>

        <hr>
        <Loader v-if='loading' />
        <TodoList 
          v-else-if='filteredTodos.length'
          v-bind:todos='filteredTodos'
          @remove-todo='removeTodo'
        />
        <p v-else>No todos</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'

export default {
  name: 'App',
  data(){
    return{
      todos: [],
      loading: true,
      filter: 'all'
    }
  },
//   watch:{
//       filter(value){
//           console.log(value)
//       }
//   },
computed:{
    filteredTodos(){
        if (this.filter === 'all'){
            return this.todos
        }

        if (this.filter === 'completed'){
            return this.todos.filter(t => t.completed)
        }

        if (this.filter === 'not-completed'){
            return this.todos.filter(t => !t.completed)
        }
    }
},
  mounted(){
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
    .then(response => response.json())
    .then(json => {
        setTimeout(() => {
        this.todos = json
        this.loading = false
        },1000)
      
    })
  },
  methods:{
    removeTodo(id){
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo){
      this.todos.push(todo)
    }
  },
  components: {
    TodoList,
    AddTodo,
    Loader
  }
}
</script>


<style scoped>
div{
    display: flex;
    justify-content: center;
    align-content: center;
    flex-direction: column;
}
    select{

        width: 400px;
    }

    .input-select-position{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    a{
        color:red;
        text-decoration: none;
    }
</style>