<script setup>

defineEmits(["clicked"]);

</script>

<template>
    <button id="nextLevel" @click="$emit('clicked')">
        NEXT LEVEL
    </button>
</template>

<style scoped>
#nextLevel {
    height: 50px; 
    width: 100%;
    font-size: 16px;
    font-weight: bold;
    border: 0;
    border-radius: 15px;
    color: var(--indigo-dark);
    background-color: var(--sea-green);
    margin-top: 100px;
    position: sticky;
    bottom: 25px;
}

#nextLevel:hover {
    font-size: 18px;
}
</style>
