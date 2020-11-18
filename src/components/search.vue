<template>
  <v-app-bar @submit.prevent="setpokemonUrl" color="red darken-1" dark class="pa-1" app>
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
          <v-form>
            <div class="d-flex align-center flex-grow-1">
              <v-text-field
                color="red lighten-1"
                v-model="searchvalue"
                class="d-flex ml-3 mt-6"
                clearable
                outlined
                dense
              ></v-text-field>
              <v-btn
                class="mx-2"
                fab
                dark
                small
                @click="setpokemonUrl"
                type="submit"
                color="red lighten-1"
              >
                <v-icon> mdi-magnify </v-icon>
              </v-btn>
            </div>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
    <div class="d-flex align-center"></div>
  </v-app-bar>
</template>

<script>
export default {
  props: ['apiUrl'],

  data: () => ({
    searchvalue: '',
  }),
  methods: {
    setpokemonUrl() {
      if (this.searchvalue !== '') {
        this.$emit('setpokemonUrl', this.apiUrl + this.searchvalue.trim().toLowerCase());
      }
    },
  },
};
</script>
