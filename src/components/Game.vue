<template>
  <div class="flex flex-col justify-center">
    <div class="text-center mb-4">
      <h2>Turn: {{ playerName }}</h2>
    </div>
    <div class="flex justify-center">
      <div id="board" class="border-2 border-gray-600 shadow grid grid-cols-3 mb-6 bg-gray-100">
        <button 
          v-for="(item, index) in board" 
          :class="
            [
              `border-2 w-20 h-20 flex justify-center items-center hover:cursor-pointer disabled:cursor-default`,
              item == 'X' ? 'bg-purple-200' : item == 'O' ? 'bg-amber-200' : ''
            ]
          " 
          :key="index"
          @click="handleClick(index)"
          :disabled="isGameOver"
        >
          <AkCross v-if="item == 'X'" class="text-xl font-bold" />
          <AkCircle v-else-if="item=='O'" class="text-xl font-bold" />
        </button>
      </div>
    </div>
    
    <div class="text-center">
      <button @click="resetGame" 
        class="px-6 py-2 rounded-3xl bg-sky-500 hover:bg-sky-400 hover:cursor-pointer text-white font-semibold"
      >Start a New Game</button>
    </div>
    <Teleport to="body">
      <WinnerAlert v-if="gameResult == 'win'" :player="winner" @new-game="newGame" @close="closeModal" />
      <DrawAlert v-if="gameResult == 'draw'" @new-game="newGame" />
    </Teleport>
  </div>
</template> 
<script setup>
import { computed, ref, Teleport } from 'vue'
import WinnerAlert from './WinnerAlert.vue'
import DrawAlert from './DrawAlert.vue'
import { AkCross, AkCircle } from '@kalimahapps/vue-icons';

const board = ref(['', '', '', '', '', '', '', '', ''])

const currentPlayer = ref('X')
const winner = ref(null) 
const isGameOver = ref(false)
const gameMode = ref(['new', 'win', 'draw'])
const gameResult = ref('new')

const winningPatterns = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // Top, Middle, Bottom Row
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // Left, Middle, Right Column
  [0, 4, 8], [2, 4, 6]             // Top-left to Bottom Right, Top-right to Bottom-left Diagonal
];

const playerName = computed(() => 
  currentPlayer.value == 'X' ? 'Player 1' : 'Player 2'
)

function handleClick(index) {
  if(!currentPlayer.value || board.value[index]) return
  board.value[index] = currentPlayer.value
  console.log('board', board.value)
  checkGame()  
}

function checkGame () {
  let result = ''
  winningPatterns.forEach(pattern => {
    const [a, b, c ] = pattern
    if(board.value[a] == currentPlayer.value && board.value[b] == currentPlayer.value && board.value[c] == currentPlayer.value) {
      result = 'win'
    }
  });

  if(result) {
    endGame(result);
    return
  } 
  
  const isDraw = board.value.every(cell => cell !== '')
  if(isDraw) {
    endGame('draw') 
    return
  }

  togglePlayer()
}

function togglePlayer () {
  if(currentPlayer.value == 'X') {
    currentPlayer.value = 'O'
  } else {  
    currentPlayer.value = 'X'
  }
}

function endGame (result) {
  winner.value = playerName.value
  gameResult.value = result
  isGameOver.value = true
}

function resetGame () {
  gameResult.value = 'new'
  board.value = ['', '', '', '', '', '', '', '', '']
  isGameOver.value = false
  currentPlayer.value = 'X'
}

function newGame() {
  resetGame()
}

const closeModal = () => {
  gameResult.value = 'new'
}
</script>