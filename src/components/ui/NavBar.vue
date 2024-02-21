<script setup>
import LifesBar from "./LifesBar.vue";
import LevelIndicator from "./LevelIndicator.vue";
import GameOver from "./GameOver.vue";
import data_json from "../../assets/data.json";
import { ref, onMounted, watch } from "vue";

const props = defineProps({
  "level": Number,
  "mistakes": Number,
  "gameOver": Boolean
})

const emit = defineEmits(['setLevel', 'wait', 'reset']);

const lvl = ref(props.level)
const subtitles = data_json["indicator"]["subtitles"]
const details = data_json["indicator"]["details"]

onMounted(() => {
  if (!props.gameOver) {
    setButtonClasses() 
  }
})

watch(props, (newProps) => {
  lvl.value = newProps.level
})

watch(lvl, (newLvl) => {
  emit("setLevel", newLvl)
  setButtonClasses()
})

function setButtonClasses() {
  let btns = document.getElementsByClassName('navBtn')
  for (let i = 0; i < btns.length; i++) {
    btns[i].classList.remove("previousLevel")
    btns[i].classList.remove("currentLevel")
  }
  for (let id = -1; id < lvl.value; id++) {
    document.getElementById(id + 'LvlBtn').classList.add("previousLevel")
  }
  let cur = document.getElementById(lvl.value + 'LvlBtn')
  function showCurrent() {
    if (cur) {
      cur.classList.add("currentLevel")
    } else {
      setTimeout(showCurrent, 10);
    }
    showCurrent()
  }
}
</script>

<template>
  <div v-if="!props.gameOver">
    <LifesBar :level="lvl" :mistakes="props.mistakes" />
    <div id="navBar">
      <button id="-1LvlBtn" class="navBtn" @click="$emit('setLevel', -1)">
        <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 256 256">
          <path fill="currentColor"
            d="m218.83 103.77l-80-75.48a1.14 1.14 0 0 1-.11-.11a16 16 0 0 0-21.53 0l-.11.11l-79.91 75.48A16 16 0 0 0 32 115.55V208a16 16 0 0 0 16 16h48a16 16 0 0 0 16-16v-48h32v48a16 16 0 0 0 16 16h48a16 16 0 0 0 16-16v-92.45a16 16 0 0 0-5.17-11.78M208 208h-48v-48a16 16 0 0 0-16-16h-32a16 16 0 0 0-16 16v48H48v-92.45l.11-.1L128 40l79.9 75.43l.11.1Z" />
        </svg>
      </button>
      <button id="0LvlBtn" class="navBtn" @click="lvl = 0">0</button>
      <button id="1LvlBtn" class="navBtn" @click="lvl = 1">1</button>
      <button id="2LvlBtn" class="navBtn" @click="lvl = 2">2</button>
      <button id="3LvlBtn" class="navBtn" @click="lvl = 3">3</button>
      <button id="4LvlBtn" class="navBtn" @click="lvl = 4" v-if="lvl==4">
        <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 256 256">
          <path fill="currentColor"
            d="M36.08 43.37A6 6 0 0 0 34 47.9V216a6 6 0 0 0 12 0v-45.22c28.08-22.79 51.88-11 79.34 2.57c16.12 8 33.49 16.58 52 16.58c13.57 0 27.76-4.6 42.56-17.42A6 6 0 0 0 222 168V47.9a6 6 0 0 0-9.93-4.54c-29 25.12-53.28 13.09-81.41-.84c-27.89-13.81-59.66-29.36-94.58.85M210 165.17c-28.08 22.8-51.88 11-79.34-2.58C105.4 150.08 77.09 136.07 46 156V50.72c28.08-22.8 51.88-11 79.34 2.56C150.6 65.79 178.91 79.8 210 59.91Z" />
        </svg>
      </button>
    </div>
    <LevelIndicator v-if="lvl == 0" :level="lvl" :subtitle="subtitles[0]" :details="details[0]"
      @wait="(w) => $emit('wait', w)" />
    <LevelIndicator v-if="lvl == 1" :level="lvl" :subtitle="subtitles[1]" :details="details[1]"
      @wait="(w) => $emit('wait', w)" />
    <LevelIndicator v-if="lvl == 2" :level="lvl" :subtitle="subtitles[2]" :details="details[2]"
      @wait="(w) => $emit('wait', w)" />
    <LevelIndicator v-if="lvl == 3" :level="lvl" :subtitle="subtitles[3]" :details="details[3]"
      @wait="(w) => $emit('wait', w)" />
  </div>
  <GameOver v-else @reset="$emit('reset')"/>
</template>

<style scoped>
#navBar {
  margin: 25px 0 2rem;
  padding: 1rem;
  display: flex;
  justify-content: center;
}

button {
  width: 50px;
  height: 50px;
  font-size: 16px;
  background-color: var(--steel-blue);
  border: 0;
  border-radius: 15px;
  margin: 4px;
}

button:hover {
  width: 55px;
  height: 55px;
  font-size: 20px;
}

.previousLevel {
  background-color: var(--sea-green);
}

.currentLevel {
  width: 60px;
  height: 60px;
  font-size: 20px;
}

.currentLevel:hover {
  width: 60px;
  height: 60px;
}
</style>
