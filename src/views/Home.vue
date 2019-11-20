<template>
  <div class="home">
    <h1>Todo w/ Django & Vue</h1>
    <TodoInput @createTodoEvent="createTodo" />
    <TodoList :todos="todos"/>
  </div>
</template>

<script>
// @ is an alias to /src
import TodoList from '@/components/TodoList.vue'
import TodoInput from '@/components/TodoInput.vue'
import router from '@/router'
import jwtDecode from 'jwt-decode'
import axios from 'axios'

export default {
  name: 'home',
  data() {
    return {
      todos: []
    }
  },
  components: {
    TodoList,
    TodoInput,
  },
  methods: {
    loggedIn() {
      this.$session.start()
      if (!this.$session.has('jwt')) {
        router.push('/login')
      }
    },
    getTodos() {
      const token = this.$session.get('jwt')
      const user_id = jwtDecode(token).user_id
      const options = {
        headers: {
          Authorization: 'JWT ' + token
        }
      }
      
      axios.get(`http://127.0.0.1:8000/api/v1/users/${user_id}/`, options)
      .then(res => {
        this.todos = res.data.todo_set
      })
    },
    createTodo(title) {
      const token = this.$session.get('jwt')
      const options = {
        headers: {
          Authorization: 'JWT ' + token
        }
      }
      const user_id = jwtDecode(token).user_id

      const data = new FormData()
      // data.append(키, 벨류)
      data.append('user', user_id)
      data.append('title', title)

      console.log(data)

      axios.post('http://127.0.0.1:8000/api/v1/todos/',data, options)
      .then(res => {
        this.todos.push(res.data)
      })
    }
  },
  // 8개의 life cycle hook
  mounted() {
    this.loggedIn()
    this.getTodos()
  }
}
</script>
