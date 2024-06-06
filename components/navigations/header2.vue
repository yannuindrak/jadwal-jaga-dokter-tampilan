<template>
  <header>
    <nav>
      <div class="row">
        <div class="column">
          <v-img src="/assets/img/2.png" id="gambar2"></v-img>
        </div>
        <div class="column">
          <h1 id="tulisanjaga">DAFTAR JAGA DOKTER IGD</h1>
        </div>
        <div class="column" id="jam">
          <h1>{{ now }}</h1>
          <h2>
            {{ date }}
          </h2>
        </div>
        <div class="column">
          <v-img src="/assets/img/5.png" id="gambar1"></v-img>
        </div>
      </div>
    </nav>
  </header>
</template>
<script setup lang="ts">
import { useNow, useDateFormat } from "@vueuse/core";

const now = useDateFormat(useNow(), "HH:mm", {
  locales: "id-ID",
});

const date = useDateFormat(useNow(), "DD MMMM YYYY", {
  locales: "id-ID",
});
</script>
<style scoped>
#jam {
  top: 15px;
}
#gambar1 {
  width: 150px;
  height: 100px;
  object-fit: none;
  object-position: right top;
  float: right;
  top: 15px;
  right: 5px;
}

#gambar2 {
  width: 150px;
  height: 100px;
  top: 10px;
  float: left;
  object-fit: none;
  object-position: left top;
  display: flex;
  left: 1px;
}

#tulisanjaga {
  -webkit-text-fill-color: white;

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

.row {
  display: flex;
  top: 10px;
  -webkit-text-fill-color: white;
  background-image: linear-gradient(90deg, #002031 0%, #0079bd 54%);
}

.column {
  flex: auto;
  height: 122px;
}
</style>
