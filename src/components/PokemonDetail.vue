<script>
  import FETCH_STATE from '../states/fetch-state';
  import { Ruler, Scale, } from 'lucide-vue-next';
  export default {
    name: 'PokemonDetail',
    components: {
      Ruler,
      Scale,
    },
    data() {
      return {
        FETCH_STATE,
        state: FETCH_STATE.initial,
        pokemon: null,
      };
    },
    methods: {
      makeSentenceCase(name) {
        const firstLetter = String(name[0]).toUpperCase();
        return firstLetter + String(name).slice(1);
      },
      setState(state) {
        this.state = state;
      },
      setPokemon(data) {
        this.pokemon = data;
      },
      createTypeColor(type) {
        switch(type) {
          case 'normal':
            return 'bg-neutral-400';
          case 'fighting':
            return 'bg-orange-800';
          case 'flying':
            return 'bg-cyan-500';
          case 'poison':
            return 'bg-indigo-500';
          case 'ground':
            return 'bg-yellow-300';
          case 'rock':
            return 'bg-yellow-500';
          case 'bug':
            return 'bg-green-700';
          case 'ghost':
            return 'bg-violet-800';
          case 'steel':
            return 'bg-slate-500';
          case 'fire':
            return 'bg-orange-500';
          case 'water':
            return 'bg-sky-400';
          case 'grass':
            return 'bg-lime-500';
          case 'electric':
            return 'bg-yellow-300';
          case 'psychic':
            return 'bg-pink-500';
          case 'ice':
            return 'bg-sky-300';
          case 'dragon':
            return 'bg-red-600';
          case 'dark':
            return 'bg-zinc-600';
          case 'fairy':
            return 'bg-pink-400';
          case 'unknown':
            return 'bg-neutral-400';
          case 'shadow':
            return 'bg-neutral-400';
          default:
            return 'bg-neutral-400';
        }
      },
      hectogramToLbs(weight) {
        weight = (weight * 0.220462).toFixed(1);
        return String(weight);
      },
      decimeterToFoot(height) {
        height = (height * 0.328084).toFixed(1);
        return String(height);
      },
    },
    mounted() {
      this.setState(FETCH_STATE.fetchingInitial);
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.name}`, { cache: 'no-cache' })
        .then(res => res.json())
        .then(data => {
          this.setPokemon(data);
          this.setState(FETCH_STATE.fetchingInitialSucceed);
        })
        .catch(err => {
          this.setState(FETCH_STATE.fetchingInitialFailed);
        });
    },
    watch: {
      state(newState, prevState) {
        console.log(newState);
      },
    },
  }
</script>

<template>
  <p v-if="(state === FETCH_STATE.fetchingInitial || state === FETCH_STATE.initial)">Loading...</p>
  <template v-if="(state === FETCH_STATE.fetchingInitialSucceed)">
    <div class="flex flex-col items-center p-4 md:flex-row md:items-start">
      <img 
        :src="(pokemon.sprites.other['official-artwork'].front_default)"
        :alt="(pokemon.name)"
        class="h-auto w-4/5 sm:w-80 sprite"
      >
      <div class="w-full md:mt-5 md:ml-3">
        <h1 class="font-bold text-2xl mb-3 text-neutral-800">{{ makeSentenceCase(pokemon.name) }}</h1>
        <section class="mb-4">
          <h2 class="font-semibold text-lg text-neutral-600 mb-2"># Type</h2>
          <ul class="flex ">
            <template
              v-for="item in pokemon.types"
            >
              <li
                :class="`mr-2 text-white text-sm py-1 px-2 rounded-md ${createTypeColor(item.type.name)}`"
              >
                {{ makeSentenceCase(item.type.name) }}
              </li>
            </template>
          </ul>
        </section>
        <section class="mb-4">
          <h2 class="font-semibold text-lg text-neutral-600 mb-2"># Physics</h2>
          <div class="flex">
            <div class="flex items-center mr-3">
              <Scale size="18" color="black" />
              <p class="text-gray ml-1 text-sm">{{ hectogramToLbs(pokemon.weight) }} lbs</p>
            </div>
            <div class="flex items-center">
              <Ruler size="17" color="black" />
              <p class="text-gray ml-1 text-sm">{{ decimeterToFoot(pokemon.height) }} ft</p>
            </div>
          </div>
        </section>
        <section>
          <h2 class="font-semibold text-lg text-neutral-600 mb-2"># Stats</h2>
          <div class="flex">
            <div class="flex items-center mr-3">
              <Scale size="18" color="black" />
              <p class="text-gray ml-1 text-sm">{{ hectogramToLbs(pokemon.weight) }} lbs</p>
            </div>
            <div class="flex items-center">
              <Ruler size="17" color="black" />
              <p class="text-gray ml-1 text-sm">{{ decimeterToFoot(pokemon.height) }} ft</p>
            </div>
          </div>
        </section>
      </div>
    </div>
  </template>
</template>

<style scoped>
  .sprite:hover {
    animation: shake 0.5s linear forwards;
  }

  @keyframes shake {
    0% {
      transform: rotate(15deg);
    }
    25% {
      transform: rotate(-15deg);
    }
    50% {
      transform: rotate(15deg);
    }
    75% {
      transform: rotate(-15deg);
    }
    100% {
      transform: rotate(0deg);
    }
  }
</style>
