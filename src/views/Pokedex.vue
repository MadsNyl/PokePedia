<template>
  <div class="main" >
      <div class="search">
          <input type="text" v-model="name">
          <i @click="search()" class="fab fa-searchengin"></i>
      </div>
      <div class="overview">
          <div class="error" v-if="this.error">
              {{ error }}
          </div>
          <div v-else class="pokemon" v-for="pokemon in detailedData" :key="pokemon">
              <img :src="pokemon.sprites.front_default" alt="">
              <p>{{ pokemon.name }}</p>
              <router-link :to="{ name: 'Pokemon' , params: { id: pokemon.id } }">details</router-link>
          </div>
      </div>
        <div v-if="!this.error" class="page-nav">
            <i @click="previousPage()" class="fas fa-caret-left"></i>
            <p>{{ page + 1 }}</p>
            <i @click="nextPage()" class="fas fa-caret-right"></i>
        </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Pokedex',
    data(){
        return{
            data: [],
            detailedData: [],
            page: 0,
            name: '',
            error: null
        }
    },
    mounted(){
        this.getAllPokemon(this.page)
    },
    methods:{
        async getAllPokemon(page){
            const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?offset=${page * 20}&limit=20`)
            this.data = response.data.results
            for(let pokemon of this.data){
                const response = await axios.get(`${pokemon.url}`)
                this.detailedData.push(response.data)
            }
        },

        async search(){
            try {
                const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.name}`)
                this.error = null
                this.detailedData = []
                this.detailedData.push(response.data)
            } catch (error) {
                if (error.message === 'Request failed with status code 404'){
                    this.error = 'Sorry, I could not find that Pokemon...'
                }
            }
        },

        nextPage(){
            if (this.page <= 55){
                this.page++
                this.detailedData = []
                this.getAllPokemon(this.page)
            } else {
                this.page = this.page
            }
        },

        previousPage(){
            if (this.page > 0){
                this.page--
                this.detailedData = []
                this.getAllPokemon(this.page)
            } else {
                this.page = this.page
            } 
        },
    }
}
</script>

<style lang="scss">
.main{
    max-width: 100%;

    .search{
        display: flex;
        justify-content: center;
        align-items: center;
    }
    
    .overview{
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

        .error{
            padding-top: 200px;
        }

        .pokemon{
            background: #fff;
            text-align: center;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
            width: 160px;
            height: 200px;
            margin: 10px 20px;
            padding: 10px 20px;
        }
    }

    .page-nav{
        display: flex;
        justify-content: center;
        align-items: center;

        p{
            margin: 0 10px;
            font-size: 24px;
        }

        i{
            cursor: pointer;
            font-size: 24px;
            transition: .3s ease-in-out;

            &:hover{
                opacity: .5;
            }
        }
    }

}
</style>