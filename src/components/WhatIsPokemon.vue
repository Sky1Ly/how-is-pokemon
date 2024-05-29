<template>
    <div>
        <img v-if="data.porDescurbri" :src="data.image" :class="{ descubrir: data.porDescurbri }" />
        <img v-if="!data.porDescurbri" :src="data.gif" />
        <br />
        <input type="text" name="inputNombre" v-model="nombrePokemon[i]" v-if="data.porDescurbri">
        <p v-if="!data.porDescurbri">{{ data.name }}</p>
        <button v-if="data.porDescurbri" type="button" class="btn btn-outline-success mt-3"
            @click="descubrirPokemon(i)">Descubrir</button>
    </div>
</template>
<script>
export default {
    data() {
        return {
            pokemons: ["image", "gif", "name", "porDescubrir"]
        }
    },
}
</script>
<style></style>