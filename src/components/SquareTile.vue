<template>
  <div id="container" :class="classlist">
    <div class="center" @click="changeTic">
      {{ Tic }}
    </div>
  </div>
</template>

<style scoped>
#container {
  width: 100px;
  height: 100px;
  border: 1px solid black;
}
.center {
  width: 100px;
  height: 100px;
  font-size: 50px;
  align-items: center;
  justify-content: center;
  display: flex;
}
.default {
  background-color: blueviolet;
}
.X {
  background-color: red;
}
.O {
  background-color: greenyellow;
}
</style>

<script setup lang="ts">
import { ref, watch } from "vue";
import { defineProps } from "vue";
import { defineEmits } from "vue";

const props = defineProps({
  player: String,
  row: Number,
  col: Number,
  reset: Boolean,
});

let Tic = ref("");
let classlist = ref("");
classlist.value = "default";

const emit = defineEmits(["changePlayer", "newGame"]);

watch(
  () => props.reset,
  (newVal) => {
    if (newVal) {
      Tic.value = "";
      classlist.value = "default";
      emit("newGame");
    }
  }
);

function changeTic() {
  if (Tic.value == "") {
    Tic.value = props.player ?? "";
    if (props.player == "X") {
      classlist.value = "X";
    } else {
      classlist.value = "O";
    }
    emit("changePlayer", props.row, props.col);
  }
}
</script>
