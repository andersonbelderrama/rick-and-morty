<script setup>
import { ref } from 'vue'
import IconSearch from './icons/IconSearch.vue'
import IconFilter from './icons/IconFilter.vue'

const props = defineProps({
  searchName: {
    type: String,
    required: true,
  },
  searchStatus: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['update:searchName', 'update:searchStatus'])

const statusOptions = ['alive', 'dead', 'unknown']
const statusTranslations = {
  alive: 'Vivo',
  dead: 'Morto',
  unknown: 'Desconhecido',
}
</script>

<template>
  <div class="flex flex-col md:flex-row gap-4 mb-6">
    <div class="flex-1 relative">
      <input
        :value="searchName"
        @input="emit('update:searchName', $event.target.value)"
        type="text"
        placeholder="Buscar por nome..."
        class="w-full pl-10 pr-4 py-2 rounded-lg bg-gray-800 border border-gray-700 focus:outline-none focus:border-green-400 transition-colors"
      />
      <div class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">
        <IconSearch />
      </div>
    </div>

    <div class="relative">
      <select
        :value="searchStatus"
        @change="emit('update:searchStatus', $event.target.value)"
        class="w-full pl-10 pr-4 py-2 rounded-lg bg-gray-800 border border-gray-700 focus:outline-none focus:border-green-400 transition-colors"
      >
        <option value="">Todos os Status</option>
        <option v-for="status in statusOptions" :key="status" :value="status">
          {{ statusTranslations[status] }}
        </option>
      </select>
      <div class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">
        <IconFilter />
      </div>
    </div>
  </div>
</template>
