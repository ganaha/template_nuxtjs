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
      <button @click="getUser" class="button--green">ユーザー取得</button>
      <nuxt-link to="/users" class="button--green">Users</nuxt-link>
      <nuxt-link to="/signup" class="button--green">Sign up</nuxt-link>
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
      user: {}
    };
  },
  methods: {
    async login() {
      const res = await this.$auth
        .loginWith("local", { data: this.post })
        .catch(err => {
          this.message = err.message;
        });
      this.token = res.data.record.token;
      // this.$router.push('/');
    },
    getUser() {
      this.message = this.$auth.user;
    }
  }
};
</script>
