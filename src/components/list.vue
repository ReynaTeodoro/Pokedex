<template>
  <v-main>
    <h1 v-if="error">{{ error }}</h1>
    <v-container grid-list-md>
      <v-layout row wrap>
        <v-flex xs6 sm4 lg2 v-for="(pokemon, index) in limit" :key="'poke' + index">
          <v-card class="ma-1" elevation="2" outlined shaped>
              <v-img
              :src="`${imageUrl}` + (index + 1) + '.png'"
              contain
              max-height="400"
              max-width="400"
              class="rounded-circle grey ma-1"
              :class="[isDarkTheme ? 'darken-4' : 'lighten-4']"
              ></v-img>
            <v-divider></v-divider>
            <v-card-title class="d-flex justify-space-between mb-6 text-capitalize" display: block
            >
            {{
              pokemon.name.match(/^(mr|nidoran)/) ? pokemon.name :
              pokemon.name.replace('-', ' ').split(" ")[0] }}
            </v-card-title>
            <v-card-actions class="text-center">
              <v-btn color="info" text @click="setpokemonUrl(pokemon.url)">Ver</v-btn>
              <v-spacer></v-spacer>
              #{{  formatNumber(index + 1) }}
            </v-card-actions>
          </v-card>
        </v-flex>
        <div id="scrolltrigger" ref="infinitescrolltrigger" class="ma-auto ">
          <v-progress-circular
            indeterminate
            color="blue-grey"
            :width="7"
            :size="100"
          ></v-progress-circular>
        </div>
      </v-layout>
    </v-container>
  </v-main>
</template>

<script>
export default {
  name: 'List',
  props: ['imageUrl', 'apiUrl'],
  computed: {
    limit() {
      return this.pokemons.filter((x, index) => index <= 892);
    },
    isDarkTheme() {
      return this.$vuetify.theme.dark;
    },
  },
  data: () => ({
    pokemons: [],
    nextUrl: '',
    error: null,
    currentUrl: '',
  }),
  methods: {
    async fetchData() {
      try {
        const response = await fetch(this.currentUrl, {
          headers: {
            accept: 'application/json',
          },
        });
        if (response.ok) {
          const data = await response.json();
          this.pokemons = this.pokemons.concat(data.results);
          this.nextUrl = data.next;
        } else {
          const data = await response.json();
          this.error(data.message);
        }
      } catch (error) {
        this.error(error.message);
      }
    },
    formatNumber(num) {
      if (num < 10) {
        return `00${num}`;
      }
      if (num < 100 && num >= 10) {
        return `0${num}`;
      }
      return num;
    },
    scrolltrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });
      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setpokemonUrl(url) {
      this.$emit('setpokemonUrl', url);
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrolltrigger();
  },
};
</script>
