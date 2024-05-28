<script>
import axios from "axios";

export default {
  data() {
    return {
      getDataApi: [],
      dataPokemon: {},
      nombrePokemon: [],
      count: 0
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

          //URL para saber nombres de pokemons en consola
        console.log(`Haz descubierto los nombres :o \n ${pokemon.name}`);

          return {
            name: pokemon.name,
            image: data.sprites?.front_default,
            gif: data.sprites?.other?.showdown?.front_default,
            porDescurbri: true
          }
        })

        this.dataPokemon = await Promise.all(promesa)

        // Inicializar el array de nombres
        this.nombrePokemon = new Array(this.dataPokemon.length).fill("")

      } catch (error) {
        console.error(`Ha ocurrido un error: ${error}`);
      }
    },

    descubrirPokemon(index) {
      if (this.nombrePokemon[index].toLowerCase() == this.dataPokemon[index].name.toLowerCase()) {
        // Actualizar la propiedad descubierto
        this.dataPokemon[index].porDescurbri = false
        this.count++
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
      <h1>What is Pokemon?</h1>
      <h3>discovered Pokémon: {{ count }}</h3>
    </div>

    <div class="row">
      <div v-for="(data, i) in dataPokemon" :key="i" class="col-sm-3 mx-auto mb-5">
        <img v-if="data.porDescurbri" :src="data.image" :class="{descubrir: data.porDescurbri}" />
        <img v-if="!data.porDescurbri" :src="data.gif" />
        <br />
        <input type="text" name="inputNombre" v-model="nombrePokemon[i]" v-if="data.porDescurbri">
        <p v-if="!data.porDescurbri">{{ data.name }}</p>
        <button v-if="data.porDescurbri" type="button" class="btn btn-outline-success mt-3" @click="descubrirPokemon(i)">Descubrir</button>
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

.descubrir {
  filter: blur(2px) grayscale(100%);
}
</style>
