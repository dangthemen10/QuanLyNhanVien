<template>
  <div>
    <div class="d-flex justify-content-between align-items-center">
      <h1>Department</h1>
      <nuxt-link to="/department/add" class="btn btn-success btn-add"
        >Add New</nuxt-link
      >
    </div>
    <hr />

    <div v-if="$route.params.created == 'yes'" class="alert alert-success">
      Record added successfully
    </div>
    <div v-if="$route.params.deleted == 'yes'" class="alert alert-success">
      Record deleted successfully
    </div>

    <div v-if="departments.data.length">
      <table class="table table-hover list-emp">
        <thead class="bg-info">
          <tr>
            <th scope="col">#</th>
            <th scope="col">ID</th>
            <th scope="col">Name</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="dept in departments.data" :key="dept.id">
            <td>{{ dept.id }}</td>
            <td>{{ dept.deptId }}</td>
            <td>
              <nuxt-link :to="'/department/' + dept.id">
                {{ dept.deptName }}
              </nuxt-link>
            </td>
            <td>
              <nuxt-link
                :to="'/department/' + dept.id + '/update'"
                class="btn btn-primary mr-3"
                >Update</nuxt-link
              >
              <button class="btn btn-danger" @click="deleteRecord(dept.id)">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-else class="alert alert-info">No records found.</div>
  </div>
</template>

<script>
export default {
  async asyncData(context) {
    const {data} = await context.$axios.get('/departments')
    return {
      departments: data
    }
  },
  mounted() {
    this.$route.params.created = 'no'
    this.$route.params.deleted = 'yes'
  },
  methods: {
    deleteRecord(id) {
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete(`/department/${id}`)
          .then(() => {
            this.$router.app.refresh()
          })
          .catch((error) => {
            alert(error)
          })
      }
    },
  },
}
</script>
