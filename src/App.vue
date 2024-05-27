<script>
import axios from "axios";

export default {
  data() {
    return {
      getDataApi: [],
    }
  },

  methods: {
    async consultaApiRandom() {
      try {
        const datoBusqueda = Math.floor(Math.random() * 1000)
        const url = `https://pokeapi.co/api/v2/pokemon?offset=${datoBusqueda}&limit=20`

        const { data } = await axios.get(url)
        this.getDataApi = data

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
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
