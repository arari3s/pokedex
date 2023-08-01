<template>
  <div class="rounded-md bg-white p-4 shadow-md">
    <img
      :src="getImageUrl(pokemon.id)"
      :alt="pokemon.name"
      class="mx-auto h-28 w-28"
    />
    <h2 class="mt-2 text-lg font-semibold">
      {{ upperCase(pokemon.name) }}
    </h2>
    <p class="text-gray-600">
      {{ upperCase(getTypes(pokemon.types)) }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      require: true,
    },
  },

  methods: {
    // membuat teks awal capital
    upperCase(str) {
      // return str.charAt(0).toUpperCase() + str.slice(1)
      return str
        .split(' ')
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(' ')
    },

    // get type pokeomon
    getTypes(types) {
      return types.map((type) => type.type.name).join(' / ')
    },
    getImageUrl(pokemonId) {
      // Ubah URL gambar sesuai dengan official-artwork
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonId}.png`
    },
  },
}
</script>
