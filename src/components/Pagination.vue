<script setup>
const props = defineProps({
  currentPage: {
    type: Number,
    required: true,
  },
  totalPages: {
    type: Number,
    required: true,
  },
})

const emit = defineEmits(['changePage'])

const getDisplayedPages = () => {
  const maxVisiblePages = window.innerWidth < 640 ? 3 : 5
  const halfVisible = Math.floor(maxVisiblePages / 2)

  let startPage = Math.max(props.currentPage - halfVisible, 1)
  let endPage = Math.min(startPage + maxVisiblePages - 1, props.totalPages)

  if (endPage - startPage + 1 < maxVisiblePages) {
    startPage = Math.max(endPage - maxVisiblePages + 1, 1)
  }

  return Array.from({ length: endPage - startPage + 1 }, (_, i) => startPage + i)
}
</script>

<template>
  <div class="flex justify-center items-center gap-2 mt-8">
    <!-- Botão Anterior -->
    <button
      v-if="currentPage > 1"
      @click="emit('changePage', currentPage - 1)"
      class="px-3 py-1 rounded-lg bg-gray-800 hover:bg-gray-700 transition-colors text-sm"
    >
      Anterior
    </button>

    <!-- Primeira página -->
    <button
      v-if="getDisplayedPages()[0] > 1"
      @click="emit('changePage', 1)"
      class="px-3 py-1 rounded-lg bg-gray-800 hover:bg-gray-700 transition-colors text-sm"
    >
      1
    </button>

    <!-- Indicador de páginas omitidas no início -->
    <span v-if="getDisplayedPages()[0] > 2" class="text-gray-400">...</span>

    <!-- Páginas numeradas -->
    <button
      v-for="page in getDisplayedPages()"
      :key="page"
      @click="emit('changePage', page)"
      :class="[
        'px-3 py-1 rounded-lg transition-colors text-sm',
        currentPage === page ? 'bg-green-400 text-gray-900' : 'bg-gray-800 hover:bg-gray-700',
      ]"
    >
      {{ page }}
    </button>

    <!-- Indicador de páginas omitidas no final -->
    <span
      v-if="getDisplayedPages()[getDisplayedPages().length - 1] < totalPages - 1"
      class="text-gray-400"
    >
      ...
    </span>

    <!-- Última página -->
    <button
      v-if="getDisplayedPages()[getDisplayedPages().length - 1] < totalPages"
      @click="emit('changePage', totalPages)"
      class="px-3 py-1 rounded-lg bg-gray-800 hover:bg-gray-700 transition-colors text-sm"
    >
      {{ totalPages }}
    </button>

    <!-- Botão Próximo -->
    <button
      v-if="currentPage < totalPages"
      @click="emit('changePage', currentPage + 1)"
      class="px-3 py-1 rounded-lg bg-gray-800 hover:bg-gray-700 transition-colors text-sm"
    >
      Próximo
    </button>
  </div>
</template>
