<template>
  <div class="absolute top-0 left-0 w-full h-full flex justify-center items-center">
    <div class="absolute top-0 left-0 z-0 bg-gray w-full h-full bg-gray-500 opacity-80"></div>
    <div 
      class="z-10 opacity-100 w-full mx-4 md:max-w-md bg-linear-to-b from-green-300 to-green-100 shadow-2xl overflow-hidden
      rounded-xl flex flex-col gap-4 py-12 px-6"
    >
      <div class="flex justify-center items-center flex-col">
        <div 
          class="p-4 mb-6 
          bg-white border-2 border-green-700 rounded-full shadow-2xl"
        >
          <PhConfetti class="text-green-500 text-6xl" /> 
        </div>
        
        <h2 class="mb-1 uppercase font-bold text-lg md:text-3xl text-green-900">
          Congratulations
        </h2>
        <p class="text-lg md:text-xl text-green-900">
          You won <span class="text-purple-500 font-bold">{{ player }}</span>!
        </p>
      </div>
      <div class="mt-auto flex gap-2 justify-center">
        <button @click="startNewGame" class="px-6 py-2 rounded-3xl bg-sky-500 hover:bg-sky-400 hover:cursor-pointer shadow text-white">
          New game
        </button>
        <button @click="closeModal" class="px-6 py-2 rounded-3xl bg-gray-400 hover:bg-gray-500 hover:cursor-pointer text-white">
          Close
        </button>
      </div>
      
    </div>
  </div>
</template>
<script setup>
import { PhConfetti } from '@kalimahapps/vue-icons';

const props = defineProps({
  player: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['newGame', 'close'])

const startNewGame = () => {
  emit('newGame')
}

const closeModal = () => {
  emit('close')
}
</script>