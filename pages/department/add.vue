<template>
  <div>
    <h1>Add New Department</h1>
    <hr />

    <div class="row">
      <div class="col-md-12">
        <form action="" method="post" @submit.prevent="submitForm()">
          <div class="form-group">
            <label for="">Code</label>
            <input
              v-model="deptId"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors && errors.deptId }"
            />
            <div v-if="errors && errors.deptId" class="invalid-feedback">
              {{ errors.deptId.message }}
            </div>
          </div>

          <div class="form-group">
            <label for="">Name</label>
            <input
              v-model="deptName"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors && errors.deptName }"
            />
            <div v-if="errors && errors.deptName" class="invalid-feedback">
              {{ errors.deptName.message }}
            </div>
          </div>

          <input type="submit" value="Submit" class="btn btn-primary mr-3" />
          <nuxt-link to="/department" class="btn btn-secondary mr-3"
            >Cancel</nuxt-link
          >
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  //   middleware: 'auth',
  data() {
    return {
      errors: null,
      deptId: null,
      deptName: null,
    }
  },
  methods: {
    submitForm() {
      this.$axios
        .post('/department', {
          deptId: this.deptId,
          deptName: this.deptName,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'department',
              params: { created: 'yes' },
            })
          }
        })
        .catch((error) => {
          alert(error)
        })
    },
  },
}
</script>