<template>
  <div>
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
      <p>message: {{ message }}</p>
      <button @click="onClickGetUser" class="button--green">
        ユーザー取得
      </button>
      <nuxt-link to="/users" class="button--green">Users</nuxt-link>
    </div>
  </div>
</template>

<style scope></style>

<script>
export default {
  auth: false,
  data() {
    return {
      post: {
        email: "",
        password: ""
      },
      token: "",
      message: "",
      user: {},
      users: []
    };
  },
  methods: {
    login() {
      this.token = "";
      this.message = "";
      this.getToken()
        .then(res => {
          this.token = res.data;
          this.$axios.setHeader("Authorization", `Bearer ${res.data}`);
        })
        .catch(err => {
          console.log(err);
          this.message = err.message;
        });
    },
    onClickGetUser() {
      this.getUser().then(res => {
        this.message = res.data;
      });
    },
    async getToken() {
      return await this.$axios.post("/api/sanctum/token", this.post);
    },
    async getUser() {
      return await this.$axios.get("/api/user");
    }
  }
};
</script>
