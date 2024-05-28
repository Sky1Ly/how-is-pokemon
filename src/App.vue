<script>
import axios from "axios";

export default {
  data() {
    return {
      getDataApi: [],
      dataPokemon: {},
      nombrePokemon: {}
    }
  },

  methods: {
    async consultaApiRandom() {
      try {
        const datoBusqueda = Math.floor(Math.random() * 1000)
        const url = `https://pokeapi.co/api/v2/pokemon?offset=${datoBusqueda}&limit=20`

        const response = await axios.get(url)
        this.getDataApi = response.data

        const promesa = this.getDataApi.results.map(async (pokemon) => {
          const { data } = await axios.get(pokemon.url)

          return {
            name: pokemon.name,
            image: data.sprites?.front_default,
            gif: data.sprites?.other?.showdown?.front_default
          }
        })

        this.dataPokemon = await Promise.all(promesa)

        console.log(this.dataPokemon);

      } catch (error) {
        console.error(`Ha ocurrido un error: ${error}`);
      }
    }
  },

  mounted() {
    this.consultaApiRandom()
  },
}

//API para obtener Pokemons randoms: https://pokeapi.co/api/v2/pokemon?offset=980&limit=20
</script>

<template>
  <div class="container">
    <div class="text-center mt-5 mb-5">
      <img src="./assets/pokemon-logo.png" alt="Pokemon logo" style="max-width: 350px;" />
      <h1>How is Pokemon?</h1>
    </div>

    <div class="row">
      <div v-for="(data, i) in dataPokemon" :key="i" class="col-sm-3 mx-auto mb-5">
        <img :src="data.image" style="filter: blur(2px) grayscale(100%);" />
        <br/>
        <input type="text" name="inputNombre" v-model="nombrePokemon">
        <p>{{ data.name }}</p>
        <button type="button" class="btn btn-outline-success">Descubrir</button>        
        <p>{{ nombrePokemon }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-family: "Press Start 2P", system-ui;
  font-weight: 400;
  font-style: normal;
}
</style>
