<template>
    <header>
        <router-link class="logo" to="/">PokéPedia</router-link>
        <div class="navs">
            <router-link v-for="nav in navs" :key="nav" :to="nav.href">{{ nav.title }}</router-link>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
    data(){
        return{
            navs: [
                {
                    title: 'home',
                    href: '/'
                },
                {
                    title: 'pokedex',
                    href: '/pokedex'
                },
                {
                    title: 'locations',
                    href: '/locations'
                },
                {
                    title: 'types',
                    href: '/types'
                },
            ]
        }
    }
}
</script>

<style lang="scss">
header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #fff;
    padding: 10px 0;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

    .logo{
        margin-left: 20px;
        text-decoration: none;
        font-size: 32px;
        font-weight: bold;
        color: #000;
    }

    .navs{
        margin-right: 20px;

        a{
            color: #000;
            text-decoration: none;
            margin: 0 10px;
            text-transform: uppercase;
            font-size: 12px;

        }
    }
}
</style>