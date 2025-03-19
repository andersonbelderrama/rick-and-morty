<script setup>
import IconGithub from './icons/IconGithub.vue'
import IconLinkedin from './icons/IconLinkedin.vue'

const currentYear = new Date().getFullYear()
</script>

<template>
  <footer class="mt-5 pt-8 pb-4 text-center">
    <div class="max-w-7xl mx-auto px-4">
      <div class="flex flex-col items-center justify-center space-y-2">
        <p class="text-gray-400">Desenvolvido com ❤️ por Anderson Belderrama</p>
        <div class="flex space-x-4">
          <a
            href="https://github.com/andersonbelderrama"
            target="_blank"
            rel="noopener noreferrer"
            class="text-gray-400 hover:text-green-400 transition-colors"
          >
            <IconGithub class="w-6 h-6" />
          </a>
          <a
            href="https://www.linkedin.com/in/anderson-belderrama"
            target="_blank"
            rel="noopener noreferrer"
            class="text-gray-400 hover:text-green-400 transition-colors"
          >
            <IconLinkedin class="w-6 h-6" />
          </a>
        </div>
        <p class="text-sm text-gray-500">&copy; {{ currentYear }} Rick and Morty App</p>
      </div>
    </div>
  </footer>
</template>
