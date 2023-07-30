<template>
  <div>
    <div class="container mx-auto px-4 py-8">
      <h1 class="mb-4 text-3xl font-bold">Pokedex</h1>
      <div
        class="grid grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <!-- Sample Pokemon Card (Replace this with actual data from the API) -->
        <div class="rounded-md bg-white p-4 shadow-md">
          <img
            src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg"
            alt="Bulbasaur"
            class="mx-auto h-24 w-24"
          />
          <h2 class="mt-2 text-lg font-semibold">Bulbasaur</h2>
          <p class="text-gray-600">Grass/Poison</p>
        </div>
        <!-- End of Sample Pokemon Card -->
        <!-- You can repeat the above div to show multiple Pokemon cards -->
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>
