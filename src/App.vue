<script>
import axios from "axios";

export default {
  data() {
    return {
      getDataApi: [],
      dataPokemon: {}
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
  <div>
    <h1>How is Pokemon?</h1>
  </div>
</template>

<style scoped>
h1 {
  font-family: "Press Start 2P", system-ui;
  font-weight: 400;
  font-style: normal;
}
</style>
