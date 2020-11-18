<template >
  <v-container class="pa-0" v-click-outside="close">
    <v-row justify="center" v-if="show">
      <v-card dark v-if="randomDescription" class="pa-2 pt-0">
        <v-card-title class="text-capitalize indigo darken-4" dark>
          {{ name.replace(/-/g, ' ') }}
        </v-card-title>
        <v-card-subtitle class="indigo darken-4">
          {{ specie.genera[7].genus }}
        </v-card-subtitle>
        <v-card-text class="mt-2">
          {{ randomDescription }}
        </v-card-text>
      </v-card>
    </v-row>
    <v-skeleton-loader
      contain
      v-bind="attrs"
      type="card-heading,list-item-three-line"
      v-else
      dark
    ></v-skeleton-loader>
  </v-container>
</template>

<script>
export default {
  props: ['speUrl', 'name'],
  data: () => ({
    attrs: {
      class: 'mb-1',
      boilerplate: false,
      elevation: 2,
    },
    show: false,
    specie: {},
  }),
  computed: {
    randomDescription() {
      if (!this.specie.flavor_text_entries) return '';
      const entries = this.specie.flavor_text_entries.filter((x) => x.language.name === 'en');
      return entries[Math.floor(Math.random() * entries.length)].flavor_text;
    },
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch(this.speUrl, {
          headers: {
            accept: 'application/json',
          },
        });
        if (response.ok) {
          const data = await response.json();
          this.specie = data;
          this.show = true;
        } else {
          const data = await response.json();
          this.error(data.message);
        }
      } catch (error) {
        this.error(error.message);
      }
    },
    close() {
      this.fetchData();
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style scoped>
.pa-0 {
  overflow-x: hidden;
}
</style>
