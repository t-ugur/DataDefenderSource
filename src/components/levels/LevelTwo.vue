<script setup>
import { ref, watch } from 'vue'
import GigaBubble from "../ui/GigaBubble.vue"
import NextButton from "../ui/NextButton.vue"
import NextLevelButton from "../ui/NextLevelButton.vue"
import Question from "../ui/Question.vue"
import data_json from "../../assets/data.json"

const textID = ref(-1)
const showNextButton = ref(false)
const data = data_json["levelTwo"]
const nData = Object.keys(data).length

const props = defineProps({
  "wait": Boolean
});

watch(props, (newProps) => {
  showNextButton.value = !newProps.wait
})

const emit = defineEmits(['updateMistakes', 'nextLevel'])

watch(textID, (newID) => {
  function showNewElement() {
    let el = document.getElementById(newID + 'giga')
    if (el) {
      el.scrollIntoView({ behavior: "smooth" })
    } else {
      setTimeout(showNewElement, 10);
    }
  }
  showNewElement()
})

</script>

<template>
  <div v-for="(d, i) in data">
    <GigaBubble v-if="d['type'] == 'message' && textID >= i" :id="i + 'giga'" :message="data[i]['msg']"
      @wait="(w) => showNextButton = !w" />
    <Question v-if="d['type'] == 'question' && textID >= i" :id="i + 'giga'" :data="data[i]" :a-id="i"
      @wait="(w) => showNextButton = !w" @answered="(a) => $emit('updateMistakes', a)" />
    <!-- <GigaBubble v-if="d['type'] == 'message'" :id="i + 'giga'" :message="data[i]['msg']"
      @wait="(w) => showNextButton = !w" />
    <Question v-if="d['type'] == 'question'" :id="i + 'giga'" :data="data[i]" :a-id="i" @wait="(w) => showNextButton = !w"
      @answered="(a) => $emit('updateMistakes', a)" /> -->
  </div>
  <NextButton v-if="showNextButton && textID < nData - 1" @clicked="textID++" />
  <NextLevelButton v-if="showNextButton && textID == nData - 1" @clicked="$emit('nextLevel')" />
</template>

<style scoped></style>