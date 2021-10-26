<template>
  <div>
    <h1>Department Article</h1>
    <hr>

    <div class="row">
      <div class="col-md-6">
        <form action=""
          method="post"
          @submit.prevent="submitForm()">

          <div class="form-group">
            <label for="">Code</label>
            <input v-model="deptId" type="text"
              class="form-control"
              :class="{ 'is-invalid': errors && errors.deptId }">
            <div v-if="errors && errors.deptId" class="invalid-feedback">
              {{ errors.deptId.msg }}
            </div>
          </div>

          <div class="form-group">
            <label for="">Name</label>
            <input v-model="deptName" type="text"
              class="form-control"
              :class="{ 'is-invalid': errors && errors.deptName }">
            <div v-if="errors && errors.deptName" class="invalid-feedback">
              {{ errors.deptName.msg }}
            </div>
          </div>

          <input type="submit" value="Submit" class="btn btn-primary mr-3">
          <nuxt-link :to="'/department/' + $route.params.id" class="btn btn-secondary mr-3">Cancel</nuxt-link>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
//   middleware: 'auth',
  async asyncData(context){
    const {data} = await context.$axios.$get('/department/' + context.route.params.id)
    return {
      department : data
    }
  },
  data(){
    return{
      errors:null,
      deptId:null,
      deptName:null,
    }
  },
  mounted(){
    this.deptId = this.department.deptId
    this.deptName = this.department.deptName
  },
  methods:{
    submitForm(){
      this.$axios.put( '/department/' + this.$route.params.id , {
          deptId: this.deptId,
          deptName: this.deptName
        })
        .then((response) => {
          if(response.status === 200){
            this.$router.push({ name:'department-id', params:{ updated:'yes', id: this.$route.params.id } })
          }
        })
        .catch( (error) => {
          alert(error)
        });
    }
  }
}
</script>