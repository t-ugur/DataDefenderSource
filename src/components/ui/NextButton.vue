<script setup>

const props = defineProps({
  "disabled": Boolean,
});

defineEmits(["clicked"]);

</script>

<template>
  <button id="nextButton" @click="$emit('clicked')" :disabled="disabled">
    NEXT
  </button>
</template>

<style scoped>
#nextButton {
  position: fixed;
  bottom: 25px;
  right: 25px;
  width: 50px;
  height: 50px;
  border: 0;
  border-radius: 15px;
  color: var(--indigo-dark);
  background-color: var(--ice-blue);
}

#nextButton:hover {
  width: 55px;
  height: 55px;
  border-radius: 15px;
  font-weight: bold;
}
</style>
