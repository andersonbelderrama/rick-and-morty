<script setup>
const props = defineProps({
  message: {
    type: String,
    default: 'Nenhum personagem encontrado',
  },
  description: {
    type: String,
    default: 'Tente ajustar seus filtros de busca para encontrar outros personagens.',
  },
})
</script>

<template>
  <div class="max-w-7xl mx-auto text-center p-8 bg-gray-800 rounded-lg shadow-lg animate-fade-in">
    <svg
      class="w-16 h-16 mx-auto mb-4 text-gray-400"
      fill="none"
      stroke="currentColor"
      viewBox="0 0 24 24"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
      />
    </svg>
    <h3 class="text-xl font-semibold mb-2 text-green-400">{{ message }}</h3>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
