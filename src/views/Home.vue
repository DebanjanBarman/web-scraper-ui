<template>
  <v-card theme="light" class="pa-8 d-flex justify-center flex-wrap" variant="flat">
    <v-responsive>
      <v-banner-text
        class="pa-16 d-flex justify-center flex-wrap"
        style="font-size: 2rem; font-weight: bold"
      >Python Image Scraper
      </v-banner-text>

      <v-form @submit.prevent="onClick">
        <v-text-field
          placeholder="Search for a plant"
          variant="outlined"
          v-model="query"
          prepend-icon="m"
          append-inner-icon="mdi-magnify"
          @click:append-inner="onClick"
          :loading="loading"
          style=" display: block;margin-left: auto;margin-right: auto;width: 60%;"
        ></v-text-field>
        <v-row style="display: flex;width: 62%; margin: auto">
          <v-col>
            <v-select
              label="License Type"
              density="compact"
              variant="outlined"
              v-model="license"
              :items="['All', 'Creative Commons','Commercial']"
            ></v-select>
          </v-col>
          <v-col>
            <v-text-field
              v-model="number_of_images"
              density="compact"
              variant="outlined"
              label="No of Images"
              type="number"
            ></v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              v-model="image_height"
              label="Image Height"
              density="compact"
              variant="outlined"
              type="number"
            ></v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              v-model="image_width"
              type="number"
              label="Image Width"
              density="compact"
              variant="outlined"
            ></v-text-field>
          </v-col>
        </v-row>
      </v-form>
      <v-row>
        <v-col
          v-for="(image, i) in images"
          :key="i"
        >
          <img :src="image" :alt="query">
        </v-col>
      </v-row>
    </v-responsive>
  </v-card>
</template>
<script setup>

import {ref} from "vue";
import axios from "axios";

let loading = ref(false)
let query = ref("")
let license_type
let license = ref("All")
let number_of_images = ref(10)
let image_height = ref(400)
let image_width = ref(600)
let images = ref([])


let onClick = async () => {
  if (query.value === "") {
    return
  }
  loading.value = true
  if (license.value === "All") {
    license_type = 3
  } else if (license.value === "Creative Commons") {
    license_type = 1
  } else if (license.value === "Commercial") {
    license_type = 2
  }
  axios.defaults.headers.post['Access-Control-Allow-Origin'] = '*';
  let resp = await axios.request({
    method: "POST",
    url: "http://127.0.0.1:5000/api/getimages",
    data: {
      "query": query.value,
      "license_type": license_type,
      "number_of_images": Number(number_of_images.value),
      "image_height": Number(image_height.value),
      "image_width": Number(image_width.value),
    }
  })
  console.log(resp)
  loading.value = false
  if (resp.status === 200) {
    loading.value = false
    resp.data.forEach(el => {
      images.value.push(el[0])
    })
    // alert("Images downloaded successfully")
  }
  images.value.forEach(image => {
    console.log(image)
  })
}
</script>
