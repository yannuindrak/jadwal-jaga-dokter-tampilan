<style scoped>
@import url("~/assets/css/header.css");
</style>

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
        <div class="column">
          <h1 id="jam">{{ now }}</h1>
          <h2 id="tanggal">
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
