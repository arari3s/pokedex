<template class>
  <!-- <div class="dark:bg-slate-900"> -->
  <div class="container mx-auto px-4 py-8">
    <!-- <div class="flex items-center space-x-2">
        <button
          @click="toggleTheme"
          class="text-gray-500 hover:text-gray-700 focus:outline-none"
        >
          <svg
            v-if="isDarkTheme"
            xmlns="http://www.w3.org/2000/svg"
            height="30px"
            viewBox="0 0 384 512"
          >
            <path
              d="M223.5 32C100 32 0 132.3 0 256S100 480 223.5 480c60.6 0 115.5-24.2 155.8-63.4c5-4.9 6.3-12.5 3.1-18.7s-10.1-9.7-17-8.5c-9.8 1.7-19.8 2.6-30.1 2.6c-96.9 0-175.5-78.8-175.5-176c0-65.8 36-123.1 89.3-153.3c6.1-3.5 9.2-10.5 7.7-17.3s-7.3-11.9-14.3-12.5c-6.3-.5-12.6-.8-19-.8z"
              fill="#FAFAFD"
            />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            height="30px"
            viewBox="0 0 512 512"
          >
            <path
              d="M361.5 1.2c5 2.1 8.6 6.6 9.6 11.9L391 121l107.9 19.8c5.3 1 9.8 4.6 11.9 9.6s1.5 10.7-1.6 15.2L446.9 256l62.3 90.3c3.1 4.5 3.7 10.2 1.6 15.2s-6.6 8.6-11.9 9.6L391 391 371.1 498.9c-1 5.3-4.6 9.8-9.6 11.9s-10.7 1.5-15.2-1.6L256 446.9l-90.3 62.3c-4.5 3.1-10.2 3.7-15.2 1.6s-8.6-6.6-9.6-11.9L121 391 13.1 371.1c-5.3-1-9.8-4.6-11.9-9.6s-1.5-10.7 1.6-15.2L65.1 256 2.8 165.7c-3.1-4.5-3.7-10.2-1.6-15.2s6.6-8.6 11.9-9.6L121 121 140.9 13.1c1-5.3 4.6-9.8 9.6-11.9s10.7-1.5 15.2 1.6L256 65.1 346.3 2.8c4.5-3.1 10.2-3.7 15.2-1.6zM160 256a96 96 0 1 1 192 0 96 96 0 1 1 -192 0zm224 0a128 128 0 1 0 -256 0 128 128 0 1 0 256 0z"
            />
          </svg>
        </button>
      </div> -->

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
  <!-- </div> -->
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
    // toggleTheme() {
    //   this.isDarkTheme = !this.isDarkTheme
    //   if (this.isDarkTheme) {
    //     // Terapkan gaya dark theme
    //     document.documentElement.classList.add('dark')
    //   } else {
    //     // Hapus gaya dark theme
    //     document.documentElement.classList.remove('dark')
    //   }
    // },
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
