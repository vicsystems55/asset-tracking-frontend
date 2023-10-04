<template>
  <div>

    <div class="container">
      <div class="row">
        <div class="col-md-12">

          <div class="mx-auto py-5 text-center">
            <router-link :to="'/create-submission'">
              <buttons class="btn btn-primary btn-lg">
                Create Submission
              </buttons>
            </router-link>
          </div>

          <div class="card">
            <div class="card-body table-responsive">

              <table class="table">
                <thead>
                  <tr>
                    <th>#</th>
                    <th>Tracking ID</th>
                    <th>Department</th>
                    <th>Date</th>
                    <th>Status</th>
                    <th />
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="submission,key in submissions" :key="submission.index">
                    <td>{{ key +1 }}</td>
                    <td>
                      <router-link :to="'/submission/'+submission.id">
                        {{submission.tracking_code}}
                      </router-link>

                    </td>
                    <td>{{submission.office.name}}</td>
                    <td>{{submission.date}}</td>
                    <td>
                      <span class="badge badge-primary">pending</span>
                    </td>

                  </tr>
                </tbody>
              </table>

            </div>
          </div>

        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  components: {

  },

  data() {
    return {
      submissions: [],

    }
  },

  mounted() {
    this.getSubmissions()
  },

  methods: {

    getSubmissions() {
      axios({
        url: `${process.env.VUE_APP_BACKEND_URL}/api/visitor-submissions`,
        method: 'get',
      }).then(res => {
        console.log(res)
        this.submissions = res.data
      }).catch(error => {
        console.log(error)
      })
    },

  },

}
</script>

<style></style>
