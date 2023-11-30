<template>
  <header>
    <h1>Tic-Tac-Toe</h1>
  </header>
  <TicTacGrid />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TicTacGrid from "@/components/TicTacGrid.vue";

export default defineComponent({
  name: "TicTacView",
  components: {
    TicTacGrid,
  },
});
</script>
