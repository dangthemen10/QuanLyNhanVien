<template>
  <div>
    <div class="d-flex justify-content-between align-items-center">
      <h1>Employee</h1>
      <form id="form-search" action="" method="post" @submit.prevent="filtered()">
        <div class="input-group">
          <div class="form-outline">
            <input id="search-input"
                v-model="search"
                type="search"
                class="form-control"
              />
          </div>
          <button id="search-button" type="submit" class="btn btn-primary">
            Search
          </button>
      </div>
    </form>
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

    <div v-if="!employees.data.length" class="alert alert-info">No records found.</div>
      <table class="table table-hover list-emp" >
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
          <tr v-for="emp in filteredList" :key="emp.id">
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

    <div class="btn-wrapper">
      <button class="btn-paging" type="button" :disabled="currentPage === 1" @click="changePage(-1)">Previous</button>
      <button class="btn-paging" type="button" :disabled="currentPage === 4" @click="changePage(1)">Next</button>
    </div>
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
  data() {
    return{
      search: '',
      pages: [],
      prePage: 5,
      currentPage: 1
    }
  },
  computed: {
    filteredList() {
      const star = (this.currentPage - 1) * this.prePage
      const end = this.currentPage * this.prePage
      const result = this.employees.data.slice(star, end)
      return result
    }
  },
  methods: {
    filtered(){
      this.$axios.get(`filter?name=${this.search}`).then((response) => {
        this.list = this.list.concat(response.data.data)
        this.employees = response.data
      })
    },
    deleteRecord(id) {
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete(`/employee/${id}`)
          .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'employee',
              params: { deleted: 'yes', created: 'no'}
            })
          }
          this.$router.app.refresh()
        })
          .catch((error) => {
            alert(error.message)
          })
      }
    },
    changePage(num) {
      this.currentPage = this.currentPage + num
    }
  }
}
</script>
