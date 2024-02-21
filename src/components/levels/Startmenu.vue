<script setup>
import { ref } from 'vue'

defineEmits(["setLevel"])

const showLevels = ref(false);

</script>

<template>
    <div class="title">
        <h1>DATA DEFENDER</h1>
        <p>PROTECT USER DATA AND AVOID GDPR VIOLATIONS</p>
    </div>
    <div class="buttons">
        <button class="mainBtn" @click="$emit('setLevel', 0)">
            Start Game
        </button>
        <button class="mainBtn" @click="showLevels = !showLevels">
            Select Level
        </button>
        <div class="levelButtons" v-show="showLevels">
            <button class="lvlBtn" @click="$emit('setLevel', 0)">Introduction</button>
            <button class="lvlBtn" @click="$emit('setLevel', 1)">Definitions</button>
            <button class="lvlBtn" @click="$emit('setLevel', 2)">Principles</button>
            <button class="lvlBtn" @click="$emit('setLevel', 3)">Rights</button>
        </div>
    </div>
</template>

<style scoped>
.title {
    height: 65vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.buttons {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.mainBtn {
    width: 65%;
    height: 50px;
    border: 0;
    border-radius: 15px;
    margin: 4px;
    font-size: 16px;
    background-color: var(--ice-blue);
}

.mainBtn:hover {
    font-weight: bold;
    width: 70%;
    box-shadow: 0 0 4px 1px var(--ice-blue);
}

.levelButtons {
    display: flex;
    flex-wrap: wrap;
    width: 65%;
}

.lvlBtn {
    flex: 1;
    margin: 4px;
    height: 25px;
    border: 0;
    border-radius: 8px;
    background-color: var(--ice-blue);
}

.lvlBtn:hover {
    flex: 2;
    font-weight: bold;
    box-shadow: 0 0 2px 1px var(--ice-blue);
}

h1 {
    font-size: 48px;
}

p {
    font-size: 15px;
}
</style>