<template>
  <div>
    <div
      class="p-10 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-4 lg:grid-cols-4 xl:grid-cols-4 gap-5"
    >
      <div
        class="rounded overflow-hidden shadow-lg"
        v-for="pokemon in pokemons"
        :key="pokemon.name"
      >
        <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
          <img
            class="object-contain h-48 w-full"
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${urlIdLookup[pokemon.name]}.svg`"
            alt="{{pokemon.name}}"
          />
          <div class="py-4 text-center">
            <div class="font-bold text-xl mb-2">{{pokemon.name}}</div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, onBeforeMount, onMounted, onUnmounted } from "vue";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      currentPage: 0,
      totalPages: 0,
      limit: 20,
      text: "",
      urlIdLookup: {},
      isInitialRequestLoading: true,
      isLoading: false,
    });

    const fetchPokemons = async () => {
      // console.log(state.currentPage);
      try {
        const response = await fetch(
          `https://pokeapi.co/api/v2/pokemon?offset=${state.currentPage}&limit=${state.limit}`
        );
        const parsedResponse = await response.json();
        //state.pokemons = parsedResponse.results;
        state.pokemons = [...state.pokemons, ...parsedResponse.results];
        // console.log(state.pokemons);
        state.urlIdLookup = state.pokemons.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
        state.totalPages = parsedResponse.count;
      } catch (err) {
        console.log(err); // state-of-the-art-error-handling
      }
    };

    onBeforeMount(() => {
      fetchPokemons();
    });

    onMounted(() => {
      window.addEventListener("scroll", handleScroll);
    });

    onUnmounted(() => {
      window.removeEventListener("scroll", handleScroll);
    });

    const handleScroll = () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        state.currentPage = state.currentPage + 20;

        state.isLoading = true;
        fetchPokemons();
        state.isLoading = false;
      }
    };

    // fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
    //   .then((res) => res.json())
    //   .then((data) => {
    //     //console.log(data);
    //     console.log(data.results);
    //     state.pokemons = data.results;
    //     state.urlIdLookup = data.results.reduce(
    //       (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
    //       {}
    //     );
    //   });
    return { ...toRefs(state) };
  },
};
</script>

<style scoped>
.loading-spinner {
  padding-bottom: 3rem;
}
</style>