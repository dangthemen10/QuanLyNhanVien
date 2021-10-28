<template>
  <div>
    <h1>Project Edit</h1>
    <hr />

    <div class="row-6">
      <div class="col-md-6">
        <form action="" method="post" @submit.prevent="submitForm()">

          <div class="form-project">
            <label for="">Name</label>
            <input
              v-model="projectName"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': errors }"
            />
            <div v-if="errors" class="invalid-feedback">
              {{ errors.message }}
            </div>
          </div>

          <input type="submit" value="Submit" class="btn btn-primary mr-2 mt-3" />
          <nuxt-link
            :to="`/project/${$route.params.id}`"
            class="btn btn-secondary mt-3"
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
      `/project/${context.route.params.id}`
    )
    return {
      project: data,
    }
  },
  data() {
    return {
      errors: null,
      projectName: null,
    }
  },
  mounted() {
    this.projectName = this.project.data.projectName
  },
  methods: {
    submitForm() {
      this.$axios
        .put(`/project/${this.$route.params.id}`, {
          name: this.projectName,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'project-id',
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