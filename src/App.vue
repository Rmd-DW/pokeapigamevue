<template>
  <div class="cont-app">
    <img src="/logo.png" alt="Pokemon Logo" width="250px">
    <h1>¿Quién es ese Pokémon?</h1>
    <img class="iconPoke" src="/pokeball_gray.png" alt="" width="50px">
    <h2>Total descubiertos: {{ discoveredCount }}</h2>
    <div v-if="pokemones.length > 0">
      <div class="grilla-pokemon">
        <pokemonCard 
          :pokemon="pokemon" 
          v-for="pokemon in pokemones" 
          :key="pokemon.name" 
          @pokemon-discovered="incrementDiscoveredCount"
        />
      </div>
    </div>
    <div v-else>
      <p v-if="error">Error al cargar los Pokémon. Por favor, inténtalo de nuevo más tarde.</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import pokemonCard from '@/components/pokemonCard.vue';

export default {
  components: {
    pokemonCard,
  },
  data(){
    return {
      urlBase: 'https://pokeapi.co/api/v2/pokemon',
      pokemones: [],
      error: null,
      discoveredCount: 0,
    };
  },
  methods: {
    async getPokemones() {
      try {
        const { data } = await axios.get(`${this.urlBase}?limit=20`);
        const listaPokemones = data.results;

        const promises = listaPokemones.map(async (pokemon, index) => {
          const { data } = await axios.get(`${this.urlBase}/${index + 1}/`);
          const { name, sprites: { other: { dream_world: { front_default } } } } = data;

          return {
            name,
            img: front_default,
          };
        });

        this.pokemones = await Promise.all(promises);
      } catch (error) {
        this.error = error.message || "Error al cargar los Pokémon";
      }
    },
    incrementDiscoveredCount() {
      this.discoveredCount += 1;
    },
  },
  mounted() {
    this.getPokemones();
  }
};
</script>

<style>
body {
  display: flex;
  justify-content: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-image: url("https://assets.pokemon.com/static2/_ui/img/chrome/body_bg.png");
  color: white;
}

h1 {
  font-size: 61px;
  color: white;
}

.cont-app {
  text-align: center;
  margin: 1rem;
  padding: 1rem;
}



.grilla-pokemon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: row;
  flex-wrap: wrap;
}

.iconPoke {
  filter: brightness(100%);
}
</style>
