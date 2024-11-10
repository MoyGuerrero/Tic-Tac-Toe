<script setup lang="ts">
import { ref } from 'vue';
import confetti from 'canvas-confetti'
import ButtonType from './components/ButtonType.vue';
import ButtonCat from './components/ButtonCat.vue';

const turno = ref<string>('');
const sendShift = ref<string[]>(['', '', '', '', '', '', '', '', ''])
const quantityButtons = [0, 1, 2, 3, 4, 5, 6, 7, 8]
const isDisabled = ref<boolean>(false);
const isDisabledGame = ref<boolean>(false)

const onclick = (evt: string) => {
  turno.value = evt;
  isDisabled.value = true;
};

const changeShift = (): string => turno.value = turno.value.includes('X') ? 'O' : 'X';

const winner = () => {

  const combinationsWin = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]

  for (const winner of combinationsWin) {
    const [a, b, c] = winner;
    if (sendShift.value[a] &&
      sendShift.value[a] === sendShift.value[b] &&
      sendShift.value[a] === sendShift.value[c]) {
      isDisabledGame.value = true;
      confetti({
        particleCount: 300,
        spread: 150,
        origin: { y: 0.6 }
      });
      alert(`Winner is ${sendShift.value[a]}`);
      return
    }
  }

  if (sendShift.value.every(cell => cell !== '')) {
    alert("¡Es un empate!");
    isDisabledGame.value = true;
  }
}

const selectShift = (selectPosition: number) => {
  if (!turno.value.trim()) return alert('Favor de seleccionar una opcion.');
  if (!sendShift.value[selectPosition]) {
    sendShift.value[selectPosition] = turno.value;
    changeShift();
    winner();
  }
}

const restart = () => {
  turno.value = '';
  isDisabled.value = false;
  isDisabledGame.value = false;
  sendShift.value = ['', '', '', '', '', '', '', '', '']
}
</script>

<template>
  <div class="flex justify-center gap-2 mt-10">
    <ButtonType tipo="X" :is-disabled="isDisabled" @click="onclick" />
    <ButtonType tipo="O" :is-disabled="isDisabled" @click="onclick" />
  </div>
  <div class="flex flex-col items-center justify-center mt-5 mb-5">
    <label class="text-white text-lg">Turno: <span>{{ turno }}</span></label>
  </div>
  <div class="flex justify-center mb-5">
    <button
      class="text-white dark:bg-blue-400 px-3 py-2 block w-40 text-lg dark:hover:bg-blue-600 cursor-pointer rounded-lg"
      @click="restart" v-if="isDisabledGame">Restart</button>
  </div>
  <div class="grid grid-cols-3 gap-4 border-2 dark:border-gray-600 p-4 rounded-lg">
    <div class="w-48 h-48 border-box rounded-lg" v-for="quantity in quantityButtons" :key="quantity">
      <ButtonCat :position="quantity" :is-disabled="isDisabledGame" :value="sendShift[quantity]"
        @click="selectShift" />
    </div>
  </div>
</template>

<style scoped>
.border-box {
  border: 1px solid white
}
</style>
