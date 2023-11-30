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

<script lang="ts">
import SquareTile from "./SquareTile.vue";
import { defineComponent } from "vue";
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
  if (checkWin() != "") {
    // console.clear();
    reset.value = true;
    arr.value = [Array(3).fill(""), Array(3).fill(""), Array(3).fill("")];
    lastWinner.value = checkWin();
  }
  if (player.value == "X") {
    player.value = "O";
  } else if (player.value == "O") {
    player.value = "X";
  }
}

function checkWin() {
  console.log(turns.value);
  if (turns.value == 9) {
    console.log("Draw");
    return "Draw";
  }
  for (let i = 0; i < 3; i++) {
    // horizontal traverse
    if (
      arr.value[i][0] == arr.value[i][1] &&
      arr.value[i][1] == arr.value[i][2] &&
      arr.value[i][0] != ""
    ) {
      return arr.value[i][0];
    }
    // vertical traverse
    if (
      arr.value[0][i] == arr.value[1][i] &&
      arr.value[1][i] == arr.value[2][i] &&
      arr.value[0][i] != ""
    ) {
      return arr.value[0][i];
    }
  }
  // diagonal traverse
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
  return "";
}

export default defineComponent({
  name: "TicTacGrid",
  components: {
    SquareTile,
  },
  data() {
    return {
      player,
      reset,
      lastWinner,
      turns,
    };
  },
  methods: {
    alternate,
    checkWin,
  },
});
</script>
