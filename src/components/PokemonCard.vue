<script>
  export default {
    name: 'PokemonCard',
    props: {
      name: String,
      url: String,
    },
    methods: {
      makeSentenceCase(name) {
        const firstLetter = String(name[0]).toUpperCase();
        return firstLetter + String(name).slice(1);
      },
      makePokemonIdFromURL(url) {
        url = url.replace('https://pokeapi.co/api/v2/pokemon/', '');
        const id = url.replace('/', '');
        return id;
      }
    },
  }
</script>

<template>
  <router-link 
    :to="`/pokemon/${name}`"
    class="card bg-white w-full h-full p-2 flex flex-col justify-center items-center rounded-md shadow"
  >
    <img 
      class="w-1/2 h-auto md:w-28 md:h-28"
      :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${makePokemonIdFromURL(url)}.png`"
      :alt="makeSentenceCase(name)"
      loading="lazy"
    />
    <p class="font-medium">{{ makeSentenceCase(name) }}</p>
  </router-link>
</template>

<style scoped>
  .card {
    cursor: pointer;
  }

  .card:hover {
    animation: shake 0.5s ease forwards;
  }

  @keyframes shake {
    0% {
      transform: rotate(10deg);
    }
    25% {
      transform: rotate(-10deg);
    }
    50% {
      transform: rotate(10deg);
    }
    75% {
      transform: rotate(-10deg);
    }
    100% {
      transform: rotate(0deg);
    }
  }
</style>