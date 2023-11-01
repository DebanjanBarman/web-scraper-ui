<template>

  <v-card theme="light" class="pa-8 d-flex justify-center flex-wrap" variant="flat">
    <v-responsive>
      <v-banner-text
        class="pa-16 d-flex justify-center flex-wrap"
        style="font-size: 2rem; font-weight: bold"
      >Python Image Scraper


      </v-banner-text>

      <v-form
        theme="light"
        variant="outlined"
        @submit.prevent="onClick">
        <v-text-field
          placeholder="Search for a plant"
          variant="outlined"
          v-model="name"
          prepend-icon="m"
          append-inner-icon="mdi-magnify"
          @click:append-inner="onClick"
          :loading="loading"
          style=" display: block;margin-left: auto;margin-right: auto;width: 60%;"
        ></v-text-field>
      </v-form>
    </v-responsive>
  </v-card>
</template>
<script setup>

import {ref} from "vue";
import axios from "axios";

let name = ref("")
let loading = ref(false)

let onClick = async () => {
  if (name.value === "") {
    return
  }
  loading.value = true

  let resp = await axios.post("http://127.0.0.1:5000/api/getimages", {
    "name": name.value
  })
  console.log(resp.data)
  if (resp.data === 'success') {
    loading.value = false
    alert("Images downloaded successfully")
  }
}

</script>
