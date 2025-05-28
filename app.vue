<template>
  <!-- Full-viewport hero section -->
  <div
    class="min-h-screen flex flex-col items-center justify-start gap-16 py-20 px-4 bg-gradient-to-b from-slate-900 via-slate-950 to-black text-white">
    <!-- Gradient headline -->
    <h1 class="text-4xl sm:text-6xl font-extrabold tracking-tight bg-gradient-to-r from-emerald-400 to-cyan-500 bg-clip-text text-transparent text-center">
      SONG RADIO HUB STATS
    </h1>

    <!-- Giant search bar -->
    <div class="w-full max-w-3xl">
      <label for="song-search" class="sr-only">Suche deinen Song</label>
      <div class="relative">
        <input 
          id="song-search" 
          v-model="query" 
          type="text" 
          :placeholder="currentPlaceholder"
          @keyup.enter="handleKeyPress"
          class="w-full h-16 sm:h-20 rounded-full pl-8 pr-16 text-xl sm:text-2xl font-light bg-slate-800/80 backdrop-blur-md placeholder:text-slate-400 placeholder:font-extralight text-white focus:outline-none focus:ring-4 focus:ring-emerald-500/60 transition" 
        />
        <button @click="onSearch"
          class="absolute right-6 top-1/2 -translate-y-1/2 text-slate-400 hover:text-emerald-400 transition-colors">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </button>
      </div>
    </div>

    <!-- Embedded Grafana panel -->
    <div v-if="encodedQuery" class="w-full max-w-7xl overflow-hidden shadow-2xl">
      <iframe 
        :src="`https://grafana.zaubara.com/d-solo/debxnvoq3i77kf/radio-scraper?orgId=1&refresh=30s&var-searchterm=${encodedQuery}&panelId=9`" 
        width="100%" 
        height="400px"
        class="rounded-lg"
        frameborder="0">
      </iframe>

      <div class="h-16"></div>

      <iframe 
        :src="`https://grafana.zaubara.com/d-solo/debxnvoq3i77kf/radio-scraper?orgId=1&refresh=30s&var-searchterm=${encodedQuery}&panelId=11`"
        width="100%"
        height="400px"
        frameborder="0">
      </iframe>

      <div class="h-16"></div>

      <iframe 
        :src="`https://grafana.zaubara.com/d-solo/debxnvoq3i77kf/radio-scraper?orgId=1&refresh=30s&var-searchterm=${encodedQuery}&panelId=2`"
        width="100%"
        height="800"
        frameborder="0">
      </iframe>
    </div>

    <!-- Optional: Show a message when no search is active -->
    <div v-else class="text-slate-400 text-center mt-8">
      Enter a search term and press Enter to see statistics
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

// Reactive search query
const query = ref('')
const currentPlaceholder = ref('')

// Array of example searches
const placeholders = [
  'krawall',
  'seiler & speer',
  'aut of orda',
  'edmund',
  'coffeeshock company',
  'coldplay',
  'eminem'
]

let currentIndex = 0

// Function to cycle through placeholders
function cyclePlaceholder() {
  currentPlaceholder.value = placeholders[currentIndex]
  currentIndex = (currentIndex + 1) % placeholders.length
}

// Start cycling when component mounts
onMounted(() => {
  cyclePlaceholder() // Initial placeholder
  setInterval(cyclePlaceholder, 1000) // Change every 3 seconds
})

// Add reactive ref for the encoded query
const encodedQuery = ref('')

// Update search function
function onSearch() {
  if (!query.value) {
    encodedQuery.value = ''
    return
  }
  // Replace spaces with + and update encodedQuery
  encodedQuery.value = query.value.replace(/ /g, '+')
}

// Add keyup.enter event handler to input
function handleKeyPress(event: KeyboardEvent) {
  if (event.key === 'Enter') {
    onSearch()
  }
}
</script>

<!-- No additional <style> block needed – fully styled with Tailwind -->
