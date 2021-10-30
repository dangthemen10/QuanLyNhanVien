<template>
  <div>
    <div class="d-flex justify-content-between align-items-center">
      <h1>Group</h1>
      <nuxt-link to="/group/add" class="btn btn-success btn-add"
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

    <div v-if="groups.data.length">
      <table class="table table-hover list-emp">
        <thead class="bg-info">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Role</th>
            <th scope="col">Name</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="group in groups.data" :key="group.id">
            <td>{{ group.id }}</td>
            <td>{{ group.roleGroup }}</td>
            <td>
              <nuxt-link :to="'/group/' + group.id">
                {{ group.groupName }}
              </nuxt-link>
            </td>
            <td>
              <nuxt-link
                :to="'/group/' + group.id + '/update'"
                class="btn btn-primary mr-3"
                >Update</nuxt-link
              >
              <button class="btn btn-danger" @click="deleteRecord(group.id)">
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
    const { data } = await context.$axios.get('/groups')
    return {
      groups: data,
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
          .delete('/group/' + id)
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
