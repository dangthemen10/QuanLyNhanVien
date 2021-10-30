<template>
  <div>
    <h1>Add New Employee</h1>
    <hr />

    <form action="" method="post" @submit.prevent="submitForm()">
      <div class="container-fluid">
        <div class="row">
          <div class="col-3">
            <div class="form-group">
              <label for="">Full Name</label>
              <input
                v-model="fullName"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': errors }"
              />
            </div>

            <div class="form-group">
              <label>Gender</label>

              <label class="group-radio">
                <input
                  id="radios"
                  v-model="gender"
                  class="form-check-input"
                  value="Male"
                  type="radio"
                />
                Male
              </label>

              <label class="group-radio1">
                <input
                  id="radios1"
                  v-model="gender"
                  class="form-check-input"
                  value="Female"
                  type="radio"
                />
                Female
              </label>
            </div>

            <div class="form-group">
              <label for="">Birthday</label>
              <input v-model="dayOfBirth" type="date" />
            </div>

            <div class="form-group">
              <label for="">Address</label>
              <input
                v-model="address"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': errors }"
              />
            </div>

            <div class="form-group">
              <label for="">Email</label>
              <input
                v-model="email"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': errors }"
              />
            </div>
          </div>
          <div class="col-3">
            <div class="form-group">
              <label for="">Phone</label>
              <input
                v-model="phone"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': errors }"
              />
            </div>

            <div class="form-group">
              <label for="">Department</label>
              <select v-model="department" class="form-control">
                <option disabled selected>Please, choose department</option>
                <option
                  v-for="dept in departments"
                  :key="dept.id"
                  :value="dept.id"
                >
                  {{ dept.deptName }}
                </option>
              </select>
            </div>

            <div class="form-group">
              <label for="">Group</label>
              <select v-model="group" class="form-control">
                <option disabled selected>Please, choose group</option>
                <option v-for="gr in groups" :key="gr.id" :value="gr.id">
                  {{ gr.groupName }}
                </option>
              </select>
            </div>

            <div class="form-group">
              <label for="">Project</label>
              <select v-model="project" class="form-control">
                <option disabled selected>Please, choose project</option>
                <option v-for="pro in projects" :key="pro.id" :value="pro.id">
                  {{ pro.projectName }}
                </option>
              </select>
            </div>
          </div>
        </div>
        <input type="submit" value="Submit" class="btn btn-primary mr-3" />
        <nuxt-link to="/employee" class="btn btn-secondary mr-3"
          >Cancel</nuxt-link
        >
      </div>
    </form>
  </div>
</template>

<script>
export default {
  //   middleware: 'auth',
  async asyncData({ $axios }) {
    const { data: datadepartments } = await $axios.get('/departments')
    const { data: dataGroups } = await $axios.get('/groups')
    const { data: dataProjects } = await $axios.get('/projects')
    return {
      departments: datadepartments.data,
      groups: dataGroups.data,
      projects: dataProjects.data,
    }
  },
  data() {
    return {
      errors: null,
      fullName: null,
      gender: null,
      dayOfBirth: null,
      address: null,
      email: null,
      phone: null,
      department: null,
      group: null,
      project: null,
    }
  },
  mounted() {
    this.department = 'Please, choose department'
    this.group = 'Please, choose group'
    this.project = 'Please, choose project'
  },
  methods: {
    submitForm() {
      this.$axios
        .post('/employee', {
          fullname: this.fullName,
          gender: this.gender,
          birth: this.dayOfBirth,
          address: this.address,
          email: this.email,
          phone: this.phone,
          department: this.department,
          group: this.group,
          project: this.project,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'employee',
              params: { created: 'yes' },
            })
          }
        })
        .catch((error) => {
          this.errors = error
        })
    },
  },
}
</script>
