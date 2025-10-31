<template>
  <div class="absolute top-0 left-0 w-full h-full flex justify-center items-center">
    <div class="absolute top-0 left-0 z-0 bg-gray w-full h-full bg-gray-500 opacity-80"></div>
    <div 
      class="z-10 opacity-100 w-full mx-4 md:max-w-md bg-linear-to-b from-red-300 to-red-100 shadow-2xl overflow-hidden
      rounded-xl flex flex-col gap-4 py-12 px-6"
    >
      <div class="flex justify-center items-center flex-col">
        <div 
          class="p-4 mb-6 
          bg-white border-2 border-red-700 rounded-full shadow-2xl"
        >
          <FaRegHandshake  class="text-red-500 text-6xl" /> 
        </div>
        
        <h2 class="text-lg md:text-3xl mb-1 uppercase font-bold text-red-600">
          DRAW
        </h2>
        <p class="text-lg md:text-xl text-red-900">
          It's a tie. 
        </p>
      </div>
      <div class="mt-auto text-center">
        <button @click="startNewGame" class="px-6 py-2 rounded-3xl bg-sky-500 hover:bg-sky-400 hover:cursor-pointer text-white shadow">
          Play again
        </button>
      </div>
      
    </div>
  </div>
</template>
<script setup>
import { FaRegHandshake } from '@kalimahapps/vue-icons';

const props = defineProps({
  player: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['newGame'])

const startNewGame = () => {
  emit('newGame')
}
</script>