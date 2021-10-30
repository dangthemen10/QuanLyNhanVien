<template>
  <div>
    <h1>Department Article</h1>
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
              <label id="gender"
                >Gender(<small id="tag-gender">{{ gender }}</small
                >)</label
              >

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
              <small id="tag-birth">Birthday: {{ dayOfBirth }}</small
              ><br />
              <label>Birthday</label>
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
  async asyncData(context) {
    const { data: dataEmployee } = await context.$axios.get(
      '/employee/' + context.route.params.id
    )
    const { data: dataDepartments } = await context.$axios.get('/departments')
    const { data: dataProjects } = await context.$axios.get('/projects')
    const { data: dataGroups } = await context.$axios.get('/groups')
    return {
      employee: dataEmployee.data,
      departments: dataDepartments.data,
      projects: dataProjects.data,
      groups: dataGroups.data,
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
    this.fullName = this.employee.fullName
    this.gender = this.employee.gender
    this.dayOfBirth = this.employee.dayOfBirth
    this.address = this.employee.address
    this.email = this.employee.email
    this.phone = this.employee.phone
    this.department = this.employee.departmentId
    this.group = this.employee.groupId
    this.project = this.employee.projectId
  },
  methods: {
    submitForm() {
      this.$axios
        .put('/employee/' + this.$route.params.id, {
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
              name: 'employee-id',
              params: { updated: 'yes', id: this.$route.params.id },
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
