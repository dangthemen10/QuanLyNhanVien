<template>
  <div class="row-6">
    <div v-if="$route.params.updated == 'yes'" class="alert alert-success">
      Record updated successfully
    </div>
    <nuxt-link to="/department" class="btn btn-secondary ml-5 mb-3"
      >Back</nuxt-link
    >
    <div class="card bg-light ml-5" style="width: 18rem">
      <img
        class="card-img-top"
        src="https://wiki.tino.org/wp-content/uploads/2021/07/word-image-653.png"
        alt="Card image cap"
      />
      <div class="card-body">
        <h5 class="card-title">Department Details</h5>
        <p class="card-text">Information of {{ department.data.deptId }}</p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">Code: {{ department.data.deptId }}</li>
        <li class="list-group-item">Name: {{ department.data.deptName }}</li>
      </ul>
      <div class="card-body">
        <nuxt-link
          :to="'/department/' + department.data.id + '/update'"
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
      '/department/' + context.route.params.id
    )
    return {
      department: data,
    }
  },
  mounted() {
    this.$route.params.updated = 'yes'
  },
  methods: {
    deleteRecord() {
      if (confirm('Are you sure?') === true) {
        this.$axios
          .delete('/department/' + this.$route.params.id)
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
