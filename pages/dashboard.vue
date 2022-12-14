<template>
  <v-row justify="center" class="category">
    <v-col cols="12" sm="8" md="11">
      <div class="primary--text display-1 font-weight-normal">
        Update Patient Medical Record
      </div>
      <div class="grey--text caption mt-4 mb-5">
        Click the tabs to view and edit patient medical details
      </div>
      <v-card class="white main" flat>
        <v-card-title class="headline black--text py-4 px-5">
          <div
            class="category"
            v-for="n in 2"
            :key="n.id"
            v-show="!data && skeleton === true"
          >
            <v-skeleton-loader
              v-model="skeleton"
              style="width: 50%"
              type="list-item"
            ></v-skeleton-loader>

            <div class="checkbox-containers">
              <v-skeleton-loader
                v-model="skeleton"
                v-for="n in 9"
                :key="n.id"
                type="list-item"
              ></v-skeleton-loader>
            </div>
            <v-divider class="my-4"></v-divider>
          </div>

          <div class="category" v-for="item in data" :key="item.id">
            <div class="primary--text">{{ item.title }}</div>

            <div class="checkbox-containers">
              <v-checkbox
                v-show="data"
                v-for="investigation in item.investigations"
                :key="investigation.id"
                v-model="selected"
                :label="investigation.title"
                :value="investigation.id"
              ></v-checkbox>
            </div>

            <v-divider class="my-4"></v-divider>
          </div>
        </v-card-title>
        <v-form class="grey--text">
          <v-container>
            <v-row align="center">
              <v-col class="" cols="6">
                <div class="my-2">CT Scan</div>
                <v-select
                  :items="ctscan"
                  v-model="selectedCtscan"
                  placeholder="*Specify"
                  outlined
                ></v-select>
              </v-col>
              <v-col class="" cols="6">
                <div class="my-2">MRI</div>
                <v-select
                  v-model="selectedMRI"
                  :items="mri"
                  placeholder="*Specify"
                  outlined
                ></v-select>
              </v-col>
            </v-row>
          </v-container>
        </v-form>
        <v-row class="px-5">
          <v-spacer />
          <v-btn
            depressed
            color="primary"
            @click="updateData"
            class="text-capitalize"
          >
            Save and Send
          </v-btn>
        </v-row>
      </v-card>
    </v-col>
    <v-snackbar color="green" v-model="snackbar" :timeout="5000" class="py-0">
      <div class="font-weight-bold subtitle-2">Record saved successfully</div>
      <template v-slot:action="{ attrs }">
        <v-btn
          rounded
          color="white"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>
  </v-row>
</template>

<script>
import axios from 'axios'
export default {
  name: 'DashboardPage',
  data() {
    return {
      snackbar: false,
      skeleton: true,
      selected: [],
      data: '',
      ctscan: [
        'Scan needed in the left cerebral hemisphere',
        'Scan needed in the right cerebral hemisphere',
        'Scan needed in the left hip bone',
        'Scan needed in the right hip bone',
        'Scan needed in the lower abdomen',
      ],
      mri: [
        'Full body MRI',
        'Full skull MRI',
        'Full spinal MRI',
        'Full arm MRI',
      ],
      selectedCtscan: '',
      selectedMRI: '',
      api: 'https://testdrive.kompletecare.com/api/investigations',
    }
  },
  methods: {
    getData() {
      axios({
        method: 'GET',
        url: `${this.api}`,
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('token'),
          Accept: 'application/json',
        },
      }).then((response) => {
        this.data = response.data.data
        console.log(response)
      })
    },
    updateData() {
      let formData = new FormData()
      for (let i = 0; i < this.selected.length; i++) {
        formData.append(`investigations[${i}]`, this.selected[i])
      }
      formData.append('ctscan', `${this.selectedCtscan} `)
      formData.append('mri', `${this.selectedMRI}`)
      formData.append('developer', 'Developer')

      axios({
        method: 'POST',
        url: `${this.api}`,
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('token'),
          Accept: 'application/json',
        },
        data: formData,
      }).then((response) => {
        console.log(response)
        this.snackbar = true
      })
    },
  },
  mounted() {
    this.getData()
  },
}
</script>

<style>
.main {
  padding: 2% 5% 5%;
}
.category {
  width: 100%;
}
.checkbox-containers {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
</style>
