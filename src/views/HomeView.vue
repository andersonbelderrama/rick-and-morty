<script setup>
import { ref, onMounted } from 'vue'
import Pagination from '@/components/Pagination.vue'
import SearchFilter from '@/components/SearchFilter.vue'

const characters = ref([])
const searchName = ref('')
const searchStatus = ref('')
const currentPage = ref(1)
const totalPages = ref(0)
const loading = ref(false)
const selectedCharacter = ref(null)
const showModal = ref(false)

async function fetchCharacters() {
  loading.value = true
  try {
    const params = new URLSearchParams({
      page: currentPage.value,
      name: searchName.value,
      status: searchStatus.value,
    })
    const response = await fetch(`https://rickandmortyapi.com/api/character?${params}`)
    const data = await response.json()
    characters.value = data.results
    totalPages.value = data.info.pages
  } catch (error) {
    console.error('Error fetching characters:', error)
    characters.value = []
  } finally {
    loading.value = false
  }
}

function showCharacterDetails(character) {
  selectedCharacter.value = character
  showModal.value = true
}

function closeModal() {
  showModal.value = false
  selectedCharacter.value = null
}

function changePage(page) {
  currentPage.value = page
  fetchCharacters()
}

onMounted(() => {
  fetchCharacters()
})
</script>

<template>
  <main class="min-h-screen bg-gray-900 text-white p-4">
    <!-- Search Section -->
    <div class="max-w-7xl mx-auto mb-8">
      <h1 class="text-4xl font-bold text-center mb-8 text-green-400">
        Personagens de Rick and Morty
      </h1>

      <SearchFilter
        v-model:searchName="searchName"
        v-model:searchStatus="searchStatus"
        @update:searchName="
          () => {
            currentPage = 1
            fetchCharacters()
          }
        "
        @update:searchStatus="
          () => {
            currentPage = 1
            fetchCharacters()
          }
        "
      />
    </div>

    <!-- Loading State -->
    <div v-if="loading" class="flex justify-center items-center py-12">
      <div class="animate-spin rounded-full h-16 w-16 border-t-2 border-b-2 border-green-400"></div>
    </div>

    <!-- Characters Grid -->
    <div
      v-else
      class="max-w-7xl mx-auto grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <div
        v-for="character in characters"
        :key="character.id"
        @click="showCharacterDetails(character)"
        class="bg-gray-800 rounded-lg overflow-hidden cursor-pointer transform hover:scale-105 transition-transform duration-300 shadow-lg hover:shadow-green-400/20"
      >
        <img :src="character.image" :alt="character.name" class="w-full h-48 object-cover" />
        <div class="p-4">
          <h3 class="text-xl font-semibold mb-2 truncate">{{ character.name }}</h3>
          <div class="flex items-center gap-2 mb-2">
            <span
              class="w-2 h-2 rounded-full"
              :class="{
                'bg-green-400': character.status.toLowerCase() === 'alive',
                'bg-red-400': character.status.toLowerCase() === 'dead',
                'bg-gray-400': character.status.toLowerCase() === 'unknown',
              }"
            ></span>
            <span
              >{{
                character.status === 'Alive'
                  ? 'Vivo'
                  : character.status === 'Dead'
                    ? 'Morto'
                    : 'Desconhecido'
              }}
              - {{ character.species }}</span
            >
          </div>
          <p class="text-gray-400 text-sm truncate">
            Visto por último: {{ character.location.name }}
          </p>
        </div>
      </div>
    </div>

    <!-- Pagination -->
    <Pagination :current-page="currentPage" :total-pages="totalPages" @change-page="changePage" />

    <!-- Character Details Modal -->
    <div
      v-if="showModal"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4"
      @click="closeModal"
    >
      <div
        class="bg-gray-800 rounded-lg p-6 max-w-2xl w-full transform transition-transform duration-300"
        @click.stop
      >
        <div class="flex items-start justify-between mb-4">
          <h2 class="text-2xl font-bold">{{ selectedCharacter?.name }}</h2>
          <button @click="closeModal" class="text-gray-400 hover:text-white transition-colors">
            ×
          </button>
        </div>

        <div class="flex flex-col md:flex-row gap-6">
          <img
            :src="selectedCharacter?.image"
            :alt="selectedCharacter?.name"
            class="w-full md:w-1/2 rounded-lg"
          />
          <div class="flex-1">
            <div class="mb-4">
              <h3 class="text-lg font-semibold mb-2">Estado</h3>
              <div class="flex items-center gap-2">
                <span
                  class="w-2 h-2 rounded-full"
                  :class="{
                    'bg-green-400': selectedCharacter?.status.toLowerCase() === 'alive',
                    'bg-red-400': selectedCharacter?.status.toLowerCase() === 'dead',
                    'bg-gray-400': selectedCharacter?.status.toLowerCase() === 'unknown',
                  }"
                ></span>
                <span
                  >{{
                    selectedCharacter?.status === 'Alive'
                      ? 'Vivo'
                      : selectedCharacter?.status === 'Dead'
                        ? 'Morto'
                        : 'Desconhecido'
                  }}
                  - {{ selectedCharacter?.species }}</span
                >
              </div>
            </div>
            <div class="mb-4">
              <h3 class="text-lg font-semibold mb-2">Localização</h3>
              <p>{{ selectedCharacter?.location.name }}</p>
            </div>
            <div class="mb-4">
              <h3 class="text-lg font-semibold mb-2">Origem</h3>
              <p>{{ selectedCharacter?.origin.name }}</p>
            </div>
            <div>
              <h3 class="text-lg font-semibold mb-2">Episódios</h3>
              <p>Aparece em {{ selectedCharacter?.episode.length }} episódios</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
