<template>
  <div v-if="pokemon">
    <div class="grid grid-cols-1 sm:grid-cols-2 sm:px-8 sm:py-12 sm:gap-x-8 md:py-16">
      <div
        class="relative z-10 col-start-1 row-start-1 px-4 pt-40 pb-3 bg-gradient-to-t from-black sm:bg-none"
      >
        <h2
          class="text-xl font-semibold text-white sm:text-2xl sm:leading-7 sm:text-black md:text-3xl"
        >{{ pokemon.name }}</h2>
      </div>
      <div class="col-start-1 row-start-2 px-4 sm:pb-16">
        <div class="flex items-center text-sm font-medium my-5 sm:mt-2 sm:mb-4">
          <div class="ml-1">
            <span class="text-black">Base Experience</span>
            <span class="sm:hidden md:inline"></span>
          </div>
          <div class="text-base font-normal mx-2">·</div>

          <span
            class="inline-flex items-center justify-center px-2 py-1 mr-2 text-xs font-bold leading-none text-red-100 bg-red-600 rounded-full"
          >{{pokemon.base_experience}} XP</span>
        </div>
        <hr class="w-16 border-gray-300 hidden sm:block" />
        <div class="flex items-center text-sm font-medium my-5 sm:mt-2 sm:mb-4">
          <div class="ml-1">
            <span class="text-black">Types</span>
            <span class="sm:hidden md:inline"></span>
          </div>
          <div class="text-base font-normal mx-2">·</div>
          <div v-for="(type, idx) in pokemon.types" :key="idx">
            <span
              class="inline-flex items-center justify-center px-2 py-1 mr-2 text-xs font-bold leading-none text-indigo-100 bg-indigo-600 rounded-full"
            >{{type.type.name}}</span>
          </div>
        </div>
        <hr class="w-16 border-gray-300 hidden sm:block" />
        <div class="flex items-center text-sm font-medium my-5 sm:mt-2 sm:mb-4">
          <div class="ml-1">
            <span class="text-black">Abilities</span>
            <span class="sm:hidden md:inline"></span>
          </div>
          <div class="text-base font-normal mx-2">·</div>
          <div v-for="(ability, idx) in pokemon.abilities" :key="idx">
            <span
              class="inline-flex items-center justify-center px-2 py-1 mr-2 text-xs font-bold leading-none text-indigo-100 bg-indigo-600 rounded-full"
            >{{ability.ability.name}}</span>
          </div>
        </div>
        <hr class="w-16 border-gray-300 hidden sm:block" />
        <br />
        <router-link :to="`/`">
          <span
            class="inline-flex items-center justify-center px-2 py-1 mr-2 font-bold leading-none text-indigo-100 bg-indigo-600 rounded-full"
          >Back</span>
        </router-link>
      </div>

      <div class="col-start-1 row-start-1 flex sm:col-start-2 sm:row-span-3">
        <div class="w-full grid grid-cols-3 grid-rows-2 gap-2">
          <div class="relative col-span-3 row-span-2 md:col-span-2">
            <img
              :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${pokemon.id}.svg`"
              alt
              class="absolute inset-0 w-full h-full object-fill bg-gray-100 sm:rounded-lg"
            />
          </div>
          <div class="relative hidden md:block">
            <img
              :src="pokemon.sprites.front_default"
              alt
              class="absolute inset-0 w-full h-full object-cover rounded-lg bg-gray-100"
            />
          </div>
          <div class="relative hidden md:block">
            <img
              :src="pokemon.sprites.back_default"
              alt
              class="absolute inset-0 w-full h-full object-cover rounded-lg bg-gray-100"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const route = useRoute();
    const pokemon = reactive({
      pokemon: null,
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        pokemon.pokemon = data;
      });
    return { ...toRefs(pokemon) };
  },
};
</script>
