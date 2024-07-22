<!-- TEMPLATE -->
<template>
    <div id="CardPoke" class="container text-center my-4">
        <div class="card bg-transparent border-0" style="width:">
            <!-- Hicimos binding de clases -->
            <img :src="imagenPokemon" class="card-img-top"
                :class="esconderPokemon ? 'blur' : ''"/>
            <div class="card-body">
                <h4 v-show="!esconderPokemon">{{ pokemon.name }}</h4>
                <form v-show="esconderPokemon">
                    <input  type="text" class="form-control my-3" name="pokemon" id="pokemon" v-model="descubrirPokemon" @keyup.enter="descubrir"/>
                    <div class="d-grid">
                        <button @click.prevent="descubrir" class="btn btn-light btn-lg">Descubrir</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<!-- SCRIPTS -->
<script>
import axios from "axios";

export default {
    name: "CardPoke",
    props: {
        pokemon: Object,
    },

    data() {
        return {
            infoPokemon: {},
            esconderPokemon: true,
            descubrirPokemon:"",
        };
    },
    computed:{
        imagenPokemon(){
            return this.infoPokemon.sprites?.other['official-artwork'].front_default;
        },
    },
    // Ciclo de vida de los componentes (ejecuta lo que esta dentro de la función created)
    created() {
        this.getInfoPokemon();
    },
    methods: {
        async getInfoPokemon() {
            try {
                //  destructurar {data}
                let { data } = await axios.get(this.pokemon.url);
                this.infoPokemon = data;
            } catch (error) {
                console.log(error);
            }
        },
        // Función que nos permita descubrir el pokemon al escribir el nombre
        descubrir(){
            if(this.descubrirPokemon.toLowerCase() === this.pokemon.name.toLowerCase()){
                this.esconderPokemon = false;
                this.$emit("respuestaCorrecta");
            }else{
                console.log("incorrecto");
            }
        }
    },
};
</script>

<!-- STYLES -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
    font-family: "Pixelify Sans", sans-serif;

}

.blur {
    filter: blur(5px) drop-shadow(0px 10px 2px #393d7531) grayscale(70%);
}

.btn {
    color: rgb(22, 34, 104);

}

/* Para no arrastrar la imagen */
img{
    user-drag: none;
    -webkit-user-drag: none;
    user-select: none;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
}

h4{
    filter: drop-shadow(0px 2px 2px #181e72c2);
    text-transform: capitalize;
}

</style>
