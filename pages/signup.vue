<template>
  <div>
    <form @submit.prevent="register">
      <div class="row">
        <label class="col-sm-1 col-form-label">E-mail</label>
        <div class="col-sm-2">
          <input
            type="email"
            class="form-control"
            placeholder="name@example.com"
            name="email"
            v-model="post.email"
          />
        </div>
      </div>

      <div class="row">
        <label class="col-sm-1 col-form-label">Password</label>
        <div class="col-sm-2">
          <input
            type="password"
            class="form-control"
            placeholder="Password"
            name="passsword"
            v-model="post.password"
          />
        </div>
      </div>

      <div class="row">
        <label class="col-sm-1 form-label">Confirm</label>
        <div class="col-sm-2">
          <input
            type="password"
            class="form-control"
            placeholder="Password"
            name="password_confirmation"
            v-model="post.password_confirmation"
          />
        </div>
      </div>

      <div class="row">
        <button
          class="col-sm-3 btn btn-primary"
          type="submit"
          :disabled="loading"
        >
          <span v-show="!loading">Register</span>
          <span
            v-show="loading"
            class="spinner-border spinner-border-sm"
            role="status"
            aria-hidden="true"
          ></span>
          <span v-show="loading" class="sr-only">Loading...</span>
        </button>
      </div>

      <div class="row" v-show="message">
        <div class="alert alert-light" role="alert">{{ message }}</div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  auth: "guest",
  data() {
    return {
      post: {
        email: "",
        password: "",
        password_confirmation: ""
      },
      loading: false,
      message: ""
    };
  },
  methods: {
    async register() {
      this.loading = true;
      const res = await this.$axios
        .post("/api/register", this.post)
        .catch(err => {
          this.message = err.message;
        });
      console.log(res);
      this.loading = false;
    }
  }
};
</script>
