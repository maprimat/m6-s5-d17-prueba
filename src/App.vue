<!-- TEMPLATE -->
<template>
    <div id="app" class="background">
        <div class="container py-4">
            <header class="text-center">
                <img class="w-50 h-25" src="./assets/Pokemon.svg" alt="" />
                <h2 class="my-4">¿Quién es ese Pokémon?</h2>
                <h3 class="my-4">Pokemones descubiertos: <span>{{ contador }}</span>/20</h3>
            </header>
            <div class="row g-3 ">
                <div class="col-3" v-for="(pokemon, index) in pokemones" :key="index">
                    <!-- props(:pokemon) = valor("pokemon") -->
                    <CardPoke :pokemon="pokemon" @respuestaCorrecta="incrementar" />
                </div>
            </div>
        </div>
    </div>
</template>

<!-- SCRIPTS -->
<script>
import CardPoke from "./components/CardPoke.vue";
import axios from "axios";

export default {
    name: "App",
    components: {
        CardPoke,
    },
    data() {
        return {
            pokemones: [],
            contador: 0
        };
    },
    methods: {
        // Acá creamos una función para obtener el pokemón y usamos el manejo del error try catch, por si hay un error, no se caiga la página
        async getPoke() {
            try {
                let offset = Math.floor(Math.random() * 1000);
                let response = await axios.get(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=20`);
                // console.log(response);
                this.pokemones = response.data.results;
                this.count = this.pokemons.length; // Update the count here
            } catch (error) {
                console.log(error);
            }
        },
        // Función para incrementar
        incrementar() {
            this.contador++;
        },

    },

    // Usamos el ciclo de vida de mounted para llamar la función getPoke y así "activarla". Después de crear todo lo otro y después la llame. Created es apenas se cree la página la llame.
    mounted() {
        this.getPoke();
    },
};
</script>

<!-- STYLES -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#app {
    font-family: "Pixelify Sans", sans-serif;
    color: rgb(255, 255, 255);
}

html {
    margin: 0;
}

header {
    filter: drop-shadow(0px 2px 2px #181e72c2);
}

.background {
    background-position: center;
    background-attachment: fixed;
    background-image: url("../src/assets/background.gif");
    background-size: cover;
}
</style>
