<script setup>
import GigaBubble from "./GigaBubble.vue";
import data_json from "../../assets/data.json"
import { ref, onMounted } from 'vue'

const props = defineProps({
    "data": Object,
    "aId": String
})

const emit = defineEmits(['wait', 'answered'])

const trueComment = data_json["comments"]["true"]
const falseComment = data_json["comments"]["false"]

const showAnswers = ref(false)
const showInfo = ref(false)
const answered = ref(false)
const currentComment = ref("")
const currentTruth = ref(false)

var correctSound = new Audio('src/assets/correct.wav');
var wrongSound = new Audio('src/assets/wrong.mp3');

onMounted(() => {
    emit("wait", true)
})

function evalAnswer(a, i) {
    currentTruth.value = a.isCorrect
    if (a.comment) {
        currentComment.value = a.comment
    } else if (a.isCorrect) {
        let randIndx = Math.floor(Math.random() * trueComment.length)
        currentComment.value = trueComment[randIndx]
    } else {
        let randIndx = Math.floor(Math.random() * falseComment.length)
        currentComment.value = falseComment[randIndx]
    }
    if (!answered.value) {
        answered.value = true
        if (a.isCorrect && props.data.bonus) {
            emit("answered", true)
        } else if (!a.isCorrect) {
            emit("answered", false)
        }
    }
    if (a.isCorrect) {
        document.getElementById(props.aId + i + 'aBtn').classList.add("correctAnswerButton")
        emit("wait", false)
        correctSound.play();
        // emit("answered", true)

    } else {
        document.getElementById(props.aId + i + 'aBtn').classList.add("wrongAnswerButton")
        wrongSound.play();
    }
}

// shuffle answers into random order
const answers = props.data.answers
for (let i = answers.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [answers[i], answers[j]] = [answers[j], answers[i]];
}
</script>

<template>
    <div class="q-box">
        <div :class="props.data.bonus ? 'bonusRing' : ''">
            <GigaBubble :message="data.question" @wait="(w) => showAnswers = !w"
                :comment="{ 'cmt': currentComment, 'truth': currentTruth }" />
            <div v-if="showAnswers">
                <div id="upperQRow" class="q-row">
                    <button :id="props.aId + '0aBtn'" class="answerBtn" @click="evalAnswer(answers[0], 0)">
                        {{ answers[0].answer }}
                    </button>
                    <button :id="props.aId + '1aBtn'" class="answerBtn" @click="evalAnswer(answers[1], 1)">
                        {{ answers[1].answer }}
                    </button>
                </div>
                <div class="q-row">
                    <button :id="props.aId + '2aBtn'" class="answerBtn" @click="evalAnswer(answers[2], 2)">
                        {{ answers[2].answer }}
                    </button>
                    <button :id="props.aId + '3aBtn'" class="answerBtn" @click="evalAnswer(answers[3], 3)">
                        {{ answers[3].answer }}
                    </button>
                </div>
                <div v-if="answered" class="infoBox">
                    <button class="infoBtn" @click="showInfo = !showInfo">
                        <svg id="infosvg" xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                            <path fill="currentColor"
                                d="M13 9h-2V7h2m0 10h-2v-6h2m-1-9A10 10 0 0 0 2 12a10 10 0 0 0 10 10a10 10 0 0 0 10-10A10 10 0 0 0 12 2" />
                        </svg>
                    </button>
                    <div v-if="showInfo" class="info" v-html="data.info"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.bonusRing {
    padding: 8px;
    border-radius: 15px;
    box-shadow: 0px 0px 4px var(--steel-blue);
}

#upperQRow {
    margin-top: 4px;
}

#infosvg {
    font-size: 25px;
}

#infosvg:hover {
    font-size: 28px;
}

.infoBox {
    border: 2px;
    border-radius: 15px;
    border-style: solid;
    border-color: var(--steel-blue);
    min-height: 25px;
    margin: 4px;
    padding: 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.infoBtn {
    color: var(--steel-blue);
    background-color: var(--indigo-dark);
    border: 0;
}

.info {
    margin: 0 24px 16px;
    text-align: justify;
}

.answerBtn {
    width: 50%;
    min-height: 50px;
    margin: 4px;
    border: 0;
    border-radius: 16px;
    background-color: lightsteelblue;
    padding: 8px;
}

.answerBtn:hover {
    width: 75%;
    font-weight: bold;
}

.q-box {
    margin-top: 50px;
}

.q-row {
    display: flex;
}

.correctAnswerButton {
    background-color: darkseagreen;
}

.wrongAnswerButton {
    background-color: salmon;
}
</style>