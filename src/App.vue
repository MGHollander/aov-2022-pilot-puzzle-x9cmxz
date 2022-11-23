<script setup>
import { reactive, ref } from 'vue'

const turn = ref('X')
const steps = reactive({
  X : [],
  O : [],
})
const winner = ref(null)

const check = (player) => {
  if (
       (steps[player].includes(1) && steps[player].includes(2) && steps[player].includes(3))
    || (steps[player].includes(4) && steps[player].includes(5) && steps[player].includes(6))
    || (steps[player].includes(7) && steps[player].includes(8) && steps[player].includes(9))
    || (steps[player].includes(1) && steps[player].includes(4) && steps[player].includes(7))
    || (steps[player].includes(2) && steps[player].includes(5) && steps[player].includes(8))
    || (steps[player].includes(3) && steps[player].includes(6) && steps[player].includes(9))
    || (steps[player].includes(1) && steps[player].includes(5) && steps[player].includes(9))
    || (steps[player].includes(3) && steps[player].includes(5) && steps[player].includes(7)) 
  ) {
    winner.value = player;
  }

  else if (steps.X.length + steps.O.length === 9) {
      winner.value = 'draw'
  }
}

const click = (event, cell) => {
  if (steps.X.includes(cell) || steps.O.includes(cell) || winner.value) {
    return;
  }
  
  steps[turn.value].push(cell)
  
  check(turn.value)
  
  turn.value = turn.value === 'X' ? 'O' : 'X'
} 

const reset = () => {
  winner.value = null
  turn.value = 'X'
  steps.X = []
  steps.O = []
}
</script>

<template>
  <div class="grid w-full h-full place-items-center">
    <div>
      <div class="grid grid-cols-3">
        <div 
          v-for="cell in 9" 
          @click="click($event, cell)" 
          class="w-20 h-20 text-6xl grid place-items-center" 
          :class="{ 'border-b': cell < 7, 'border-r': cell % 3 }" 
        >
          <template v-if="steps.X && steps.X.includes(cell)">X</template>
          <template v-if="steps.O && steps.O.includes(cell)">O</template>
        </div>
      </div>

      <div class="mt-4 text-center">
        <template v-if="! winner">
          It's <span class="text-4xl">{{ turn }}</span>'s turn!
        </template>

        <template v-else>
         <div class="text-4xl">
            <template v-if="winner === 'draw'">
              It's a draw
            </template>

            <template v-else>
            {{ winner }} won!
            </template>
          </div>

          <button @click="reset" class="mt-4 bg-green hover:bg-gray-dark transition-all hover:text-green px-4 py-2 rounded">Play again!</button>
        </template>
      </div>
    </div>
  </div>
</template>
