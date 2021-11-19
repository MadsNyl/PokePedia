<template>
    <div>
        <h1>{{data.name}}</h1>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Pokemon',
    data() {
        return {
            data: {}
        }
    },
    mounted(){
        this.getPokemon()
    },
    methods: {
        async getPokemon(){
            const id = this.$route.params.id
            const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
            this.data = response.data
        }
    },
}
</script>

<style>

</style>