<template>
  <div class="flex h-screen flex-col justify-between font-poppins">
    <div class="app" :class="{ 'dark-theme': isDarkTheme }">
      <button @click="toggleTheme" class="theme-toggle">
        <svg
          v-if="isDarkTheme"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="h-6 w-6"
        >
          <!-- ... SVG untuk tema gelap ... -->
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M5 13l4 4L19 7"
          />
        </svg>
        <svg
          v-else
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="h-6 w-6"
        >
          <!-- ... SVG untuk tema terang ... -->
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 1c5.523 0 10 4.477 10 10s-4.477 10-10 10S2 16.523 2 11 6.477 1 12 1zm0 2a8 8 0 100 16 8 8 0 000-16z"
          />
        </svg>
      </button>
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

<style scoped>
/* Gaya tema gelap */
.dark-theme {
  background-color: #333;
  color: #fff;
}

/* Gaya tombol toggle tema */
.theme-toggle {
  background: none;
  border: none;
  cursor: pointer;
}
</style>
