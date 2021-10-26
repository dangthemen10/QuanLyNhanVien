<template>
  <div>
    <h1>Department Details</h1>

    <hr />

    <div v-if="$route.params.updated == 'yes'" class="alert alert-success">
      Record updated successfully
    </div>

    <h2>{{ department.data.deptId }}</h2>

    <h2>{{ department.data.deptName }}</h2>

    <hr />
    <div class="d-flex justify-content-between">
      <div>
        <nuxt-link
          :to="'/department/' + department.data.id + '/update'"
          class="btn btn-primary mr-3"
          >Update</nuxt-link
        >
        <button class="btn btn-danger" @click="deleteRecord()">Delete</button>
      </div>
      <nuxt-link to="/department" class="btn btn-secondary mr-3"
        >Back to Department</nuxt-link
      >
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