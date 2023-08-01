<template>
  <div>
    <div class="container mx-auto px-4 py-8">
      <h1 class="mb-4 text-3xl font-bold">Pokedex</h1>

      <div
        class="grid grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <!-- Sample Pokemon Card (Replace this with actual data from the API) -->
        <div
          class="rounded-md bg-white p-4 shadow-md"
          v-for="pokemon in pokemons"
          :key="pokemon.id"
        >
          <img
            src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg"
            alt="Bulbasaur"
            class="mx-auto h-24 w-24"
          />
          <h2 class="mt-2 text-lg font-semibold">{{ pokemon.name }}</h2>
          <p class="text-gray-600">{{ getTypes(pokemon.types) }}</p>
        </div>
        <!-- End of Sample Pokemon Card -->
        <!-- You can repeat the above div to show multiple Pokemon cards -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'dashboard',

  data() {
    return {
      pokemons: [],
    }
  },

  async asyncData({ $axios }) {
    // Ambil data dari API menggunakan Axios
    const response = await $axios.get(
      process.env.POKE_API_URL + '/pokemon?limit=20'
    )
    const pokemons = response.data.results

    // Ambil data detil untuk setiap Pokemon
    const pokemonsWithData = await Promise.all(
      pokemons.map(async (pokemon) => {
        const detailResponse = await $axios.get(pokemon.url)
        return detailResponse.data
      })
    )

    return {
      pokemons: pokemonsWithData,
    }
  },
  methods: {
    getTypes(types) {
      return types.map((type) => type.type.name).join(' / ')
    },
  },
}
</script>
