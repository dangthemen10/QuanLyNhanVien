<template>
  <div>
    <h1>Department Article</h1>
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

          <input type="submit" value="Submit" class="btn btn-primary mr-2" />
          <nuxt-link
            :to="'/department/' + $route.params.id"
            class="btn btn-secondary"
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
  async asyncData(context) {
    const { data } = await context.$axios.get(
      '/department/' + context.route.params.id
    )
    return {
      department: data,
    }
  },
  data() {
    return {
      errors: null,
      deptId: null,
      deptName: null,
    }
  },
  mounted() {
    this.deptId = this.department.data.deptId
    this.deptName = this.department.data.deptName
  },
  methods: {
    submitForm() {
      this.$axios
        .put('/department/' + this.$route.params.id, {
          deptId: this.deptId,
          deptName: this.deptName,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'department-id',
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