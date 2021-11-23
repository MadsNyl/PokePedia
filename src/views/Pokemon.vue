<template>
    <div class="main">
        <div class="sprites">
            <div v-for="sprite in sprites" :key="sprite" class="sprite">
                <img :src="sprite" alt="">
            </div>  
        </div>
        <div class="general-info">
            <div class="general">
                <h1>{{ data.name }}</h1>
                <div class="types">
                    <p v-for="type in data.types" :key="type">
                        {{ type.type.name }}
                    </p>
                </div>
                <div class="body">
                    <p><strong>Height</strong>: {{ data.height * 10 }} cm</p>
                    <p><strong>Weight</strong>: {{ data.weight / 10 }} kg</p>
                </div>
            </div>
            <div class="abilities">
                <h1>abilities</h1>
                <div>
                    <p v-for="ability in data.abilities" :key="ability">
                        {{ ability.ability.name }}
                    </p>
                </div>
            </div>
        </div>
        <div class="attribute-info">
            <div class="stats">
                <div class="stat" v-for="stat in data.stats" :key="stat">
                    <div class="stat-info">
                        <strong>{{ stat.stat.name }}</strong>
                        <h3>-</h3>
                        <p>{{ stat.base_stat }}</p>
                    </div>
                </div>
            </div>
            <div class="moves">
                <table>
                    <tr>
                        <th>Name</th>
                        <th>Power</th>
                        <th>Accuracy</th>
                        <th>PP</th>
                    </tr>
                    <tr v-for="move in moves" :key="move">
                        <td>{{ move.name }}</td>
                        <td>{{ move.power }}</td>
                        <td>{{ move.accuracy }}</td>
                        <td>{{ move.pp }}</td>
                    </tr>
                </table>
            </div>
        </div>
        <div class="evolution-chain">
            <h1>evolution-chain</h1>
            <div class="evolution-info">
                <div class="first">
                    <img :src="sprites[0]" alt="">
                    <h2>{{ data.name }}</h2>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Pokemon',
    data() {
        return {
            data: {},
            sprites: [],
            moves: [],
            evolution: {}
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

            this.sprites.push(response.data.sprites.front_default)
            this.sprites.push(response.data.sprites.back_default)
            this.sprites.push(response.data.sprites.front_shiny)
            this.sprites.push(response.data.sprites.back_shiny)

            for (let move in response.data.moves){
                const moves = await axios.get(`${response.data.moves[move].move.url}`) 
                this.moves.push(moves.data)
            }

            const specieData = await axios.get(`${response.data.species.url}`) 
            const evolutionData = await axios.get(`${specieData.data.evolution_chain.url}`)
            this.evolution = evolutionData.data

            console.log(this.evolution)
        },

    },
}
</script>

<style lang="scss">
.main{
    width: 100%;

    .sprites{
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 20px 0;

        .sprite{
            padding: 10px;
            background: #fff;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 10px;
        }
    }

    .general-info{
            margin: 60px 20px;
            display: flex;
            justify-content: space-around;
            align-items: center;

            .general{
                padding: 10px 140px;
                height: 150px;
                background: #fff;
                box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                border-radius:  8px;

                h1{
                    text-transform: uppercase;
                    font-size: 28px;
                }

                .types{
                    display: flex;
                    padding: 10px 0;

                    p{
                        margin: 0 10px;
                        padding: 5px 10px;
                        border-radius: 4px;
                        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                        background: rgb(163, 24, 24);
                        color: #fff;
                    }
                }

                .body{
                    display: flex;
                    padding: 10px 0;

                    p{
                        margin: 0 10px;
                    }
                }
            }

            .abilities{
                padding: 10px 140px;
                height: 150px;
                background: #fff;
                box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                border-radius:  8px;

                h1{
                    text-transform: uppercase;
                    font-size: 28px;
                }

                div{
                    display: flex;
                    flex-wrap: wrap;

                    p{
                        margin: 20px 10px;
                        padding: 5px 20px;
                        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                        border-radius: 4px;
                        background: rgb(12, 12, 36);
                        color: #fff;
                    }
                }
            }
    }

    .attribute-info{
        margin: 30px 20px;
        display: flex;
        justify-content: space-between;

        .stats{
                padding: 10px 80px;
                background: #fff;
                box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                border-radius:  8px;

                .stat{

                    .stat-info{
                        display: flex;
                        align-items: center;

                        strong{
                            text-transform: uppercase;
                            font-size: 16px;
                        }

                        h3{
                            margin: 0 10px;
                        }
                    }
                }
            }

            .moves{
                width: 800px;
                max-height: 300px;
                padding: 20px 20px;
                background: #fff;
                box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
                border-radius:  8px;
                overflow-y: scroll;

                table{
                    width: 100%;

                    td, th{
                        border: 1px solid #ddd;
                        text-align: left;
                        padding: 8px;
                    }

                    tr:nth-child(even){
                        background: #ddd;
                    }
                }
            }
    }
}
</style>