<template>
  <div>
    <h1>Add New Group</h1>
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

          <input type="submit" value="Submit" class="btn btn-primary mr-3" />
          <nuxt-link to="/group" class="btn btn-secondary mr-3"
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
      roleGroup: null,
      groupName: null,
    }
  },
  methods: {
    submitForm() {
      this.$axios
        .post('/group', {
          role: this.roleGroup,
          name: this.groupName,
        })
        .then((response) => {
          if (response.status === 200) {
            this.$router.push({
              name: 'group',
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
