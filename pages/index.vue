<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="mb-6 text-3xl font-bold text-red-600">Pokédex</h1>
    <!-- Jika showDetail bernilai false, tampilkan PokemonCard -->
    <div v-if="!showDetail">
      <div class="grid grid-cols-2 gap-4 md:grid-cols-3 lg:grid-cols-4">
        <PokemonCard
          v-for="pokemon in visiblePokemons"
          :key="pokemon.id"
          :pokemon="pokemon"
          @showDetail="showPokemonDetail(pokemon)"
        />
      </div>
    </div>
    <!-- Jika showDetail bernilai true, tampilkan PokemonDetail di sebelah kanan -->
    <div v-else class="grid grid-cols-1 gap-4 sm:grid-cols-2">
      <div class="col-span-1">
        <div class="grid grid-cols-2 gap-4 md:grid-cols-2 lg:grid-cols-3">
          <PokemonCard
            v-for="pokemon in visiblePokemons"
            :key="pokemon.id"
            :pokemon="pokemon"
            @showDetail="showPokemonDetail(pokemon)"
          />
        </div>
      </div>
    </div>
    <!-- <div class="col-span-1"> -->
    <PokemonDetail
      v-if="showDetail"
      :pokemon="selectedPokemon"
      @closeDetail="closePokemonDetail"
    />
    <!-- Button close ... -->
    <!-- Kode untuk menutup PokemonDetail saat diklik -->
    <div class="fixed right-4 top-8 md:right-12 lg:right-24">
      <button
        v-if="showDetail"
        @click="closePokemonDetail"
        class="rounded-lg bg-red-500 px-3 py-1 text-white hover:bg-red-600"
      >
        Close
      </button>
    </div>
    <div ref="scrollTrigger"></div>
    <!-- Tambahkan elemen ref untuk menandai area scroll trigger -->
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
      limit: 16,
      offset: 0,
      selectedPokemon: null, // Variable untuk menyimpan data Pokemon yang dipilih
      showDetail: false, // Menambahkan variabel showDetail untuk menampilkan atau menyembunyikan PokemonDetail
    }
  },

  async asyncData({ $axios }) {
    // Ambil data awal dengan limit yang lebih kecil (misalnya 20) untuk mengurangi beban awal
    // Ambil data dari API menggunakan Axios
    const response = await $axios.get(
      process.env.POKE_API_URL + '/pokemon?limit=80'
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
    async loadMorePokemon() {
      if (this.showLoadMore) {
        // Tambahkan data lebih banyak dengan offset berikutnya
        this.offset += this.limit
        const morePokemons = this.pokemons.slice(
          this.offset,
          this.offset + this.limit
        )
        this.visiblePokemons.push(...morePokemons)

        // Tunggu sebentar sebelum mendeteksi trigger scroll berikutnya
        await this.$nextTick()
      }
    },
    onScroll() {
      // Deteksi scroll ke bawah dan memuat lebih banyak data jika sudah mencapai bagian bawah halaman
      const scrollTrigger = this.$refs.scrollTrigger
      const scrollY = window.scrollY || document.documentElement.scrollTop
      const triggerTop = scrollTrigger.getBoundingClientRect().top

      if (scrollY >= triggerTop - window.innerHeight) {
        this.loadMorePokemon()
      }
    },
  },
  mounted() {
    // Inisialisasi data awal yang akan ditampilkan
    this.visiblePokemons = this.pokemons.slice(0, this.limit)

    // Pasang event listener untuk scroll
    window.addEventListener('scroll', this.onScroll)
  },
  beforeDestroy() {
    // Hapus event listener sebelum komponen dihancurkan
    window.removeEventListener('scroll', this.onScroll)
  },
}
</script>
