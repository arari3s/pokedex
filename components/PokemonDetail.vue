<template>
  <div
    class="fixed right-4 top-24 max-w-3xl rounded-lg bg-white p-4 shadow-md dark:bg-slate-500 md:right-12 lg:right-24"
  >
    <div class="mx-6 flex items-center">
      <img
        :src="getImageUrl(pokemon.id)"
        :alt="pokemon.name"
        class="mx-auto h-28 w-28 md:h-48 md:w-48 lg:h-52 lg:w-52"
      />
      <div class="ml-4">
        <p class="mb-1 text-lg text-slate-600 dark:text-slate-300">
          {{ formatPokemonId(pokemon.id) }}
        </p>
        <h2
          class="mb-2 text-2xl font-semibold text-slate-800 dark:text-slate-100"
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
    </div>

    <table class="mb-4 w-full table-auto text-slate-800 dark:text-slate-100">
      <thead>
        <tr>
          <th>Height</th>
          <th>Weight</th>
          <th>Exp</th>
        </tr>
      </thead>
      <tbody class="text-center">
        <tr>
          <td>{{ pokemon.height / 10 }} m</td>
          <td>{{ pokemon.weight / 10 }} kg</td>
          <td>{{ pokemon.base_experience }}</td>
        </tr>
      </tbody>
    </table>

    <h3
      class="mb-2 ml-2 text-xl font-semibold text-slate-800 dark:text-slate-100"
    >
      State:
    </h3>
    <div class="m-2 flex flex-wrap">
      <ul class="w-full">
        <li v-for="stat in pokemon.stats" :key="stat.stat.name" class="mb-1">
          <div class="mb-1 flex justify-between">
            <span class="flex font-bold text-slate-800 dark:text-slate-100">
              {{ formatStatName(stat.stat.name) }}
            </span>
            <span class="stat-base text-slate-800 dark:text-slate-100">{{
              stat.base_stat
            }}</span>
          </div>
          <div class="mb-2 h-2 w-full overflow-hidden rounded-full bg-gray-200">
            <div
              :style="{ width: stat.base_stat / 2 + '%' }"
              class="h-full bg-green-500"
            ></div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      required: true,
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
    formatStatName(name) {
      // Ubah format nama statistik, misalnya "hp" menjadi "HP"
      return name
        .replace(/-/g, ' ')
        .replace(/\b\w/g, (firstChar) => firstChar.toUpperCase())
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
