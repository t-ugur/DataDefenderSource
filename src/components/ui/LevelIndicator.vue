<script setup>
import { onMounted, onUnmounted } from "vue";

const props = defineProps({
    "level": Number,
    "subtitle": String,
    "details": Array
})

const emit = defineEmits(["wait"]);

var titleOpacity = 0
var subTitleOpacity = 0
var detailsOpacity = 0
var timeouts = []

onMounted(() => {
    fadeIndicator()
})

onUnmounted(() => {
    for (let i = 0; i < timeouts.length; i++) {
        clearTimeout(timeouts[i])
    }
})

function fadeIndicator() {
    emit('wait', true)
    document.getElementById(props.level + 'indTitle').style["opacity"] = 0
    document.getElementById(props.level + 'indSubtitle').style["opacity"] = 0
    document.getElementById(props.level + 'indDetails').style["opacity"] = 0
    fadeTitle()
    timeouts.push(setTimeout(fadeSubTitle, 750))
    timeouts.push(setTimeout(fadeDetails, 1500))
    timeouts.push(setTimeout(() => { emit('wait', false) }, 3000))
}

function fadeTitle() {
    if (titleOpacity < 1) {
        titleOpacity += 0.1
        document.getElementById(props.level + 'indTitle').style["opacity"] = titleOpacity
        timeouts.push(setTimeout(fadeTitle, 40))
    } else {
        document.getElementById(props.level + 'indTitle').style["opacity"] = 1
    }
}

function fadeSubTitle() {
    if (subTitleOpacity < 1) {
        subTitleOpacity += 0.01
        document.getElementById(props.level + 'indSubtitle').style["opacity"] = subTitleOpacity
        timeouts.push(setTimeout(fadeSubTitle, 5))
    } else {
        document.getElementById(props.level + 'indSubtitle').style["opacity"] = 1
    }
}

function fadeDetails() {
    if (detailsOpacity < 1) {
        detailsOpacity += 0.01
        document.getElementById(props.level + 'indDetails').style["opacity"] = detailsOpacity
        timeouts.push(setTimeout(fadeDetails, 20))
    } else {
        document.getElementById(props.level + 'indDetails').style["opacity"] = 1
    }
}
</script>

<template>
    <div class="indicator">
        <span :id="props.level + 'indTitle'" class="indicatorTitle">Level {{ props.level }}</span>
        <span :id="props.level + 'indSubtitle'" class="indicatorSubtitle">{{ props.subtitle }}</span>
        <div :id="props.level + 'indDetails'" class="indicatorDetailsBox">
            <span v-for="d in props.details" class="indicatorDetail" v-html="d"></span>
        </div>
    </div>
</template>

<style scoped>
.indicator {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 50px;
}

.indicatorTitle {
    font-size: 64px;
    font-family: 'Brush Script MT';
}

.indicatorSubtitle {
    font-size: 22px;
    margin-bottom: 16px;
    text-align: center;
    font-weight: 400;
}

.indicatorDetailsBox {
    display: flex;
    flex-direction: column;
    max-width: 75%;
    font-weight: 100;
    /* font-size: 16px; */
}

.indicatorDetail {
    margin: 5px;
}
</style>
