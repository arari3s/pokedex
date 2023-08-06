<template>
  <div
    class="rounded-md bg-white p-4 shadow-md dark:bg-slate-500"
    @click="showPokemonDetail"
  >
    <img
      :src="getImageUrl(pokemon.id)"
      :alt="pokemon.name"
      class="mx-auto h-32 w-32"
    />
    <p class="text-slate-600 dark:text-slate-300">
      {{ formatPokemonId(pokemon.id) }}
    </p>
    <h2
      class="text-lg font-semibold text-slate-800 dark:text-slate-100 md:text-xl"
    >
      {{ capitalizeEachWord(pokemon.name) }}
    </h2>
    <div class="mt-2 flex">
      <span
        v-for="tipe in pokemon.types"
        :key="tipe.slot"
        :class="getTypeClass(tipe.type.name)"
      >
        {{ capitalizeEachWord(tipe.type.name) }}
      </span>
    </div>
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
    formatTypeName(type) {
      return type.toUpperCase()
    },
    getTypes(types) {
      // get type pokeomon
      return types.map((type) => type.type.name).join(' / ')
    },
    getImageUrl(pokemonId) {
      // Ubah URL gambar sesuai dengan official-artwork
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonId}.png`
    },
    showPokemonDetail() {
      this.$emit('showDetail', this.pokemon)
    },
    getTypeClass(type) {
      // Ganti dengan warna dan gaya yang sesuai dengan masing-masing tipe
      if (type === 'bug') {
        return 'type-bug'
      } else if (type === 'dark') {
        return 'type-dark'
      } else if (type === 'dragon') {
        return 'type-dragon'
      } else if (type === 'electric') {
        return 'type-electric'
      } else if (type === 'fairy') {
        return 'type-fairy'
      } else if (type === 'fighting') {
        return 'type-fighting'
      } else if (type === 'fire') {
        return 'type-fire'
      } else if (type === 'flying') {
        return 'type-flying'
      } else if (type === 'ghost') {
        return 'type-ghost'
      } else if (type === 'grass') {
        return 'type-grass'
      } else if (type === 'ground') {
        return 'type-ground'
      } else if (type === 'ice') {
        return 'type-ice'
      } else if (type === 'normal') {
        return 'type-normal'
      } else if (type === 'poison') {
        return 'type-poison'
      } else if (type === 'psychic') {
        return 'type-psychic'
      } else if (type === 'rock') {
        return 'type-rock'
      } else if (type === 'steel') {
        return 'type-steel'
      } else if (type === 'water') {
        return 'type-water'
      } else {
        // Tambahkan gaya untuk tipe Pokemon lainnya
        return 'type-default'
      }
    },
  },
}
</script>

<style>
/* Gaya untuk setiap tipe Pokemon */
.type-bug {
  background-color: #3b9950;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-dark {
  background-color: #595978;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-dragon {
  background-color: #61cad9;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-electric {
  background-color: #fafa72;
  color: #000;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-fairy {
  background-color: #e91268;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-fighting {
  background-color: #ef6239;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-fire {
  background-color: #fd4a5a;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-flying {
  background-color: #94b2c7;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-ghost {
  background-color: #906790;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-grass {
  background-color: #26cb50;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-ground {
  background-color: #6e491f;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-ice {
  background-color: #d8f0fa;
  color: #000;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-normal {
  background-color: #c998a6;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-poison {
  background-color: #9b69d9;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-psychic {
  background-color: #f71c92;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-rock {
  background-color: #8a3d22;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

.type-steel {
  background-color: #42bc94;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

/* Tambahkan gaya untuk tipe Pokemon lainnya */
.type-water {
  background-color: #85a8fb;
  color: #fff;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}

/* Gaya untuk tipe Pokemon default */
.type-default {
  background-color: #ccc;
  color: #000;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 4px;
}
</style>
