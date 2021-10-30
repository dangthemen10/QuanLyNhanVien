<template>
  <div class="row-6">
    <div v-if="$route.params.updated == 'yes'" class="alert alert-success">
      Record updated successfully
    </div>
    <nuxt-link to="/project" class="btn btn-secondary ml-5 mb-3"
      >Back</nuxt-link
    >
    <div class="card bg-light ml-5" style="width: 18rem">
      <img
        class="card-img-top"
        src="https://image.shutterstock.com/image-vector/group-people-icon-260nw-342536540.jpg"
        alt="Card image cap"
      />
      <div class="card-body">
        <h5 class="card-title">Project Details</h5>
        <p class="card-text">Information of {{ project.data.projectName }}</p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">Name: {{ project.data.projectName }}</li>
      </ul>
      <div class="card-body">
        <nuxt-link
          :to="`/project/${project.data.id}/update`"
          class="btn btn-primary ml-4 mr-4"
          >Update</nuxt-link
        >
        <button class="btn btn-danger mr-3" @click="deleteRecord()">
          Delete
        </button>
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
  mounted() {
    this.$route.params.updated = 'yes'
  },
  methods: {
    deleteRecord() {
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete(`/project/${this.$route.params.id}`)
          .then(() => {
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
