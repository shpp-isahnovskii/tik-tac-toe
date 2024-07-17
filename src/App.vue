<script setup>
import TheField from '@/components/TheField.vue';
import { reactive, computed } from 'vue';

const initialState = {
  step: 0,
  activePlayer: 1,
  winner: null,
};

const field = reactive(new Array(9).fill(null));
const state = reactive({ ...initialState });

const gameOver = computed(() =>
  state.step === 9
    ? "it's a draw"
    : state.winner !== null
      ? `${state.winner === 0 ? 'O' : 'X'} wins the game`
      : '',
);

const updateGameState = (clickedCell) => {
  if (gameOver.value) return;
  state.step++;
  field[clickedCell] = state.activePlayer;
  checkWinCondition();
  changeActivePlayer();
};

const checkWinCondition = () => {
  if (!haveWinner()) return;
  state.winner = state.activePlayer;
};

const changeActivePlayer = () => {
  state.activePlayer = state.activePlayer === 0 ? 1 : 0;
};

const winCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

const haveWinner = () => {
  return winCombinations.find((combo) => {
    return combo.every((val) => field[val] === state.activePlayer);
  });
};

const resetState = () => {
  Object.assign(state, { ...initialState });
  field.fill(null);
};
</script>

<template>
  <div class="flex flex-col h-full w-max justify-center mx-auto select-none">
    <div class="mb-3 h-7 text-lg mx-auto" v-show="gameOver">
      {{ gameOver }}
    </div>
    <TheField
      class="mb-4"
      :data="field"
      :game-over="state.winner"
      @update:data="updateGameState"
    />
    <button
      v-show="gameOver"
      class="bg-blue-400 px-2 text-white rounded h-10 w-full"
      @click="resetState"
    >
      reset
    </button>
  </div>
</template>
