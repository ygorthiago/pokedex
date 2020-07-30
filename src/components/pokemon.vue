<template>
    <div id="pokemon">
        
        <div class="card">
        <div class="card-image">
            <figure>
            <img :src="currentImg" alt="Placeholder image" width="200" height="200">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
                <div class="media-content">
                    <p class="title is-4">{{num}} - {{name | upper}}</p>
                    <p class="subtitle">Type: <br> {{pokemon.type}}</p>
                    <div class="subtitle">Abilities: 
                        <div v-for="(ability, index) in pokemon.abilities" :key="ability.index">
                            <p class="subtitle is-6"> {{pokemon.abilities[index].ability.name | upper}} </p>
                                                    
                        </div>
                    
                    </div>
                    
                </div>
            </div>

            <div class="content">
                <button class="button is-medium is-fullwidth" @click="changeSprite" @>Change Sprite</button>
            </div>
        </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.abilities = res.data.abilities;            
            this.pokemon.frontSprite = res.data.sprites.front_default;
            this.pokemon.backSprite = res.data.sprites.back_default;
            this.currentImg = this.pokemon.frontSprite;
            
        })
    },
    data(){
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                frontSprite: '',
                backSprite: ''
            }
        }
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    filters: {
        upper: function(value) {
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    methods: {
        changeSprite: function (){
            if (this.isFront) {
                this.isFront = false;
                this.currentImg = this.pokemon.backSprite;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.frontSprite;
            }
        }
    //     ,
    //     abilityEffect: function(index) {
    //         var url = this.pokemon.abilities[index].ability.url;
    //         // return `<p>${url}</p>`
    //       console.log(url)
    //             axios.get(url).then(res => {
            
    //                 this.effect = res.data.effect_entries[1].effect           

    //             })
    //             console.log(this.effect)
    //             // return this.effect;
            

    //         }
                        
        
    }
}
</script>

<style>
    #pokemon{
        margin-top: 2%;
    }
</style>