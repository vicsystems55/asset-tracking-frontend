<template>
  <!-- Error page-->
  <div class="container py-2">
    <b-link class="brand-logo d-flex justify-content-center">
      <img class="text-center" style="max-height: 50px;" src="@/assets/images/logo/logo.png" alt=""> <br>

      <h5 class="brand-text text-primary ml-1 mb-5">
        Federal Ministry of <br> <span class="">Finance, Budget and National Planning</span>
      </h5>
    </b-link>

    <div class="misc-inne p-2 p-sm-3">
      <div class="w-100 text-cente">
        <h3 class="mb-1 text-center">
          Contractor Application Form
        </h3>



        <div style="max-width: 400px;" class="col-md mx-auto">
          <div class="form-group">
            <label for="contractorName">Contractor Name:</label>
            <input type="text" class="form-control" placeholder="Enter Contractor Name:" v-model="contractor_name">
          </div>
          <div class="form-group">
            <label for="contractorAddress">Contractor Address:</label>
            <input type="text" class="form-control" placeholder="Enter Contractor Address:" v-model="contractor_address">
          </div>
          <div class="form-group">
            <label for="rcNumber">RC Number:</label>
            <input type="text" class="form-control" placeholder="Enter Contractor RC Number:" v-model="rc_number">
          </div>
          <div class="form-group">
            <label for="phone">Phone:</label>
            <input type="text" class="form-control" placeholder="Enter Contractor Phone:" v-model="phone">
          </div>
          <div class="form-group">
            <label for="email">Email:</label>
            <input type="text" class="form-control" placeholder="Enter Contractor Email:" v-model="email">
          </div>
          <div class="form-group">
            <label for="brief">Brief:</label>
            <textarea name="brief" id="brief" cols="30" rows="10" placeholder="Enter brief of submission"
              class="form-control" v-model="brief"></textarea>
          </div>

          <b-button variant="primary" class="mb-2 mt-2 btn-sm-block btn-block" @click="submitContractorApplication">
            Submit
          </b-button>
        </div>





        <!-- image -->
        <!-- <b-img
          fluid
          :src="imgUrl"
          alt="Error page"
        /> -->
      </div>
    </div>
  </div>
  <!-- / Error page-->
</template>

<script>
/* eslint-disable global-require */
import { BLink, BButton, BImg } from 'bootstrap-vue'
import VuexyLogo from '@core/layouts/components/Logo.vue'
import store from '@/store/index'
import axios from 'axios'
import ToastificationContent from '@core/components/toastification/ToastificationContent.vue'


export default {
  components: {
    VuexyLogo,
    BLink,
    BButton,
    BImg,
  },
  data() {
    return {
      downImg: require('@/assets/images/pages/error.svg'),

      contractor_name: '',
      contractor_address: '',
      rc_number: '',
      phone: '',
      email: '',
      brief: '',

      loadingy: false
    }
  },
  computed: {
    imgUrl() {
      if (store.state.appConfig.layout.skin === 'dark') {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.downImg = require('@/assets/images/pages/error-dark.svg')
        return this.downImg
      }
      return this.downImg
    },
  },
  methods: {

    submitContractorApplication() {

      this.loadingy = true
      axios({
        url: `${process.env.VUE_APP_BACKEND_URL}/api/contractor-applications`,
        method: 'post',
        data: {
          contractor_name: this.contractor_name,
          contractor_address: this.contractor_address,
          rc_number: this.rc_number,
          phone: this.phone,
          email: this.email,
          brief: this.brief,
        },
      }).then(res => {
        this.loadingy = false
        console.log(res)

        this.$toast({
          component: ToastificationContent,
          props: {
            title: 'Application Submitted',
            icon: 'EditIcon',
            variant: 'success',
          },
        })

        this.$router.push('/submission-success')
      }).catch(error => {
        this.loadingy = false

        this.$toast({
          component: ToastificationContent,
          props: {
            title: error.response.data.errors.email.toString(),
            icon: 'EditIcon',
            variant: 'danger',
          },
        })

        console.log(error)
      })

    }



  },
}
</script>

<style lang="scss">
@import '@core/scss/vue/pages/page-misc.scss';
</style>
