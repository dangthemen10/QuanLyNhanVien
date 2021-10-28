<template>
  <div>
    <div class="d-flex justify-content-between align-items-center">
      <h1>Employee</h1>
      <nuxt-link to="/employee/add" class="btn btn-success btn-add"
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

    <div v-if="employees.data.length">
      <table class="table table-hover list-emp">
        <thead class="bg-info">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Full Name</th>
            <th scope="col">Gender</th>
            <th scope="col">Birthday</th>
            <th scope="col">Address</th>
            <th scope="col">Email</th>
            <th scope="col">Phone</th>
            <th scope="col">Department</th>
            <th scope="col">Group</th>
            <th scope="col">Project</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="emp in employees.data" :key="emp.id">
            <td>{{ emp.id }}</td>
            <td>
              <nuxt-link :to="`/employee/${emp.id}`">
                {{ emp.fullName }}
              </nuxt-link>
            </td>
            <td>{{ emp.gender }}</td>
            <td>{{ emp.dayOfBirth }}</td>
            <td>{{ emp.address }}</td>
            <td>{{ emp.email }}</td>
            <td>{{ emp.phone }}</td>
            <td>{{ emp.deptName }}</td>
            <td>{{ emp.groupName }}</td>
            <td>{{ emp.projectName }}</td>
            <td>
              <nuxt-link
                :to="`/employee/${emp.id}/update`"
                class="btn btn-primary mr-2"
                >Update</nuxt-link
              >
              <button class="btn btn-danger" @click="deleteRecord(emp.id)">
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
    const {data} = await context.$axios.get('/employees')
    return {
      employees: data
    }
  },
  mounted() {
    this.$route.params.created = 'no'
    this.$route.params.deleted = 'yes'
  },
  methods: {
    deleteRecord(id) {
      alert(id)
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete(`/employee/${id}`)
          .then(() => {
            this.$router.app.refresh()
          })
          .catch((error) => {
            alert(error.message)
          })
      }
    },
  }
}
</script>