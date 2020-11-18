<template>
  <v-app
    style="
    background: #121212;
    background: -webkit-radial-gradient( #0D47A1, #0A2E50);
    background: radial-gradient(#0D47A1, #0A2E50);"
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
