<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="12">
      <v-card class="white main" flat>
        <div class="primary--text text-center display-1 my-5 font-weight-bold">
          WELCOME TO SEVENZ HEALTHCARE
        </div>

        <v-row class="px-5">
          <v-spacer />
          <v-btn
            depressed
            @click="login"
            large
            color="primary"
            class="my-4 font-weight-bold"
          >
            Login
          </v-btn>
          <v-spacer />
        </v-row>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from 'axios'
export default {
  name: 'IndexPage',
  data() {
    return {
      api: 'https://testdrive.kompletecare.com/api/login',
    }
  },
  methods: {
    login() {
      let formData = new FormData()
      formData.append('email', 'tester@kompletecare.com')
      formData.append('password', 'password')
      axios({
        method: 'POST',
        url: `${this.api}`,
        data: formData,
        headers: {
          'X-Requested-With': 'XMLHttpRequest',
        },
      }).then((response) => {
        localStorage.setItem('token', response.data.data.token)
        this.$router.push('/dashboard')
        console.log(response)
      })
    },
  },
}
</script>

<style>
.main {
  padding: 2% 5% 5%;
}
</style>
