<script setup>
import { ref, watch } from 'vue'
import NavBar from "./components/ui/NavBar.vue"
import Startmenu from "./components/levels/Startmenu.vue"
import LevelZero from "./components/levels/LevelZero.vue"
import LevelOne from "./components/levels/LevelOne.vue"
import LevelTwo from "./components/levels/LevelTwo.vue"
import LevelThree from "./components/levels/LevelThree.vue"
import LevelFour from "./components/levels/LevelFour.vue"

const level = ref(-1)
const mistakes = ref(0)
const wait = ref(true)
const gameOver = ref(false)

watch(mistakes, (newMist) => {
  if (newMist < 0) {
    mistakes.value = 0
  }
  if (newMist == 5) {
    gameOver.value = true
  }
})

function reset() {
  level.value = -1
  mistakes.value = 0
  wait.value = true
  gameOver.value = false
}

</script>

<template>
  <div class="levelView">
    <Startmenu v-if="level == -1" @set-level="(l) => level = l" />
    <NavBar v-if="level != -1" :level="level" :mistakes="mistakes" :game-over="gameOver" @set-level="(l) => level = l"
      @wait="(w) => wait = w" @reset="reset()" />
    <div v-if="!gameOver">
      <LevelZero v-if="level == 0" @next-level="level++" :wait="wait" />
      <LevelOne v-if="level == 1" @update-mistakes="mistakes++" @next-level="level++" :wait="wait" />
      <LevelTwo v-if="level == 2" @update-mistakes="(m) => { if (m) { mistakes-- } else { mistakes++ } }"
        @next-level="level++" :wait="wait" />
      <LevelThree v-if="level == 3" @update-mistakes="(m) => { if (m) { mistakes-- } else { mistakes++ } }"
        @next-level="level++" :wait="wait" />
      <LevelFour v-if="level == 4" />
    </div>
  </div>
</template>

<style scoped>
.levelView {
  max-width: 650px;
  margin: 0 auto 50vh;
  padding: 0 2rem;
}
</style>
