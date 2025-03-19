<script setup>
import { ref } from 'vue'

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
    <input
      :value="searchName"
      @input="emit('update:searchName', $event.target.value)"
      type="text"
      placeholder="Buscar por nome..."
      class="flex-1 px-4 py-2 rounded-lg bg-gray-800 border border-gray-700 focus:outline-none focus:border-green-400 transition-colors"
    />

    <select
      :value="searchStatus"
      @change="emit('update:searchStatus', $event.target.value)"
      class="px-4 py-2 rounded-lg bg-gray-800 border border-gray-700 focus:outline-none focus:border-green-400 transition-colors"
    >
      <option value="">Todos os Status</option>
      <option v-for="status in statusOptions" :key="status" :value="status">
        {{ statusTranslations[status] }}
      </option>
    </select>
  </div>
</template>
