<template lang="">
    <h1 v-if="!pokemon">Espere por favor!!</h1>
    <div v-else>
        <h1>Quien es este pokemon</h1>
        <!-- PICTURE -->
        <PokemonPicture :pokemonId="pokemon.id " :showPokemon="showPokemon" />
        <PokemonOptions @selection="checkAnswer($event);" :pokemons="pokemonArr"/>
        <!-- OPCIONES -->
        <template v-if="showAnswer">
            <h2>{{ message }}</h2>
            <button @click="newGame">
                Nuevo juego
            </button>
        </template>
        
    </div>
    
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture";
import PokemonOptions from "@/components/PokemonOptions";

import getPokemonsOptions from "@/helpers/getPokemonOptions";


console.log(getPokemonsOptions());

export default {
    components:{
        PokemonPicture,
        PokemonOptions
    },
    data(){
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon:  false,
            showAnswer: false,
            message: ''
        
        }
    },
    methods:{
        async mixPokemonsArray(){
            this.pokemonArr = await getPokemonsOptions()

            const rndInt = Math.floor( Math.random() * 4 );
            this.pokemon = this.pokemonArr[rndInt];
        },
        checkAnswer(selectedId){
            this.showPokemon = true;
            this.showAnswer = true;

            if(selectedId === this.pokemon.id ){
                this.message = `Correcto, ${this.pokemon.name}`
            }else{
                this.message = `Ups, era ${this.pokemon.name}`
            }
        },
        newGame(){
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr =  []
            this.pokemon = null
            this.mixPokemonsArray()
        }
    },
    mounted(){
        this.mixPokemonsArray();
    }
    
}
</script>
