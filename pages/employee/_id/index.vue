<template>
  <div class="container-fluid">
    <h1>Employee Details</h1>
    <hr>
    <div v-if="$route.params.updated == 'yes'" class="alert alert-success">
      Record updated successfully
    </div>
    <nuxt-link to="/employee" class="btn btn-secondary row btn-back"
      >Back</nuxt-link
    >
    <div class="card bg-light border-primary row card-info">
      <img class="card-img-top" src="https://wiki.tino.org/wp-content/uploads/2021/07/word-image-653.png" width="180px" height="180px" alt="Card image cap" />
      <div class="card-body">
        <h5 class="card-title">Employee Infomation</h5>
        <p class="card-text">Information of {{ employee.fullName }}</p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item"><b>Full Name:</b> {{ employee.fullName }}  <span id="tag-b-gender"><b>Gender: </b> {{ employee.gender }}</span></li>
        <li class="list-group-item"><b>Address:</b> {{ employee.address }}  <span id="tag-b-birth"><b>Birthday: </b> {{ employee.dayOfBirth }}</span></li>
        <li class="list-group-item"><b>Email:</b> {{ employee.email }}  <span id="tag-b-phone"><b>Phone: </b> {{ employee.phone }}</span></li>
        <li class="list-group-item"><b >Department: </b> {{ department }} <span id="tag-b-group"><b>Group: </b> {{ group }}</span></li>
        <li class="list-group-item"><b>Project: </b>{{ project }}</li>
      </ul>
      <div class="card-body">
        <nuxt-link
          :to="`/employee/${employee.id}/update`"
          class="btn btn-primary btn-update"
          >Update</nuxt-link
        >
        <button class="btn btn-danger btn-delete" @click="deleteRecord()">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  //   middleware: 'auth',
  async asyncData(context) {
    const { data: dataEmployee } = await context.$axios.get(`/employee/${context.route.params.id}`)
    return {
      employee: dataEmployee.data
    }
  },
  data() {
    return {
      department: null,
      group: null,
      project: null,
    }
  },
  mounted() {
    this.department = this.departmentNames()
    this.project = this.projectNames()
    this.group = this.groupNames()
  },
  methods: {
    departmentNames(){
      this.$axios.get(`/department/${this.employee.departmentId}`)
      .then((response) => {
        this.department = response.data.data.deptName
      })
    },
    projectNames(){
      this.$axios.get(`/project/${this.employee.projectId}`)
      .then((response) => {
        this.project = response.data.data.projectName
      })
    },
    groupNames(){
      this.$axios.get(`/group/${this.employee.groupId}`)
      .then((response) => {
        this.group = response.data.data.groupName
      })
    },
    deleteRecord() {
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete('/employee/' + this.$route.params.id)
          .then(response => {
            if (response.status === 200) {
              this.$router.push({
                name: 'employee',
                params: { deleted: 'yes'},
              })
            }
            this.$router.back()
          })
          .catch((error) => {
            alert(error)
          })
      }
    },
  },
}
</script>