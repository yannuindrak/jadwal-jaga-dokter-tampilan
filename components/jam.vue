<template>
  {{ now }}
</template>

<script setup>
import { useNow, useDateFormat } from "@vueuse/core";
const now = useDateFormat(useNow(), "dddd, DD MMMM YYYY HH:mm:ss", {
  locales: "id-ID",
});
</script>
