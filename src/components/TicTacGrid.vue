<template>
  <div v-if="!reset">It's {{ player }}'s Turn.</div>
  <div v-else>{{ player }} Won</div>
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
        "
        v-for="(col, colIndex) in 3"
        :key="`col-${colIndex}`"
      />
      <br />
    </div>
  </div>
</template>

<style>
.grid {
  display: grid;
  grid-template-columns: 3;
  grid-template-rows: 3;
}
.row {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

<script lang="ts">
import SquareTile from "./SquareTile.vue";
import { defineComponent } from "vue";
import { ref } from "vue";

let player = ref("X");
let reset = ref(false);
const arr = ref([Array(3).fill(""), Array(3).fill(""), Array(3).fill("")]);

function alternate(row: any, col: any) {
  arr.value[row][col] = player.value;
  if (checkWin() != "") {
    console.clear();
    reset.value = true;
    arr.value = [Array(3).fill(""), Array(3).fill(""), Array(3).fill("")];
  }
  if (player.value == "X") {
    player.value = "O";
  } else if (player.value == "O") {
    player.value = "X";
  }
}

function checkWin() {
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
    };
  },
  methods: {
    alternate,
  },
});
</script>
