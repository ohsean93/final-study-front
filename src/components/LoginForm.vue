<template>
  <div>
    <div class="form-group">
      <label for="id">아이디</label>
      <input v-model="credentials.username" type="text" id="id" class="form-control" placeholder="아이디를 입력해 주세요!">
    </div>
    <div class="form-group">
      <label for="password">비밀번호</label>
      <input v-model="credentials.password" type="password" id="password" class="form-control" placeholder="비밀번호를 입력해 주세요!">
    </div>
    <button @click="login" class="btn btn-primary">로그인</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'LoginForm',
  data() {
    return {
      credentials: {},
    }
  },
  methods: {
    login() {
      console.log(this.credentials)
      axios.post('http://127.0.0.1:8000/api-token-verify/', this.credentials)
      .then(res => {
        console.log(res.data.token)
      })
      // .catch(err => {})
    },
  }
}
</script>

<style>

</style>