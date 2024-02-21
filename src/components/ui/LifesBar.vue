<script setup>
import { computed } from "vue";

const props = defineProps({
    "mistakes": Number,
    "level": Number
})

const lifesBackground = computed(() => {
    return props.mistakes < 5 ? "var(--sea-green)" : "var(--salmon-red)"
})

</script>

<template>
    <div v-if="props.level > 0 && props.level < 4" id="lifesBar">
        <div id="lostLifes" :style="'flex: ' + props.mistakes"></div>
        <div id="lifes" :style="'background-color: ' + lifesBackground">
            <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                <path fill="currentColor"
                    d="m12 21.35l-1.45-1.32C5.4 15.36 2 12.27 2 8.5C2 5.41 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.08C13.09 3.81 14.76 3 16.5 3C19.58 3 22 5.41 22 8.5c0 3.77-3.4 6.86-8.55 11.53z" />
            </svg>
            <span>: {{ 5 - props.mistakes }}</span>
        </div>
        <div id="remainingLifes" :style="'flex: ' + (5 - props.mistakes)"></div>
    </div>
</template>

<style scoped>
#lifesBar {
    position: fixed;
    top: 0px;
    left: 0px;
    width: 100vw;
    display: flex;
    background-color: var(--indigo-dark);
}

#lostLifes {
    background-color: var(--salmon-red);
}

#remainingLifes {
    background-color: var(--sea-green);
}

#lifes {
    font-size: 10px;
}
</style>
