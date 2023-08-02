<template>
  <div class="rounded-md bg-white p-4 shadow-md">
    <h2 class="text-xl font-semibold">{{ pokemon.name }}</h2>
    <!-- Tampilkan data detail Pokemon di sini sesuai kebutuhan -->
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
}
</script>
