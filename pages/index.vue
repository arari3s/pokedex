<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="mb-6 text-3xl font-bold text-red-600">Pokédex</h1>
    <!-- Jika showDetail bernilai false, tampilkan PokemonCard -->
    <div v-if="!showDetail">
      <div
        class="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5"
      >
        <PokemonCard
          v-for="pokemon in visiblePokemons"
          :key="pokemon.id"
          :pokemon="pokemon"
          @showDetail="showPokemonDetail(pokemon)"
        />
      </div>
      <!-- Button load more ... -->
      <div class="mt-10 flex justify-center" v-if="showLoadMore">
        <button
          @click="loadMorePokemon"
          class="rounded-xl bg-blue-500 px-24 py-3 text-white hover:bg-blue-600"
        >
          Load More ...
        </button>
      </div>
    </div>

    <!-- Jika showDetail bernilai true, tampilkan PokemonDetail di sebelah kanan -->
    <div v-if="showDetail" class="grid grid-cols-1 gap-4 sm:grid-cols-2">
      <div class="col-span-1">
        <div class="grid grid-cols-2 gap-4 md:grid-cols-2 lg:grid-cols-3">
          <PokemonCard
            v-for="pokemon in visiblePokemons"
            :key="pokemon.id"
            :pokemon="pokemon"
            @showDetail="showPokemonDetail(pokemon)"
          />
        </div>
        <!-- Button load more ... -->
        <div class="mt-6 flex justify-center" v-if="showLoadMore">
          <button
            @click="loadMorePokemon"
            class="rounded-xl bg-blue-500 px-24 py-3 text-white hover:bg-blue-600"
          >
            Load More ...
          </button>
        </div>
      </div>
      <div class="col-span-1">
        <PokemonDetail
          :pokemon="selectedPokemon"
          @closeDetail="closePokemonDetail"
        />
        <!-- </div> -->
        <!-- Button close ... -->
        <!-- Kode untuk menutup PokemonDetail saat diklik -->
        <div class="mt-6 flex justify-center" v-if="showLoadMore">
          <button
            @click="closePokemonDetail"
            class="rounded-xl bg-blue-500 px-24 py-3 text-white hover:bg-blue-600"
          >
            Close
          </button>
        </div>
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
      limit: 10,
      offset: 0,
      selectedPokemon: null, // Variable untuk menyimpan data Pokemon yang dipilih
      showDetail: false, // Menambahkan variabel showDetail untuk menampilkan atau menyembunyikan PokemonDetail
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
    showPokemonDetail(pokemon) {
      this.selectedPokemon = pokemon // Menyimpan data Pokemon yang dipilih
      this.showDetail = true // Menampilkan PokemonDetail saat tombol pada PokemonCard diklik
    },
    closePokemonDetail() {
      this.selectedPokemon = null
      this.showDetail = false // Menutup PokemonDetail saat tombol close di PokemonDetail diklik
    },
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
