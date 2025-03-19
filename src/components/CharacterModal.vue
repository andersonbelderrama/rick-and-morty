<script setup>
import IconClose from './icons/IconClose.vue'

const props = defineProps({
  character: {
    type: Object,
    required: true,
  },
  show: {
    type: Boolean,
    required: true,
  },
})

const emit = defineEmits(['close'])

function closeModal() {
  emit('close')
}
</script>

<template>
  <div
    v-if="show"
    class="fixed inset-0 backdrop-blur-sm bg-black/70 flex items-center justify-center p-4 transition-all duration-300"
    @click="closeModal"
  >
    <div
      class="bg-gray-800/95 rounded-lg p-6 max-w-2xl w-full transform transition-all duration-300 scale-100 shadow-xl border border-gray-700/50 hover:border-gray-600/50"
      :class="{ 'scale-95 opacity-0': !show }"
      @click.stop
    >
      <div class="flex items-start justify-between mb-4">
        <h2 class="text-2xl font-bold text-green-400">{{ character?.name }}</h2>
        <button
          @click="closeModal"
          class="text-gray-400 hover:text-green-400 transition-colors text-2xl"
        >
          <IconClose />
        </button>
      </div>

      <div class="flex flex-col md:flex-row gap-6">
        <img :src="character?.image" :alt="character?.name" class="w-full md:w-1/2 rounded-lg" />
        <div class="flex-1">
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2">Estado</h3>
            <div class="flex items-center gap-2">
              <span
                class="w-2 h-2 rounded-full"
                :class="{
                  'bg-green-400': character?.status.toLowerCase() === 'alive',
                  'bg-red-400': character?.status.toLowerCase() === 'dead',
                  'bg-gray-400': character?.status.toLowerCase() === 'unknown',
                }"
              ></span>
              <span
                >{{
                  character?.status === 'Alive'
                    ? 'Vivo'
                    : character?.status === 'Dead'
                      ? 'Morto'
                      : 'Desconhecido'
                }}
                - {{ character?.species }}</span
              >
            </div>
          </div>
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2">Localização</h3>
            <p>{{ character?.location.name }}</p>
          </div>
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2">Origem</h3>
            <p>{{ character?.origin.name }}</p>
          </div>
          <div>
            <h3 class="text-lg font-semibold mb-2">Episódios</h3>
            <p>Aparece em {{ character?.episode.length }} episódios</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
