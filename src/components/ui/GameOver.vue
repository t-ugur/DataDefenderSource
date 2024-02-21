<script setup>
const emit = defineEmits(['reset']);
</script>

<template>
    <div id="gameOver">
        <h1 id="goHeader">Game Over</h1>
        <span class="goInfo">Oh no! But don't worry. Next time you can do it.</span>
        <span class="goInfo">Maybe try to read the additional information.</span>
        <button id="goBtn" @click="$emit('reset')">Continue</button>
    </div>
</template>

<style scoped>
#gameOver {
    display: flex;
    flex-direction: column;
    align-items: center;
}

#goHeader {
    font-size: 48px;
    font-weight: 900;
    margin: 100px 0;
    color: var(--salmon-red);
}

.goInfo {
    font-size: 16px;
    font-weight: 600;
    margin: 8px 0;
}

#goBtn {
    margin-top: 100px;
    height: 50px;
    width: 50%;
    border: 0;
    border-radius: 15px;
    font-size: 16px;
    font-weight: bold;
    color: var(--indigo-dark);
}

#goBtn:hover {
    background-color: var(--steel-blue);
    font-size: 18px;
    height: 55px;
    width: 55%;
}
</style>
