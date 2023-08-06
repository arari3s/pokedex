<template>
  <div :class="{ dark: isDarkTheme }" class="font-poppins">
    <div class="bg-slate-100 dark:bg-slate-800">
      <div
        class="container mx-auto flex items-center justify-between px-4 py-8"
      >
        <h1 class="text-3xl font-bold text-red-600">Pokédex</h1>
        <button @click="toggleTheme">
          <svg
            v-if="isDarkTheme"
            class="h-6 w-6"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <!-- ... SVG untuk tema gelap ... -->
            <path
              d="M17.8 13.75a1 1 0 0 0-.859-.5A7.488 7.488 0 0 1 10.52 2a1 1 0 0 0 0-.969A1.035 1.035 0 0 0 9.687.5h-.113a9.5 9.5 0 1 0 8.222 14.247 1 1 0 0 0 .004-.997Z"
              fill="#cbd5e1"
            ></path>
          </svg>

          <svg
            v-else
            id="theme-toggle-dark-icon"
            class="h-6 w-6"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 18 20"
          >
            <!-- ... SVG untuk tema terang ... -->
            <path
              d="M10 15a5 5 0 1 0 0-10 5 5 0 0 0 0 10Zm0-11a1 1 0 0 0 1-1V1a1 1 0 0 0-2 0v2a1 1 0 0 0 1 1Zm0 12a1 1 0 0 0-1 1v2a1 1 0 1 0 2 0v-2a1 1 0 0 0-1-1ZM4.343 5.757a1 1 0 0 0 1.414-1.414L4.343 2.929a1 1 0 0 0-1.414 1.414l1.414 1.414Zm11.314 8.486a1 1 0 0 0-1.414 1.414l1.414 1.414a1 1 0 0 0 1.414-1.414l-1.414-1.414ZM4 10a1 1 0 0 0-1-1H1a1 1 0 0 0 0 2h2a1 1 0 0 0 1-1Zm15-1h-2a1 1 0 1 0 0 2h2a1 1 0 0 0 0-2ZM4.343 14.243l-1.414 1.414a1 1 0 1 0 1.414 1.414l1.414-1.414a1 1 0 0 0-1.414-1.414ZM14.95 6.05a1 1 0 0 0 .707-.293l1.414-1.414a1 1 0 1 0-1.414-1.414l-1.414 1.414a1 1 0 0 0 .707 1.707Z"
              fill="#1e293b"
            ></path>
          </svg>
        </button>
      </div>
      <!-- Isi konten aplikasi Anda -->
      <Nuxt />

      <Footer />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDarkTheme: false,
    }
  },
  mounted() {
    // Cek preferensi tema yang tersimpan di localStorage saat komponen dimuat
    const storedTheme = localStorage.getItem('isDarkTheme')
    if (storedTheme !== null) {
      this.isDarkTheme = JSON.parse(storedTheme)
    }
  },
  methods: {
    toggleTheme() {
      // Ubah tema dan simpan preferensi di localStorage
      this.isDarkTheme = !this.isDarkTheme
      localStorage.setItem('isDarkTheme', JSON.stringify(this.isDarkTheme))
    },
  },
}
</script>
