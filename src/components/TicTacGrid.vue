<template>
  <div :id="player" v-if="!reset">It's {{ player }}'s Turn.</div>
  <br />
  <div class="grid">
    <div class="row" v-for="(row, rowIndex) in 3" :key="rowIndex">
      <SquareTile
        :player="player"
        :reset="reset"
        :row="rowIndex"
        :col="colIndex"
        @changePlayer="alternate"
        @newGame="
          reset = false;
          player = 'X';
          turns = 0;
        "
        v-for="(col, colIndex) in 3"
        :key="`col-${colIndex}`"
      />
      <br />
    </div>
  </div>
  <div id="playStats">
    <div v-if="lastWinner != '' && lastWinner != 'Draw'">
      Game Over. {{ lastWinner }} Wins!
    </div>
    <div v-else-if="lastWinner == 'Draw'">Game Over. It's a Draw!</div>
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: 3;
  grid-template-rows: 3;
}
#X {
  color: red;
}
#O {
  color: greenyellow;
}
.row {
  display: flex;
  justify-content: center;
  align-items: center;
}
#playStats {
  font-size: 2rem;
  font-weight: bold;
  color: #ff0000;
  padding: 16px;
}
</style>

<script lang="ts" setup>
import SquareTile from "./SquareTile.vue";
import { ref } from "vue";

let player = ref("X");
let lastWinner = ref("");
let reset = ref(false);
let turns = ref(0);
const arr = ref([Array(3).fill(""), Array(3).fill(""), Array(3).fill("")]);

function alternate(row: number, col: number) {
  lastWinner.value = "";
  arr.value[row][col] = player.value;
  turns.value += 1;
  var winValue = checkWin(row, col);
  if (winValue != "") {
    // console.clear();
    reset.value = true;
    arr.value = [Array(3).fill(""), Array(3).fill(""), Array(3).fill("")];
    lastWinner.value = winValue;
  }
  if (player.value == "X") {
    player.value = "O";
  } else if (player.value == "O") {
    player.value = "X";
  }
}

function checkWin(row: number, col: number) {
  if (turns.value == 9) {
    return "Draw";
  }
  if (
    arr.value[row][0] == arr.value[row][1] &&
    arr.value[row][1] == arr.value[row][2] &&
    arr.value[row][0] != ""
  ) {
    return arr.value[row][0];
  }
  // vertical traverse
  if (
    arr.value[0][col] == arr.value[1][col] &&
    arr.value[1][col] == arr.value[2][col] &&
    arr.value[0][col] != ""
  ) {
    return arr.value[0][col];
  }
  // }
  // diagonal traverse
  if (
    (row == 0 && col == 0) ||
    (row == 0 && col == 2) ||
    (row == 2 && col == 0) ||
    (row == 1 && col == 1) ||
    (row == 2 && col == 0) ||
    (row == 2 && col == 2)
  ) {
    if (
      arr.value[0][0] == arr.value[1][1] &&
      arr.value[1][1] == arr.value[2][2] &&
      arr.value[0][0] != ""
    ) {
      return arr.value[0][0];
    }
    // reverse diagonal traverse
    if (
      arr.value[0][2] == arr.value[1][1] &&
      arr.value[1][1] == arr.value[2][0] &&
      arr.value[0][2] != ""
    ) {
      return arr.value[0][2];
    }
  }
  return "";
}
</script>
