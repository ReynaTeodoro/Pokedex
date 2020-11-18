<template>
  <div class="home">
    <Search :apiUrl="apiUrl" @setpokemonUrl="setpokemonUrl" />
    <v-dialog v-model="showDetail" width="600" hide-overlay class="pa-0">
      <Details
        v-if="showDetail"
        :pokemonUrl="pokemonUrl"
        :imageUrl="imageUrl"
        @closeDetail="closeDetail"
      />
    </v-dialog>
    <v-dialog v-model="showDetailXS" width="600" hide-overlay class="pa-0">
      <DetailsXS
        v-if="showDetailXS"
        :pokemonUrl="pokemonUrl"
        :imageUrl="imageUrl"
        @closeDetail="closeDetail"
      />
    </v-dialog>
    <List :imageUrl="imageUrl" :apiUrl="apiUrl" @setpokemonUrl="setpokemonUrl" />
  </div>
</template>

<script>
import List from '@/components/list.vue';
import Search from '@/components/search.vue';
import Details from '@/components/details.vue';
import DetailsXS from '@/components/detailxs.vue';

export default {
  name: 'App',

  components: {
    List,
    Search,
    Details,
    DetailsXS,
  },

  data: () => ({
    // imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
    // imageUrl: 'http://localhost:1337/pokemon/',
    // apiUrl: 'http://localhost/api/v2/pokemon/',
    imageUrl: 'http://192.168.0.27:1337/pokemon/',
    apiUrl: 'http://192.168.0.27/api/v2/pokemon/',
    pokemonUrl: '',
    showDetail: false,
    showDetailXS: false,
  }),
  computed: {
    size() {
      return this.$vuetify.breakpoint.width;
    },
  },
  methods: {
    setpokemonUrl(url) {
      if (this.size >= 600) {
        this.showDetail = true;
      } else {
        this.showDetailXS = true;
      }
      this.pokemonUrl = url;
    },
    closeDetail() {
      this.pokemonUrl = '';
      this.showDetail = false;
      this.showDetailXS = false;
    },
  },
};
</script>
