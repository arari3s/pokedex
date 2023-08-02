<template>
  <div class="container mx-auto px-4 py-8">
    <div class="rounded-md bg-white p-4 shadow-md">
      <h1 class="mb-4 text-3xl font-bold">
        {{ formatPokemonId(pokemon.id) }} -
        {{ capitalizeEachWord(pokemon.name) }}
      </h1>
      <img
        :src="getImageUrl(pokemon.id)"
        :alt="pokemon.name"
        class="mx-auto h-24 w-24"
      />
      <p class="text-gray-600">{{ getTypes(pokemon.types) }}</p>
      <!-- Tampilkan detail Pokemon lainnya sesuai kebutuhan -->
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ params, $axios }) {
    // Ambil data detil Pokemon berdasarkan ID dari parameter dinamis
    const response = await $axios.get(
      process.env.POKE_API_URL + `/${params.id}`
    )
    const pokemon = response.data

    return {
      pokemon: pokemon,
    }
  },
  methods: {
    capitalizeEachWord(str) {
      // membuat teks awal capital
      // return str.charAt(0).toUpperCase() + str.slice(1)
      return str
        .split(' ')
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ')
    },
    formatPokemonId(pokemonId) {
      // Menambahkan nol di depan ID (4 digit)
      return String(pokemonId).padStart(4, '0')
    },
    getTypes(types) {
      // get type pokeomon
      return types.map((type) => type.type.name).join(' / ')
    },
    getImageUrl(pokemonId) {
      // Ubah URL gambar sesuai dengan official-artwork
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonId}.png`
    },
  },
}
</script>
