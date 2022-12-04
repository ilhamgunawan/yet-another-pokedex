<script>
  import PokemonCard from './PokemonCard.vue';
  import CardPlaceholder from './CardPlaceholder.vue'
  import FETCH_STATE from '../states/fetch-state';
  export default {
    name: 'PokemonList',
    components: { PokemonCard, CardPlaceholder },
    data() {
      return {
        FETCH_STATE,
        nextURL: 'https://pokeapi.co/api/v2/pokemon?offset=0&limit=48',
        pokemons: [],
        totalPokemons: 0,
        state: FETCH_STATE.initial,
      }
    },
    methods: {
      updatePokemons(newPokemons) {
        this.pokemons = [...this.pokemons, ...newPokemons];
      },
      setTotalPokemons(count) {
        this.totalPokemons = count;
      },
      setNextURL(nextURL) {
        this.nextURL = nextURL;
      },
      setState(state) {
        this.state = state;
      },
      isInViewport(el) {
        const rect = el.getBoundingClientRect();
        return (
          rect.top >= 0 &&
          rect.left >= 0 &&
          rect.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
          rect.right <= (window.innerWidth || document.documentElement.clientWidth)
        );
      },
      fetchNextListener() {
        const triggerFetchNextPlaceholder = document.getElementById('triggerFetchNext');
        const isTriggerShown = this.isInViewport(triggerFetchNextPlaceholder);
        const currentState = this.state === this.FETCH_STATE.fetchingInitialSucceed || this.state === this.FETCH_STATE.fetchingNextPageSucceed;
        const shouldFetch = isTriggerShown && currentState;
        if (shouldFetch) {
          this.setState(FETCH_STATE.fetchingNextPage);
        }
      },
      fetchNextPage() {
        fetch(this.nextURL, { cache: 'no-cache' })
          .then(res => res.json())
          .then(data => {
            this.updatePokemons(data.results);
            this.setNextURL(data.next);
            this.setState(FETCH_STATE.fetchingNextPageSucceed);
          })
          .catch(err => {
            this.setState(FETCH_STATE.fetchingNextPageFailed);
          });
      },
      scrollCallback(event) {
        this.fetchNextListener();
      },
    },
    mounted() {
      this.setState(FETCH_STATE.fetchingInitial);
      fetch(this.nextURL, { cache: 'no-cache' })
        .then(res => res.json())
        .then(data => {
          this.updatePokemons(data.results);
          this.setTotalPokemons(data.count);
          this.setNextURL(data.next);
          this.setState(FETCH_STATE.fetchingInitialSucceed);
          document.addEventListener('scroll', this.scrollCallback);
          this.fetchNextListener(); // When the screen height is huge;
        })
        .catch(err => this.setState(FETCH_STATE.fetchingInitialFailed));
    },
    watch: {
      state(newState, prevState) {
        if (newState === FETCH_STATE.fetchingNextPage) {
          this.fetchNextPage();
        }
      },
    },
    unmounted() {
      document.removeEventListener('scroll', this.scrollCallback);
    },
  }
</script>

<template>
  <div class="relative grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-5 md:gap-7 place-items-center">
    <template v-if="(state === FETCH_STATE.fetchingInitial || state === FETCH_STATE.initial)" v-for="n in 48">
      <CardPlaceholder />
    </template>
    <template v-if="(state === FETCH_STATE.fetchingInitialSucceed || state === FETCH_STATE.fetchingNextPage || state === FETCH_STATE.fetchingNextPageSucceed)" v-for="pokemon in pokemons">
      <PokemonCard 
        :name="pokemon.name"
        :url="pokemon.url"
      />
    </template>
    <template v-if="(state === FETCH_STATE.fetchingNextPage)" v-for="n in 48">
      <CardPlaceholder />
    </template>
    <div id="triggerFetchNext" class="absolute top-3/4 right-0 h-4 w-4"></div>
  </div>
</template>