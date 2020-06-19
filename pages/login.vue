<template>
  <div>
    <form @submit.prevent="login">
      <input
        v-model="post.email"
        type="email"
        placeholder="tarou@example.com"
      />
      <input v-model="post.password" type="password" placeholder="" />
      <input type="submit" value="ログイン" />
    </form>
    <p>token: {{ token }}</p>
    <p>err: {{ message }}</p>
    <button @click="send">ユーザー情報取得</button>
    <table>
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>email</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scope></style>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      post: {
        email: '',
        password: ''
      },
      token: '',
      message: '',
      user: {}
    }
  },
  methods: {
    login() {
      this.token = ''
      this.message = ''
      this.getToken()
        .then((res) => {
          this.token = res.data
        })
        .catch((err) => {
          console.log(err)
          this.message = err.message
        })
    },
    send() {
      this.getUser()
        .then((res) => {
          this.user = res.data
        })
        .catch((err) => {
          this.message = err.message
        })
    },
    async getToken() {
      return await axios.post(
        'http://localhost:8000/api/sanctum/token',
        this.post
      )
    },
    async getUser() {
      return await axios.get('http://localhost:8000/api/user', {
        headers: {
          Authorization: 'Bearer ' + this.token
        }
      })
    }
  }
}
</script>
