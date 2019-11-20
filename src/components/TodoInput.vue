<template>
  <div class="todo-input">
    <h2>todo 입력하기</h2>
    <form class="input-group mb-3" @submit.prevent="createTodo">
      <input v-model="title" type="text" class="form-control">
      <button type="submit" class="btn btn-primary">+</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'TodoInput',
  data() {
    return {
      title: '',
    }
  },
  methods: {
    createTodo() {
      this.$emit('createTodoEvent', this.title)
      this.title = ''
      console.log('입력 전송')
    }
  }
}
</script>

<style>

</style>