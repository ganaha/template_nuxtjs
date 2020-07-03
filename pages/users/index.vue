<template>
  <div>
    <nuxt-link to="/login" class="button--green">Login</nuxt-link>
    <nuxt-link to="/logout" class="button--green">Logout</nuxt-link>

    <h2>Page: {{ page }}</h2>

    <table class="table table-hover">
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in pagination.records" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
    
    <client-only>
      <paginate
        v-model="page"
        :page-count="pageCount"
        :click-handler="paginate"
        prev-text="Prev"
        next-text="Next"
        container-class="pagination"
        prev-class="page-item"
        prev-link-class="page-link"
        page-class="page-item"
        page-link-class="page-link"
        next-class="page-item"
        next-link-class="page-link"
      >
      </paginate>
    </client-only>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pagination: {},
      page: this.$route.query.page || 1
    };
  },
  beforeRouteUpdate (to, from, next) {
    this.page = parseInt(to.query.page);
    next();
  },
  async created() {
    this.pagination = await this.getUsers(this.page);
  },
  computed: {
    pageCount() {
      if (!this.pagination.meta) return 0;
      return Math.ceil(this.pagination.meta.total / this.pagination.meta.per_page);
    }
  },
  methods: {
    async paginate(num) {
      this.pagination = await this.getUsers(num);
      this.$router.push('/users?page=' + num);
    },
    async getUsers(num = 1) {
      return await this.$axios
        .get('/api/users', {
          params: {
            page: num
          }
        })
        .then(res => {
          return res.data;
        })
        .catch((err) => {
          console.log('err', err);
          this.$router.push('/login');
        });
    }
  }
};
</script>
