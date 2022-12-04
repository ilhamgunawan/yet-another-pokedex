<script>
  import logo from '../assets/pokemon.svg';
  import PokemonDetail from '../components/PokemonDetail.vue';
  export default {
    name: 'PokemonView',
    components: { PokemonDetail },
    data() {
      return {
        logo,
      };
    },
  }
</script>

<template>
  <main class="container m-auto py-5 md:max-w-3xl"> 
    <img 
      :src="logo" 
      alt="Pokemon"
      class="mx-auto h-auto w-auto sm:h-24 mb-3"
    >
    <PokemonDetail />
  </main>
</template>