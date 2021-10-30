<template>
  <div>
    <h1>Add New Department</h1>
    <hr />

    <div class="row-6">
      <div class="col-md-6">
        <form action="" method="post" @submit.prevent="submitForm()">
          <div class="form-group">
            <label for="">Code</label>
            <input
              v-model="deptId"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors }"
            />
            <div v-if="errors" class="invalid-feedback">
              {{ errors.message }}
            </div>
          </div>

          <div class="form-group">
            <label for="">Name</label>
            <input
              v-model="deptName"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors }"
            />
            <div v-if="errors" class="invalid-feedback">
              {{ errors.message }}
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
          this.errors = error
        })
    },
  },
}
</script>
