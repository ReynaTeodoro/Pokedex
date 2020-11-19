<template>
  <v-app
    style="
      background: #121212;
      background: -webkit-radial-gradient(#0d47a1, #0a2e50);
      background: radial-gradient(#0d47a1, #0a2e50);
    "
  >
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
.v-image__image {
  transition: all 0.3s ease-out;
}
</style>
