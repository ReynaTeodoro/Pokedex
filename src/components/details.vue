<template>
  <v-container class="pa-0">
    <div>
      <v-card v-if="missing" class="d-flex justify-start pa-1">
        <v-img
          :src="require(`@/assets/icons/squirtlecrying.gif${''}`)"
          class="rounded"
          contain
          max-height="200"
          max-width="200"
        ></v-img>
        <v-card-title class="text-capitalize d-inline-block text-truncate"
          >Unable to find that pokemon</v-card-title
        >
      </v-card>
    </div>
    <v-card v-if="show" class="pa-2">
      <v-btn @click="closeDetail()" icon color="grey" class="ma-1 mb-0">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <div class="d-flex flex-no-wrap justify-space-between">
        <div class="left">
          <v-card-title class="text-capitalize">{{ pokemon.name }}</v-card-title>

          <v-card-subtitle>#{{ formatNumber(pokemon.id) }}</v-card-subtitle>
          <div class="d-flex flex-no-wrap ml-4">
            <v-img
              v-for="(type, index) in pokemon.types"
              :key="'typ' + index"
              v-bind:src="require(`@/assets/types/${type.type.name}.png`)"
              max-height="30"
              max-width="30"
              class="mr-1"
            ></v-img>
          </div>
          <v-btn @click="specieDialog = true" small class="ma-2 mt-3">description</v-btn>
          <v-dialog v-model="specieDialog" width="500" absolute>
            <specie :speUrl="pokemon.species.url" :name="pokemon.name" />
          </v-dialog>

          <div class="d-flex flex-no-wrap justify-space-between text-capitalize">
            <div>
              <v-card-text>weight:</v-card-text>
            </div>
            <v-spacer></v-spacer>
            <div>
              <v-card-text>{{ pokemon.weight / 10 }} kg</v-card-text>
            </div>
          </div>
          <v-divider></v-divider>
          <div class="d-flex flex-no-wrap justify-space-between">
            <div>
              <v-card-text>Height:</v-card-text>
            </div>
            <div>
              <v-card-text>{{ pokemon.height / 10 }} m</v-card-text>
            </div>
          </div>
          <v-divider></v-divider>
          <div class="d-flex flex-no-wrap justify-space-between text-capitalize">
            <div>
              <v-card-text>Base experience:</v-card-text>
            </div>
            <div>
              <v-card-text>{{ pokemon.base_experience }}</v-card-text>
            </div>
          </div>
          <v-divider></v-divider>
          <v-card-text class="text-capitalize">Abilities:</v-card-text>
          <div class="text-center">
            <div class="d-flex flex-no-wrap justify-space-around">
              <v-btn
                xSmall
                @click="getabilityUrl(ability.ability.url)"
                v-for="(ability, index) in pokemon.abilities"
                :key="'abili' + index"
                >{{ ability.ability.name.replace(/-/g, ' ') }}
              </v-btn>

              <v-dialog v-model="abilityDialog" width="500">
                <ability :abilityUrl="this.abilityUrl" />
              </v-dialog>
            </div>
          </div>
          <v-card-text>Moves:</v-card-text>
          <moves :moves="this.pokemon.moves" />
        </div>
        <div id="right">
          <v-img
            :src="`${imageUrl}${showShiny ? 'shiny/' : ''}${pokemon.id}.png`"
            class="rounded-circle grey ma-2"
            :class="[isDarkTheme ? 'darken-4' : 'lighten-4']"
            @click="toggleShowShiny"
          ></v-img>
          <div class="d-flex justify-center">
            <div v-for="(type, index) in pokemon.types" :key="'typ' + index">
              <v-img
                v-bind:src="require(`@/assets/pixtypes/${type.type.name}.png`)"
                contain
                max-height="50"
                max-width="70"
                class="ma-1"
              >
              </v-img>
            </div>
          </div>
          <v-card-title class="text-left">Stats:</v-card-title>

          <div id="basestats" v-for="(cate, index) in this.statCate" :key="'stat' + index">
            <div class="d-flex flex-no-wrap justify-space-between">
              <div>
                <v-card-text>{{ cate }}</v-card-text>
              </div>
              <div>
                <v-card-text>{{ series[0].data[index] }}</v-card-text>
              </div>
            </div>
            <v-divider></v-divider>
          </div>
          <div id="chart" style="color: black">
            <apexchart type="radar" width="300" :options="options" :series="series"></apexchart>
          </div>
        </div>
      </div>
    </v-card>
    <v-row justify="center" v-if="!show && !missing">
      <v-img
        :src="require(`@/assets/icons/sparkz.gif${''}`)"
        max-height="200"
        max-width="200"
        contain
      >
      </v-img>
    </v-row>
    <v-row v-if="!show && !missing">
      <v-progress-linear indeterminate color="cyan"></v-progress-linear>
    </v-row>
  </v-container>
</template>

<script>
import ability from '@/components/ability.vue';
import moves from '@/components/moves.vue';
import specie from '@/components/species.vue';

export default {
  props: ['pokemonUrl', 'imageUrl'],
  components: {
    ability,
    specie,
    moves,
  },
  data: () => ({
    show: false,
    hover: false,
    abilityDialog: false,
    specieDialog: false,
    missing: false,
    pokemon: {},
    abilityUrl: {},
    abilityIndex: 0,
    speUrl: {},
    series: [],
    statCate: ['Hp', 'Atk', 'Def', 'Sp Atk', 'Sp Def', 'Speed'],
    options: {
      chart: {
        width: 100,
        height: 500,
        type: 'radar',
      },
      markers: {},
      dataLabels: {
        style: {
          colors: ['#F44336', '#E91E63', '#9C27B0'],
        },
      },
      title: {
        text: 'Stat Graph:',
      },
      xaxis: {
        categories: ['Hp', 'Atk', 'Def', 'Sp Atk', 'Sp Def', 'Speed'],
      },
      yaxis: {
        tickAmount: 3,
        show: false,
        max: 150,
      },
    },
    showShiny: false,
  }),
  computed: {
    isDarkTheme() {
      return this.$vuetify.theme.dark;
    },
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch(this.pokemonUrl, {
          headers: {
            accept: 'application/json',
          },
        });
        if (response.ok) {
          const data = await response.json();
          this.pokemon = data;
          this.show = true;
          this.series.push({
            name: this.pokemon.name.toUpperCase(),
            data: this.pokemon.stats.map((x) => x.base_stat),
          });
        } else {
          const data = await response.json();
          this.error(data.message);
          this.missing = true;
        }
      } catch (error) {
        this.missing = true;
        this.error(error.message);
      }
    },
    closeDetail() {
      this.series = [];
      this.$emit('closeDetail');
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
    getabilityUrl(url) {
      this.abilityUrl = url;
      this.abilityDialog = true;
    },
    toggleShowShiny() {
      this.showShiny = !this.showShiny;
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style scoped>
.left {
  width: 100%;
}
.pa-0 {
  overflow-x: hidden;
}
</style>
