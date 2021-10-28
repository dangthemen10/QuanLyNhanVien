<template>
  <div>
    <h1>Group Edit</h1>
    <hr />

    <div class="row-6">
      <div class="col-md-6">
        <form action="" method="post" @submit.prevent="submitForm()">
          <div class="form-group">
            <label for="">Role</label>
            <input
              v-model="roleGroup"
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
              v-model="groupName"
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
            :to="'/group/' + $route.params.id"
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
      '/group/' + context.route.params.id
    )
    return {
      group: data,
    }
  },
  data() {
    return {
      errors: null,
      roleGroup: null,
      groupName: null,
    }
  },
  mounted() {
    this.roleGroup = this.group.data.roleGroup
    this.groupName = this.group.data.groupName
  },
  methods: {
    submitForm() {
      this.$axios
        .put('/group/' + this.$route.params.id, {
          role: this.roleGroup,
          name: this.groupName,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'group-id',
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