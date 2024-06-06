<template>
  <!-- <header>
    <div>
      <v-img
        :width="150"
        aspect-ratio="16/9"
        cover
        src="/assets/img/5.png"
        id="gambar1"
      ></v-img>
      <v-img :width="150" src="/assets/img/2.png" id="gambar2"></v-img>
      <div align="center" mb="50" id="text1">
        <h1 id="tulisanjaga">DAFTAR JAGA DOKTER I G D</h1>
      </div>
      <div class="d-flex justify-center" id="waktu">
        <h2 class="text-lg font-bold text-white">
          {{ now }}
        </h2>
      </div>
    </div>
  </header> -->
  <header>
    <nav>
      <v-img :width="150" src="/assets/img/5.png" id="gambar2"></v-img>

      <div mb="50" id="text1">
        <h1 id="tulisanjaga">DAFTAR JAGA DOKTER IGD</h1>
      </div>
      <div>
        <h2 class="flex items-center">
          {{ now }}
        </h2>
      </div>
    </nav>
  </header>
</template>
<script setup lang="ts">
import { useNow, useDateFormat } from "@vueuse/core";

const now = useDateFormat(useNow(), "HH:mm", {
  locales: "id-ID",
});

const date = useDateFormat(useNow(), "dddd, DD MMMM YYYY", {
  locales: "id-ID",
});
</script>
<style scoped>
#gambar2 {
  width: 150px;
  height: 90px;
  top: 15px;
  float: left;
  object-fit: none;
  object-position: left top;
  display: flex;
}
#gambar1 {
  width: 180px;
  height: 120px;
  object-fit: none;
  object-position: right top;
  float: right;
  top: 15px;
  right: 10px;
}

#tulisanjaga {
  -webkit-text-fill-color: black;

  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif,
    "Lucida Sans Unicode", "Lucida Grande", "Lucida Sans", Arial, sans-serif;
  font-size: 70px;
  display: flex;
}
#waktu {
  -webkit-text-fill-color: black;
  -webkit-text-stroke-color: black;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif,
    "Lucida Sans Unicode", "Lucida Grande", "Lucida Sans", Arial, sans-serif;
  font-size: 28px;
  display: flex;
}

#garis {
  width: 30%;
}
.header {
  background: rgb(0, 111, 134);
}

.waktu {
  object-fit: none;
  object-position: right top;
  float: right;
}
</style>
dddd, DD MMMM YYYY
