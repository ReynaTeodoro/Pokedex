<template>
  <v-app-bar @submit.prevent="setpokemonUrl" color="red darken-1" class="pa-1" app>
    <v-container>
      <v-row no-gutters style="flex-wrap: nowrap">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          :src="require(`@/assets/icons/ultra.png${''}`)"
          transition="scale-transition"
          width="40"
        />
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          :src="require(`@/assets/icons/Pokédex_logo.png${''}`)"
          width="100"
        />
        <v-col cols="1" style="min-width: 100px; max-width: 100%" class="flex-grow-1 flex-shrink-0">
          <div class="d-flex align-center flex-grow-1">
            <v-autocomplete
              @change="setpokemonUrl"
              color="red lighten-2"
              :items="this.items"
              v-model="searchValue"
              class="d-flex ml-3 mt-6 text-capitalize"
              clearable
              outlined
              dense
              placeholder="Search 🔍"
            ></v-autocomplete>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <div class="shrink d-flex align-center mt-6">
      <v-switch v-model="switch1" color="black" inset></v-switch>
      <v-img class="contain mb-6" :src="require(`@/assets/icons/dark/${switch1}.png`)"></v-img>
    </div>
  </v-app-bar>
</template>
<script>
export default {
  props: ['apiUrl'],
  data: () => ({
    searchValue: null,
    items: [],
  }),
  computed: {
    switch1: {
      get() {
        return this.$vuetify.theme.dark;
      },
      set() {
        this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
      },
    },
  },
  methods: {
    setpokemonUrl() {
      if (this.searchValue) {
        this.$emit('setpokemonUrl', this.apiUrl + this.searchValue.trim().toLowerCase());
        this.searchValue = null;
      }
    },
    async fetchData() {
      try {
        const response = await fetch('https://pokeapi.co/api/v2/pokemon/?limit=893', {
          headers: {
            accept: 'application/json',
          },
        });
        if (response.ok) {
          const data = await response.json();
          this.items = data.results.map((x) => x.name.charAt(0).toUpperCase() + x.name.slice(1));
        } else {
          const data = await response.json();
          this.error(data.message);
        }
      } catch (error) {
        this.error(error.message);
      }
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style>
/* .v-list-item__title {
  text-transform: capitalize;
} */
</style>
