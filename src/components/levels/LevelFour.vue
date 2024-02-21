<script setup>
import GigaBubble from "../ui/GigaBubble.vue"

const msg = `Hey! 
Congratulation for succesfully completing all of our session! <br/><br/> 
I have no doubt, that you will suit well as our Data Protection Officer. <br/><br/> 
Feel free to repeat any level, or go back to the starting menu.`
</script>

<template>
    <GigaBubble :message="msg" />
    <div id="credits">
        <span>Made by Ugur Turhan</span>
        <span>For the lecture: Serious Games for Cyber Security</span>
    </div>
</template>

<style scoped>
#credits {
    position: fixed;
    bottom: 25px;
    left: 0;
    width: 100%;
    font-weight: 100;
    font-size: 12px;
    display: flex;
    flex-direction: column;
    align-items: center;
}
</style>