<template>
  <div class="container">
    <div class="pokemon-card">
      <h3 v-show="isPokemonNameDiscovered">{{ pokemon.name }}</h3>
      <img :src="pokemon.img" :class="{ 'no-blur': isPokemonDiscovered }" alt="">
      <input :class="{ 'no-show': isPokemonDiscovered }" v-model="pokemonName" type="text">
      <button :class="{ 'no-show': isPokemonDiscovered }"  @click="showPokemones">Descubrir</button>
    </div>
  </div>
</template>

<script>
export default {
  computed() {
    return {
      pokemonName: "",
      isPokemonDiscovered: false,
      isPokemonNameDiscovered: false,
    };
  },
  props: {
    pokemon: {
      type: Object,
      required: true
    }
  },
  methods: {
    showPokemones() {
      if (this.pokemonName === this.pokemon.name) {
        this.isPokemonNameDiscovered = true;
        this.isPokemonDiscovered = true;
        this.$emit('pokemon-discovered'); 
      }
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  margin: 0.5rem;
  padding: 0.5rem;
}

.pokemon-card {
  display: flex;
  justify-content: center;
  flex-direction: column;
  margin: 0.5rem;
  padding: 0.5rem;
  background-color: white;
  color: rgb(53, 53, 53);
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  width: 250px;
  height: 350px;
}

img {
  margin: 1rem;
  padding: 1rem;
  width: 150px;
  height: 150px;
  filter: blur(10px) grayscale(100%);
}

img.no-blur {
  filter: none;
}




span {
  color: black;
}

input {
  padding: 0.3rem;
  margin: 0.3rem;
}

input.no-show{
  display: none;
}

button {
  padding: 0.3rem;
  margin: 0.3rem;
  background-color: #085898;
  color: white;
  border-radius: 10px;
}

button.no-show{
  display: none;
}
</style>
