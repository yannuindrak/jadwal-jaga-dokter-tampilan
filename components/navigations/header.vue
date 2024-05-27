<template>
  <header>
    <div>
      <v-img
        :width="150"
        aspect-ratio="16/9"
        cover
        src="/assets/img/1.png"
        id="gambar1"
      ></v-img>
      <v-img :width="150" src="/assets/img/2.png" id="gambar2"></v-img>
      <div align="center" mb="50" id="text1">
        <h1 class="text-lg font-bold text-white">JADWAL JAGA DOKTER IGD</h1>
      </div>
      <div class="d-flex justify-center">
        <h4 class="text-h6 text-md-h5 text-lg-h4 font-bold text-white">
          {{ now }}
        </h4>
      </div>
    </div>
  </header>
</template>
<script setup lang="ts">
import { useNow, useDateFormat } from "@vueuse/core";

const now = useDateFormat(useNow(), "dddd, DD MMMM YYYY HH:mm:ss", {
  locales: "id-ID",
});
</script>
<style scoped>
#gambar1 {
  width: 150px;
  height: 100px;
  object-fit: none;
  object-position: left top;
  float: left;
  top: 15px;
  right: 0px;
  left: 50px;
}
#gambar2 {
  width: 150px;
  height: 100px;
  top: 15px;
  float: right;
}
</style>
