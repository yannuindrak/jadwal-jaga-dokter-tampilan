<template>
  <v-app id="app">
    <NavigationsHeader2 />
    <DashboardKonten />
    <!-- <NavigationsFooter /> -->
  </v-app>

  <slot />
</template>
<style>
#app {
  background: linear-gradient(215deg, #ffffff 0%, #dedcdc 99%);
}
</style>
