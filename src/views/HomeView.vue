<script>
import PokemonList from '../components/PokemonList.vue';
import logo from '../assets/pokemon.svg';
export default {
  name: 'HomeView',
  components: { PokemonList },
  data() {
    return {
      logo,
    };
  },
}
</script>

<template>
  <main class="container m-auto p-5"> 
    <img 
      :src="logo" 
      alt="Pokemon"
      class="mx-auto h-auto w-auto sm:h-24 mb-7"
    >
    <PokemonList />
  </main>
</template>
