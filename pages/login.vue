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
      <button @click="getUser" class="button--green">
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
      this.token = res.data;
      this.$router.push('/');
    },
    getUser() {
      console.log(this.$auth);
      this.message = this.$auth.user;
    }
  }
};
</script>
