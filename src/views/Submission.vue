<template>
  <div>

    <div class="containe">
      <div class="">
        <div class="">


            <div class="text-center py-2">
              <a href="" class="btn btn-warning" aria-disabled="">Issue Authorization Letter</a>

            </div>

          <div class="scroll-card   overflow-auto  p-2">

            <div 
              style="min-height: 150px; min-width: 300px;" class="card  m-1 border border-prim ard   bg-succe ">
              <div class="card-body">

                <h6 class="">Contractor Name: <span style="font-weight: bold;" class="font-weight-bold"> {{
                  submission.contractor_name}}</span></h6>
           


              </div>

              <!-- <app-collapse v-if="userData.role != 'visitor'" accordion>
                <app-collapse-item title="Action">
                  <div class="form-group">
                    <label for="">Remark</label>
                    <textarea v-model="remark" type="text" col="" row="5" class="form-control"></textarea>
                  </div>
                  <div class="form-group">
                    <label for="">Select Department:</label>
                    <select id="" v-model="office_id" class="form-control">
                      <option v-for="office in offices" :key="office.index" :value="office.id">
                        {{ office.name }}
                      </option>
                    </select>
                  </div>
                  <div class="form-group">
                    <button @click="dispatchSubmission()" class="btn btn-primary">Dispatch</button>
                  </div>
                </app-collapse-item>

              </app-collapse> -->



            </div>

            <div style="height: 150px; max-width: 300px;" class="card card-body m-1 border border-primary ard ">
              <h6></h6>
            </div>





          </div>

        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import AppCollapse from '@core/components/app-collapse/AppCollapse.vue'
import AppCollapseItem from '@core/components/app-collapse/AppCollapseItem.vue'
import ToastificationContent from '@core/components/toastification/ToastificationContent.vue'


export default {
  components: {

    AppCollapse,
    AppCollapseItem,
  },
  data() {
    return {
      submissionStatuses: [],
      submission: [],
      offices: [],
      office_id: '',
      remark: '',
      userData: '',


    }
  },
  mounted() {
    this.userData = JSON.parse(localStorage.getItem('user_data'));

    // this.getSubmissionStatus()
    this.getSubmission()
  },

  methods: {
    getSubmission() {
      axios({
        url: `${process.env.VUE_APP_BACKEND_URL}/api/contractor-applications/${this.$route.params.id}`,
        method: 'get',
      }).then(res => {
        console.log(res)
        this.submission = res.data
      }).catch(error => {
        console.log(error)
      })
    },
    getSubmissionStatus() {
      axios({
        url: `${process.env.VUE_APP_BACKEND_URL}/api/submission-statuses`,
        method: 'get',
        params: {
          visitors_submission_id: this.$route.params.id,
        },
      }).then(res => {
        console.log(res)
        this.submissionStatuses = res.data
      }).catch(error => {
        console.log(error)
      })
    },

    dispatchSubmission() {
      axios({
        url: `${process.env.VUE_APP_BACKEND_URL}/api/visitor-submissions`,
        method: 'post',
        headers: {
          'Authorization': 'Bearer ' + localStorage.getItem('token')
        },
        data: {
          visitors_submission_id: this.$route.params.id,
          dispatch: 'true',
          remark: this.remark,
          office_id: this.office_id,
        },
      }).then(res => {
        console.log(res)

        this.getSubmissionStatus()

        this.$toast({
          component: ToastificationContent,
          props: {
            title: 'Submission dispatched',
            icon: 'EditIcon',
            variant: 'success',
          },
        })
      }).catch(error => {
        console.log(error)
      })
    },
  },
}
</script>

<style>
.scroll-card::-webkit-scrollbar {
  width: 16px;
}

.scroll-card::-webkit-scrollbar-track {
  background-color: #e4e4e4;
  border-radius: 100px;
}

.scroll-card::-webkit-scrollbar-thumb {
  background-color: #d4aa70;
  border-radius: 100px;
}
</style>
