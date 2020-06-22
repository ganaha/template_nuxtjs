<template>
  <div>
    <table class="ui celled table">
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in pagination.data" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
    <paginate
      :page-count="pageCount"
      :click-handler="paginate"
      prev-text="Prev"
      next-text="Next"
      container-class="pagination"
    >
    </paginate>
  </div>
</template>

<script>
export default {
  async created() {
    this.pagination = await this.getUsers();
  },
  data() {
    return {
      pagination: {}
    };
  },
  computed: {
    pageCount() {
      return Math.ceil(this.pagination.total / this.pagination.per_page);
    }
  },
  methods: {
    async paginate(num) {
      this.pagination = await this.getUsers(num);
    },
    async getUsers(num = 1) {
      return await this.$axios.get("/api/users", {
        params: {
          page: num
        }
      }).then((res) => {
          return res.data;
      });
    }
  }
};
</script>
