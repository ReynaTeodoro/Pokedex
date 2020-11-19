<template >
  <v-container class="pa-0" v-click-outside="close">
    <v-card v-if="ability" elevation="2">
      <v-card-title class="text-capitalize cyan ma-0">
        {{ ability.name.replace(/-/g, ' ') }}
      </v-card-title>
      <v-card-subtitle class="cyan">
        {{ ability.flavor_text_entries[0].flavor_text }}
      </v-card-subtitle>
      <v-card-text class="mt-2">
        {{ ability.effect_entries[1].effect }}
      </v-card-text>
    </v-card>

    <v-skeleton-loader
      contain
      v-bind="attrs"
      type="card-heading,list-item-three-line"
      v-else
    ></v-skeleton-loader>
  </v-container>
</template>

<script>
export default {
  props: ['abilityUrl'],
  data: () => ({
    ability: null,
    attrs: {
      class: 'mb-1',
      boilerplate: false,
      elevation: 2,
    },
  }),
  methods: {
    async fetchData() {
      try {
        const response = await fetch(this.abilityUrl, {
          headers: {
            accept: 'application/json',
          },
        });
        if (response.ok) {
          const data = await response.json();
          this.ability = data;
        } else {
          const data = await response.json();
          this.error(data.message);
        }
      } catch (error) {
        this.error(error.message);
      }
    },
    close() {
      this.ability = null;
    },
  },
  created() {
    this.fetchData();
  },
  beforeUpdate() {
    this.fetchData();
  },
};
</script>
