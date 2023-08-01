<template>
  <div>
    <div class="container mx-auto px-4 py-8">
      <h1 class="mb-4 text-3xl font-bold">Pokedex</h1>

      <div
        class="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5"
      >
        <!-- Sample Pokemon Card (Replace this with actual data from the API) -->
        <PokemonCard
          v-for="pokemon in visiblePokemons"
          :key="pokemon.id"
          :pokemon="pokemon"
        >
        </PokemonCard>
        <!-- End of Sample Pokemon Card -->
      </div>
      <!-- Load more ... -->
      <div class="mt-10 flex justify-center" v-if="showLoadMore">
        <button
          @click="loadMorePokemon"
          class="rounded-xl bg-blue-500 px-24 py-3 text-white hover:bg-blue-600"
        >
          Load More ...
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import PokemonCard from '~/components/PokemonCard.vue'
export default {
  components: { PokemonCard },
  layout: 'dashboard',

  data() {
    return {
      pokemons: [],
      visiblePokemons: [],
      limit: 15,
      offset: 0,
    }
  },

  async asyncData({ $axios }) {
    // Ambil data dari API menggunakan Axios
    const response = await $axios.get(
      process.env.POKE_API_URL + '/pokemon?limit=300'
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

  computed: {
    showLoadMore() {
      return this.visiblePokemons.length < this.pokemons.length
    },
  },
  methods: {
    loadMorePokemon() {
      this.offset += this.limit
      this.visiblePokemons = this.pokemons.slice(0, this.offset + this.limit)
    },
  },
  mounted() {
    // Inisialisasi data awal yang akan ditampilkan
    this.visiblePokemons = this.pokemons.slice(0, this.limit)
  },
}
</script>
